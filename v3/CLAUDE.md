# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Business flows — non-negotiable rules

Read `docs/MODELL-ROUTINEN-UND-PROZESSE.md` (the laws are its section 8)
**before** wiring any recurring business flow — whether or not a digital
employee is involved. A purely human approval chain is covered by these rules
too.

**Three building blocks, two of them are flows:**

- **Arbeitsquelle** — a Principal's *perception*. Reads a mailbox, mirrors a
  calendar, pulls leads. Decides nothing. It is **the only thing a Routine may
  use to start Prozess runs** — a Routine never starts one directly (several
  per event is fine). A Prozess run may also be started by a business event
  from outside: form, webhook, API call, script, schedule, CRM action or manual
  start. **Every Prozess run records who or what started it** (principal +
  channel); starting grants no authority (§4). *(Changed 2026-09-23 by the
  owner: the former "only thing allowed to start Prozess runs" contradicted
  the model paper §7.3 and the code; the real risk is an invisible start, and
  the answer is provenance, not a ban — see `docs/PLAN-OEFFENTLICHE-API-V2.md`
  §1.5 and E5.)*
- **Routine** — a digital employee's *skill*. He can do it, he does it, it
  inherits his authority. Owned through the binding in `employee_capabilities`,
  lives in the Personalakte. If he goes, you rebind it. *(Humans have no
  routines — `employee_capabilities` is the only capability registry.)*
- **Prozess** — the organisation's *obligation*. It must run — on time,
  provably, whoever is available. Carries a cast, deadlines, substitution,
  escalation, and a **named process owner (a required field, not runtime
  behaviour)**. May have a single role.

**The test — ownership, not headcount:**

0. **No predetermined sequence?** → **Vorgang** (open responsibility:
   `assignments` + worklist). Stop here, this is not a flow.
1. **Do several Principals execute steps?** → **Prozess**. Do *not* count
   whoever merely decides (an approver, even one who corrects values) or
   whoever merely rescues (escalation path).
2. **Only one?** Must it survive that Principal leaving, **or** is its work
   object declared audit-bound? Either → **Prozess** (single-role). Neither →
   **Routine**.

Audit duty (`nachweispflichtig`) is a property of the **work object**, declared
once by the business side — not a question you answer while building. Every
flow touching an audit-bound object writes into that object's chain.

**When in doubt, Prozess.** The two mistakes are not symmetric: a needless
Prozess costs some cast and deadlines and is visible; a Routine that should have
been a Prozess fails *silently* — a parked run with no deadline and no
substitute waits forever. Do not argue "a Prozess is more expensive": deadlines,
substitution and audit trail are what it delivers, not overhead around it.

**A Prozess step hands work over and waits.** It never calls a participant
synchronously — that would turn a colleague into a function, without his own
cadence, prioritisation or working hours. This is also why a Prozess cannot
launder authority: it queues, so the work necessarily passes the actor's gate
chain.

### The four laws

**§1 — Know who owns the flow.** Classify before you build (Vorgang, Routine,
Prozess) and write the classification down. Whoever cannot classify it has not
understood it.

**§2 — A process is configuration, not code.** Never add a business flow as a
new branch in `EmployeeExecutor::executeAction()`. That `match` is for tools and
**data fetching** only. Measurable: the number of arms executing a *business
flow* must not grow — today eight arms on seven handlers (2026-08-18).

**§3 — A flow addresses participants, not IDs.** Resolve through `principals`.
A raw `user_id`, `employee_id` or `agent_id` in a flow definition is a bug, not
a shortcut — the flow breaks the day that person leaves.

**§4 — A flow grants no authority.** Every step runs through the gate chain of
*its* actor. A step with no resolvable actor escalates to the process owner; it
does not run. *(The former sentence "four eyes is strict — anyone who executed a
step in this case is barred from approving it" was struck on 2026-09-10 by the
owner: for five-person customers it made the calculator unable to approve and
escalated to the same person. Do not reintroduce it as a hidden default; a
process that wants separated roles declares `verschieden_von` explicitly.)*

**Upgrade, never patch.** When a Routine gains a second actor, a deadline or an
audit duty, upgrade it to a Prozess — never wire a fixed person into it. That
patch looks harmless in a diff and is the most likely way this model gets
broken.

**Naming.** "Workflow" is a pure technical term (engine, graph, tables, class
names). Anything a user or customer reads says **Routine** or **Prozess** —
including both language files under `resources/lang/`.

If a task appears to require breaking one of these, say so and stop — do not
merge around it. The rules exist because no single session sees that it is
doing this for the eighth time.

## Commands

```bash
# PHP dependencies
composer install

# Frontend dependencies & build
npm install
npm run dev          # development build (watch: npm run watch)
npm run production   # production build

# Laravel
php artisan serve    # local dev server
php artisan migrate  # run migrations
php artisan test     # run all tests
composer test-parallel   # all tests on 8 processes (~4 min instead of ~17); workers need the -d flags, the script passes them
./vendor/bin/phpunit --filter TestName   # run single test

# Queue (required for async jobs)
php artisan horizon
```

## Architecture

**Stack:** Laravel 9 (PHP 8.2) backend · Vue 2.7 + Vuetify 2.7 + Bootstrap 4 frontend · MySQL + Redis · Laravel Mix/Webpack 5.

The app is a SPA delivered through Blade — `resources/views/app.blade.php` boots the Vue root, and Vue Router handles client-side navigation. Blade is only used for the shell layout; all UI is Vue components.

**Frontend component conventions:** components live in `resources/js/components/<Module>/`. Each module typically has a root `*Component.vue` that orchestrates child step/section components. Vuetify v-components (`v-card`, `v-btn`, `v-data-table`) are the primary UI primitives — do not introduce new design systems.

**Service layer:** business logic lives in `app/Services/`. Controllers are thin and delegate to services. Key services:
- `AnthropicService` — AI text generation (currently wired to OpenAI `gpt-4o` via `OPENAI_API_KEY`; config key is `services.openai`)
- `SerpApiClient` — Google search via SerpAPI
- `RssFeedClient` — parallel RSS fetching (Guzzle promises)
- `ActivepiecesPublisher` — sends posts to platform-specific Activepieces webhooks for publishing
- `CrmAdapters/` — Salesforce and other CRM backends behind a common adapter interface

**External integrations (env-driven):**
- `OPENAI_API_KEY` — AI text generation
- `SERPAPI_API_KEY` — source search
- `WORKFLOW_SOCIAL_WEBHOOK_*` — per-platform Activepieces webhook URLs; numbered variants (`…_LINKEDIN_1`) drive the multi-account setup, see `docs/SOCIAL-MEDIA-ACCOUNTS.md`
- `LINKEDIN_ACCOUNTS` / `LINKEDIN_1` / `X_ACCOUNTS` / `X_1` / `FACEBOOK_*` — multiple accounts per platform (name + numbered webhook per account)
- `WORKFLOW_SOCIAL_CALLBACK_TOKEN` — token for the `POST /social-media/webhooks/posted/{id}` callback
- `SOCIAL_MEDIA_RSS_FEEDS` — comma-separated RSS URLs
- Salesforce via `omniphx/forrest` (config: `config/forrest.php`)
- Real-time: Pusher + Laravel Echo (config: `PUSHER_APP_*`)

## Social Media Module

Routes: `routes/web.php` under the `/social-media` prefix (auth-protected except the webhook callback).

**Flow:** 4-step Vue wizard (`SocialMediaWorkflowComponent.vue`) →
1. Topic + source search (SerpAPI / RSS)
2. Account/tone/language selection → one AI call per platform, one post per selected account (`social_media_posts.account_key`)
3. Image/graphic editor (canvas-based)
4. Kanban board (`draft → ready → scheduled → posted`)

**Models:** `SocialMediaPost` (soft deletes, statuses: draft/ready/scheduled/posted) and `SocialMediaSource` (search result cache), both scoped to `user_id`.

**Platform config:** `config/social_media.php` defines char limits, image sizes, supported tones/languages, and the system prompts fed to the AI. The system prompt per platform is stable and should remain long for effective caching.

**Publishing:** `ActivepiecesPublisher` POSTs to an external webhook; Activepieces calls back `POST /social-media/webhooks/posted/{id}?token=…` to mark the post as `posted`.

## CI/CD & Deployment

GitHub Actions (`build.yml`) builds a Docker image, pushes to GHCR, and triggers a Portainer stack redeploy via webhook. The `docker/php-apache/Dockerfile` is the production image. Do not bypass the CI pipeline for production deployments.

## UI and themes

Before any UI change in echofront layouts, load the `echofront-theme` skill
(`.claude/skills/echofront-theme/SKILL.md`), or read
`resources/themes/echofront/DESIGN.md`, `resources/themes/echofront/KOMPONENTEN.md`
and `resources/themes/README.md` directly. The design system is binding: tokens
only (no hex values, no gradients, no new colors), `ef-` building blocks from
`resources/themes/echofront/theme.scss` first, ask instead of inventing.

KOMPONENTEN.md is the catalog of every UI element (buttons, fields, selects,
checkboxes, switches, search, forms and validation, dialogs, delete
confirmation, toasts and banners, tooltips, tables, pagination, badges, KPI
tiles, loading states, mobile rules). Read the section of each element you
build or change. Never use `window.confirm`, `window.alert` or `window.prompt`;
use `this.$ui.confirm` / `this.$ui.success` etc. and `<ef-dialog>`. Every view
must work from 375px width.

Icons: one set (Lucide). Use only `<ef-icon name="...">` with names from
`resources/js/icons.js`, rules in `resources/themes/echofront/ICONS.md`. Never
add `mdi-*`, `fa-*` or emojis; `tests/js/icon-satz.js` fails if their count
grows. A missing meaning is added to `icons.js` after asking, never solved
with another icon.

Themes live in `resources/themes/<name>/` (`DESIGN.md` + `theme.scss`), are
imported by `resources/sass/app.scss` and scoped under
`[data-ef-theme="<name>"]`. A theme only applies where a layout sets that
attribute on its root. The app already uses `ef-btn`, `ef-table`, `ef-card`
etc. with its own look, so only enable the theme on views built (or rebuilt)
entirely from the theme's building blocks.

## i18n

Translation files: `resources/lang/de/` and `resources/lang/en/`. The app supports German and English. When adding user-facing strings, add keys to both language files.
