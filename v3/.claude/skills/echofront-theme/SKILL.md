---
name: echofront-theme
description: Use before any UI change in echofront layouts (Vue components, Blade views, CSS/SCSS, Sales Hubs, website, slides, documents, charts, icons, user-facing texts, dialogs, forms, tables, notifications). Loads the echofront design system from resources/themes/echofront/DESIGN.md and KOMPONENTEN.md and the theme building blocks (ef- classes, tokens) under [data-ef-theme="echofront"].
---

# echofront Theme

1. Lies `resources/themes/echofront/DESIGN.md` vollstaendig. Sie ist verbindlich.
2. Lies in `resources/themes/echofront/KOMPONENTEN.md` den Abschnitt jedes
   Elements, das du baust oder aenderst (Dialog, Formular, Tabelle, Meldung,
   Badge ...). Er ist verbindlich, auch fuer das Verhalten auf dem Handy.
3. Lies `resources/themes/README.md` (Einschalten, Kollision mit alten Klassen).
4. Bausteine und Tokens stehen in `resources/themes/echofront/theme.scss`.
5. Icons: Lies `resources/themes/echofront/ICONS.md` und nimm Namen nur aus
   `resources/js/icons.js` (`<ef-icon name="...">`). Nie `mdi-*`, `fa-*`,
   Emojis oder direkte Lucide-Importe. Fehlt eine Bedeutung: nachfragen.

Regeln beim Bauen:

- Farben, Abstaende, Radien, Schatten nur ueber die Theme-Variablen
  (`var(--navy)`, `var(--space-5)`, ...). Keine Hex-Werte, keine Verlaeufe,
  keine neuen Farben im Komponenten-Code.
- Zuerst die `ef-`-Bausteine aus `theme.scss` verwenden, erst danach eigene
  Styles. Fehlt ein Baustein oder passt die Vorgabe nicht: nachfragen statt
  erfinden.
- Die Ansicht muss im Theme liegen: Wurzelknoten mit
  `data-ef-theme="echofront"`. Nicht ueber alte Ansichten stuelpen, die
  `ef-btn--primary` und Co. verwenden; die werden zuerst umgebaut.
- Vuetify bleibt Grundlage (CLAUDE.md). Vuetify-Komponenten im Theme an die
  Tokens anpassen, kein zweites Design-System einfuehren.
- Kein `window.confirm`, `window.alert` oder `window.prompt`. Bestaetigungen
  ueber `this.$ui.confirm`, Rueckmeldungen ueber `this.$ui.success` und Co.
  (KOMPONENTEN.md, Abschnitte 10 und 11). Dialoge ueber `<ef-dialog>`.
- Jede Ansicht funktioniert ab 375px Breite. Pruefen bei 375, 768 und 1280px.
- Texte: Deutsch mit "Sie", keine Gedankenstriche, Trenner " | ". Neue Texte
  in beide Sprachdateien unter `resources/lang/`.
- Nach Frontend-Aenderungen `npm run production` und die gebauten Dateien
  mit committen.
