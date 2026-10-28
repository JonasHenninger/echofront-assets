# echofront Komponentenkatalog

Der Katalog legt für jedes UI-Element fest, welcher Baustein verwendet wird, wie er aussieht und wie er sich verhält, auf dem Desktop und auf dem Handy. Er gilt zusammen mit `DESIGN.md` (Grundsätze, Farben, Typografie) und `ICONS.md` (Icons).

**Arbeitsregeln für Entwicklung und KI**
- Vor jeder UI-Änderung den passenden Abschnitt hier lesen. Was hier steht, ist verbindlich.
- Erst den genannten Baustein verwenden. Eigene Klassen, eigene Farben oder ein zweites Muster für dieselbe Aufgabe entstehen nicht.
- Fehlt ein Element oder passt die Vorgabe nicht: nachfragen, dann hier ergänzen. Nicht im Komponenten-Code lösen.
- Jeder Abschnitt nennt unter **Bestand**, was es heute schon gibt und was beim nächsten Umbau angepasst wird.

**Inhalt**
1. Grundregeln für alle Bausteine
2. Buttons
3. Textfelder und Textareas
4. Dropdowns und Selects
5. Checkboxen und Radio-Buttons
6. Switches
7. Suchfelder
8. Formulare und Validierung
9. Dialoge
10. Bestätigung vor dem Löschen
11. Meldungen und Banner
12. Tooltips und Hilfetexte
13. Tabellen, Filter und Sortierung
14. Pagination
15. Badges und Status
16. Kennzahlen-Kacheln und große Zahlen
17. Ladeanzeigen und Fortschritt
18. Mobil: Regeln für alle Ansichten

---

## 1. Grundregeln für alle Bausteine

**Höhen und Raster**
- Bedienelemente sind 36px hoch (Buttons, Textfelder, Selects, Suchfeld). Die kleine Variante ist 28px und nur in Tabellenzeilen und Werkzeugleisten erlaubt.
- Auf Touch-Geräten ist jede Klickfläche mindestens 40 × 40px, auch wenn das Icon kleiner ist.
- Abstände nur aus `space-1` bis `space-7`. Zwischen Feldern eines Formulars `space-4`, zwischen Abschnitten `space-6`.
- Radien: `radius-sm` für Felder, Badges und Chips, `radius-md` für Buttons, Karten, Menüs und Dialoge.

**Zustände** (für jedes Bedienelement gleich)

| Zustand | Aussehen |
| --- | --- |
| Standard | wie im jeweiligen Abschnitt beschrieben |
| Hover | Fläche eine Stufe dunkler (`surface` statt `surface-raised`), nie eine andere Farbe |
| Fokus | Rahmen 2px in `focus` mit 2px Abstand, nur bei Tastaturbedienung (`:focus-visible`) |
| Deaktiviert | Fläche `surface`, Rand `border`, Schrift und Icon `ink-muted`, Zeiger `not-allowed`, kein Hover. Gilt für jede Button-Variante. Der Grund steht daneben (siehe Formulare). Nie ausgegraute Farbe mit weißer Schrift |
| Fehler | Rahmen in `danger`, darunter die Meldung (siehe Formulare) |
| Lädt | siehe Ladeanzeigen |

**Breakpoints** (dieselben Schwellen wie Vuetify, `tailwind.config.js` und `$bp` aus `efBreakpoint`)

| Name | Breite | Gerät |
| --- | --- | --- |
| `xs` | unter 600px | Handy |
| `sm` | 600 bis 959px | Tablet hochkant |
| `md` | 960 bis 1263px | Tablet quer, kleines Notebook |
| `lg` | ab 1264px | Desktop |

Getestet wird jede Ansicht bei 375px, 768px und 1280px Breite.

**Texte**
- Beschriftungen sagen, was passiert oder was eingetragen wird: „Angebot senden“, „E-Mail-Adresse“. Nie „OK“, „Ja“, „Absenden“.
- Alle Texte über `$t()`, Schlüssel in beiden Sprachdateien.

---

## 2. Buttons

**Baustein:** `ef-btn` mit Varianten, `ef-icon-btn` für Icons ohne Text. Native `<button>`, kein `v-btn`.

| Variante | Klasse | Wofür |
| --- | --- | --- |
| Primär | `ef-btn ef-btn-primary` | Die eine Hauptaktion einer Ansicht oder eines Dialogs. Höchstens einmal sichtbar |
| Sekundär | `ef-btn` | Weitere Aktionen: Exportieren, Filtern, Bearbeiten |
| Ghost | `ef-btn ef-btn-ghost` | Abbrechen, Zurück, untergeordnete Aktionen |
| Akzent | `ef-btn ef-btn-accent` | Der eine Schritt nach außen in Sales Hub und Website (Demo vereinbaren) |
| Gefahr | `ef-btn ef-btn-danger` | Nur im Bestätigungsdialog für Löschen und andere endgültige Aktionen |
| Klein | zusätzlich `ef-btn-sm` | In Tabellenzeilen und Werkzeugleisten |
| Nur Icon | `ef-icon-btn` | Aktionen in Zeilen und Karten. Immer mit `aria-label` und Tooltip |

**Regeln**
- Reihenfolge rechts ausgerichtet: Ghost, Sekundär, Primär. Die Hauptaktion steht immer ganz rechts.
- Primärbutton beim Anlegen mit Icon `add`, beim Speichern ohne Icon.
- Löschen ist nie ein sichtbarer Button neben Speichern. Es steht im Menü „Weitere“ (`more`) und führt in den Bestätigungsdialog.
- Während eine Aktion läuft: Button deaktiviert, Icon durch `loading` mit `spin` ersetzt, Beschriftung bleibt.

**Mobil:** In Dialogen und Formularen unter 600px stehen Buttons untereinander in voller Breite: oben der Hinweis, darunter die Hauptaktion, darunter Abbrechen.

**Bestand:** Die App nutzt außerhalb des Themes `ef-btn--primary`, `ef-btn--ghost`, `ef-btn--danger` und `ef-iconbtn` (BEM, `app.scss`). Beim Umbau einer Ansicht auf das Theme werden sie durch die Klassen oben ersetzt.

---

## 3. Textfelder und Textareas

**Baustein:** `ef-field` als Umschlag, darin `ef-label`, `ef-input` oder `ef-textarea`, darunter `ef-field-hint` oder `ef-field-error`.

```html
<div class="ef-field">
    <label class="ef-label" for="opp-name">Bezeichnung <span class="ef-req" aria-hidden="true">*</span></label>
    <input id="opp-name" class="ef-input" v-model="name" :aria-invalid="touched && !name ? 'true' : null"
           aria-describedby="opp-name-msg" @blur="touched = true">
    <span v-if="touched && !name" id="opp-name-msg" class="ef-field-error">
        <ef-icon name="error" /> Bitte geben Sie eine Bezeichnung ein.
    </span>
</div>
```

**Aussehen**
- Höhe 36px, Rahmen 1px `border-strong`, Fläche `surface-raised`, Text `body` in `ink`, `radius-sm`.
- Platzhalter in `ink-muted`, nur als Beispiel („z. B. Muster GmbH“), nie anstelle der Beschriftung.
- Beschriftung immer über dem Feld in 13px, 500, `ink`. Keine schwebenden Vuetify-Labels.
- Fehler: `aria-invalid="true"` am Feld färbt den Rahmen `danger`, Meldung darunter in 12px `danger` mit Icon `error`.

**Textarea:** mindestens drei Zeilen, wächst mit bis zehn Zeilen, danach scrollt sie. Bei einer Zeichengrenze steht rechts darunter „120 von 500 Zeichen“ in `caption`.

**Pflicht und optional:** Pro Formular gilt eine der beiden Kennzeichnungen. Sind die meisten Felder Pflicht, bekommen die übrigen den Zusatz „optional“ (`ef-label-tag`). Sind die meisten optional, bekommen Pflichtfelder ein `*` in `danger` (`ef-req`). Nie beides in einem Formular.

**Mobil:** Felder immer volle Breite. Passender `type` und `inputmode` (`email`, `tel`, `numeric`), damit die richtige Tastatur erscheint. Schrift in Feldern mindestens 16px unter 600px, sonst zoomt iOS beim Antippen.

**Bestand:** `ef-field`, `ef-label`, `ef-input`, `ef-textarea`, `ef-field-hint`, `ef-field-error` gibt es in `app.scss` schon, dort mit `--ef-*`-Tokens und `ef-input--error`. Im Theme gelten die Regeln aus `theme.scss`, Fehler über `aria-invalid` statt Modifikator.

---

## 4. Dropdowns und Selects

**Baustein:** `v-select` mit `class="ef-vselect"`, ab zehn Einträgen `v-autocomplete` mit derselben Klasse. Nie ein natives `<select>`.

```html
<div class="ef-field">
    <label class="ef-label" for="opp-stage">Verkaufsphase</label>
    <v-select id="opp-stage" v-model="stage" :items="stages" class="ef-vselect"
              placeholder="Bitte wählen" outlined dense hide-details />
</div>
```

**Regeln**
- Beschriftung als `ef-label` darüber, `hide-details` ist Pflicht.
- Bis fünf Optionen, die man vergleichen muss: Radio-Buttons statt Select.
- Mehrfachauswahl zeigt höchstens zwei Werte als Chip, danach „+3“.
- Die Liste öffnet unter dem Feld mit `shadow-overlay` und `radius-md`, der gewählte Eintrag mit Icon `confirm`.
- Leere Auswahl heißt „Bitte wählen“, nicht „--“ oder „Auswählen...“.

**Mobil:** Volle Breite. Die Liste darf über die ganze Bildschirmbreite gehen.

**Bestand:** `ef-vselect` gibt es in `app.scss` (siehe `docs/DIALOG-STYLE-GUIDE.md`). Im Theme wird sie über `theme.scss` an die Tokens angepasst.

---

## 5. Checkboxen und Radio-Buttons

**Baustein:** `ef-check` (Checkbox) und `ef-radio` (Radio), jeweils als `<label>` um das native `<input>`, damit die ganze Zeile klickbar ist.

```html
<label class="ef-check">
    <input type="checkbox" v-model="notify">
    <span>Zuständige per E-Mail informieren</span>
</label>
```

**Wofür**
- **Checkbox:** unabhängige Ja/Nein-Optionen und Mehrfachauswahl. Wirkt erst mit „Speichern“.
- **Radio:** genau eine aus zwei bis fünf Optionen. Eine ist immer vorausgewählt.
- **Mehr als fünf Optionen:** Select.
- **Option mit Erklärung:** Auswahlkarte (Muster `.stage-type` aus `CrmCreateStageComponent.vue`), Erklärung in `caption` unter dem Namen.

**Aussehen:** Kästchen 16px, Rahmen `border-strong`, angehakt Fläche `navy` mit weißem Haken. Radio rund, gewählt mit Punkt in `navy`. Gruppen haben eine Überschrift als `ef-label` und stehen untereinander, nicht nebeneinander.

**Mobil:** Zeilenhöhe mindestens 40px.

---

## 6. Switches

**Baustein:** `ef-switch` (natives `<input type="checkbox" role="switch">` im `<label>`).

```html
<label class="ef-switch">
    <input type="checkbox" role="switch" v-model="active" @change="saveActive">
    <span class="ef-switch-track" aria-hidden="true"></span>
    <span>Provider aktiv</span>
</label>
```

**Regel:** Ein Switch wirkt sofort, ohne Speichern. Danach kommt eine kurze Meldung („Provider aktiviert.“). Gehört die Einstellung zu einem Formular mit Speichern-Button, ist es eine Checkbox.

**Aussehen:** Spur 36 × 20px, aus in `border-strong`, an in `navy`, Knopf weiß. Beschriftung rechts daneben. Der Zustand steht in der Beschriftung, nicht als zusätzliches „An“/„Aus“.

**Nicht:** Switches im Seitenkopf als Ersatz für einen Status. Der Status steht als Badge neben dem Titel (siehe `DESIGN.md`, Seitenkopf der Detailansicht), umgeschaltet wird im Inhalt.

---

## 7. Suchfelder

**Baustein:** `ef-search`.

```html
<div class="ef-search">
    <ef-icon name="search" />
    <input type="search" v-model="query" placeholder="Name, Firma oder E-Mail" aria-label="Suchen">
    <button v-if="query" type="button" class="ef-icon-btn ef-btn-sm" aria-label="Suche leeren" @click="query = ''">
        <ef-icon name="close" />
    </button>
</div>
```

**Regeln**
- Höhe 36px, mindestens 280px breit. Der Platzhalter passt immer ganz hinein, sonst wird er gekürzt, nicht das Feld schmaler.
- Platzhalter nennt, wonach gesucht wird, ohne „Suchen nach...“ davor.
- Sucht beim Tippen nach 300ms Pause, kein Button „Suchen“.
- In Tabellen steht die Suche in derselben Zeile wie die gespeicherten Ansichten (siehe Tabellen).

**Mobil:** Volle Breite in eigener Zeile über der Liste.

---

## 8. Formulare und Validierung

**Aufbau**
- Eine Spalte. Zwei Spalten (`ef-grid-2`) nur für kurze, zusammengehörige Felder: PLZ und Ort, Vorname und Nachname. Unter 640px immer eine Spalte.
- Reihenfolge wie im Arbeitsablauf, wichtigste Felder oben. Lange Formulare in Abschnitte mit `section-title` gliedern.

**Prüfen**
- Ein Feld wird nach dem Verlassen geprüft (`@blur`), nicht beim ersten Tastendruck.
- Beim Speichern werden alle Felder geprüft. Der Fokus springt auf das erste fehlerhafte Feld, die Ansicht scrollt dorthin.
- Fehlermeldungen sagen, was fehlt und wie es richtig ist: „Bitte geben Sie eine gültige E-Mail-Adresse ein, z. B. name@firma.de.“ Nicht „Ungültige Eingabe“.
- Fehler vom Server zu einem Feld stehen am Feld. Fehler ohne Feldbezug stehen als Banner `danger` oben im Formular oder Dialog.

**Speichern-Button**
- Bleibt sichtbar. Ist das Formular unvollständig, ist er deaktiviert, und der Grund steht links daneben in `caption` und `ink-muted`: „Bitte füllen Sie die Bezeichnung aus.“
- Nach dem Speichern: Meldung „Änderungen gespeichert.“ (siehe Meldungen).

**Ungespeicherte Änderungen:** Wer ein geändertes Formular oder einen Dialog verlässt, wird gefragt: Titel „Änderungen verwerfen?“, Buttons „Weiter bearbeiten“ (Ghost) und „Verwerfen“ (Gefahr).

---

## 9. Dialoge

**Baustein:** `<ef-dialog>` (`resources/js/components/ui/EfDialog.vue`) für jeden Dialog. Er bringt Überlagerung, Breite, Vollbild auf dem Handy und Scrollen mit. Innen gelten die Klassen `ef-dialog-head`, `ef-dialog-body` und `ef-dialog-foot` aus `theme.scss`.

```vue
<ef-dialog v-model="open" title="Verkaufschance anlegen" :max-width="640" persistent>
    <div class="ef-stack">…Felder…</div>
    <template #actions>
        <span class="ef-dialog-note">{{ hint }}</span>
        <button type="button" class="ef-btn ef-btn-ghost" @click="open = false">Abbrechen</button>
        <button type="button" class="ef-btn ef-btn-primary" :disabled="!valid" @click="save">
            <ef-icon name="add" /> Anlegen
        </button>
    </template>
</ef-dialog>
```

**Aufbau (immer gleich)**

| Zone | Inhalt |
| --- | --- |
| Kopf | Titel in `section-title` (16px, 600, `ink`), optional eine Zeile Untertitel in `ink-muted`. Rechts `ef-icon-btn` mit Icon `close`. Linie `border` darunter. Steht fest |
| Inhalt | Innenabstand `space-5`, scrollt allein |
| Fuß | Links der Hinweis (`ef-dialog-note`), rechts die Buttons: Ghost, dann Primär. Linie `border` darüber. Steht fest |

Fläche `surface-raised`, `radius-md`, Schatten `shadow-overlay`. Hintergrund abgedunkelt mit `navy` bei 40 % Deckkraft. Kein Verlauf, keine Icon-Kachel im Kopf, kein farbiger Kopf.

**Breiten**

| Stufe | Breite | Wofür |
| --- | --- | --- |
| S | 480px | Bestätigungen, ein Feld |
| M | 640px | Formulare (Standard) |
| L | 880px | Zweispaltige Formulare, Tabellen, Vorschauen |

Größer wird ein Dialog nicht. Wer mehr Platz braucht, baut eine eigene Ansicht oder ein Seitenpanel (`EfDrawer`).

**Verhalten**
- Titel sagt, was passiert: „Kontakt bearbeiten“, nicht „Bearbeiten“.
- Fokus ins erste Feld. Esc und das Kreuz schließen, bei Änderungen erst nach Rückfrage (siehe Formulare).
- `persistent` bei Formularen: Ein Klick daneben schließt nicht.
- Kein Dialog über einem Dialog. Einzige Ausnahme ist die Bestätigung („Änderungen verwerfen?“, „Löschen?“).

**Mobil:** Unter 960px Vollbild (Vorgabe von `EfDialog`, `fullscreenBelow="sm"`). Kopf und Fuß bleiben stehen, Buttons im Fuß unter 600px untereinander in voller Breite.

**Bestand und Umbau**
- `EfDialog`: richtige Grundlage, bleibt. Kopf-Icon (`icon`, `iconColor`) entfällt im Theme.
- `EfFormDialog`: Kopf mit Markenverlauf und Symbolkachel widerspricht dem Theme. Wird auf den Aufbau oben umgestellt oder in `EfDialog` überführt. `docs/DIALOG-STYLE-GUIDE.md` wird danach an diesen Abschnitt angepasst.
- `vue-js-modal` (`this.$modal.show`, 56 Stellen): keine neuen Aufrufe. Bestehende beim Umbau einer Ansicht auf `EfDialog` umstellen.

---

## 10. Bestätigung vor dem Löschen

**Prüfung nativer Dialoge:** `window.confirm`, `window.alert` und `window.prompt` werden nicht verwendet.
- Sie lassen sich nicht gestalten und sehen in jedem Browser anders aus.
- Die Buttons heißen „OK“ und „Abbrechen“. Was passiert, steht nicht darauf.
- Sie halten die ganze Seite an und sind auf dem Handy schwer zu bedienen.

Ersatz gibt es schon: `this.$ui.confirm()` aus `resources/js/lib/ui.js`, gerendert von `UiNotifications.vue`.

```js
const ok = await this.$ui.confirm(
    `Die Verkaufschance „${opp.name}“ wird gelöscht.`,
    {
        title: 'Verkaufschance löschen?',
        detail: 'Zugehörige Aufgaben und Notizen bleiben erhalten.',
        confirmLabel: 'Löschen',
        cancelLabel: 'Abbrechen',
        danger: true,
    }
);
if (!ok) return;
```

**Aufbau:** Dialog Stufe S. Titel als Frage mit dem Objekt („Kontakt löschen?“). Text nennt den Datensatz beim Namen. Darunter die Folgen in `ink-muted`. Buttons „Abbrechen“ (Ghost) und „Löschen“ (`ef-btn-danger`). Der Fokus liegt auf „Abbrechen“.

**Regeln**
- `confirmLabel` ist Pflicht und wiederholt das Verb: „Löschen“, „Archivieren“, „Entfernen“. Nie „OK“ oder „Ja“.
- Mehrere Datensätze: Titel mit Anzahl („3 Kontakte löschen?“), im Text bis zu drei Namen, dann „und 2 weitere“.
- Lässt sich das Löschen nicht rückgängig machen, steht das im Text: „Das lässt sich nicht rückgängig machen.“
- Besonders folgenreiche Aktionen (Mandant, Benutzer, alle Daten eines Kunden): zusätzlich muss der Name eingetippt werden, erst dann wird „Löschen“ aktiv.

**Rückgängig:** Wo das Modell gelöschte Datensätze aufbewahrt (Soft Delete), kommt nach dem Löschen eine Meldung mit „Rückgängig“ (siehe Meldungen). Stand 27.10.2026 mit Soft Delete: `CRMOpportunity`, `CRMQuote`, `CRMDocument`, `Employee`. Ohne Soft Delete: `CRMAccount`, `CRMContact`, `CRMLead`, `CRMNote`, `Task`. Dort gibt es kein „Rückgängig“, und der Dialog sagt, dass es endgültig ist. Für „Rückgängig“ braucht jedes Modell einen Endpunkt zum Wiederherstellen.

**Bestand:** `UiNotifications.vue` rendert den Dialog heute mit Bootstrap-Klassen (`btn btn-secondary`, `btn-danger`) und `confirmLabel` steht standardmäßig auf „OK“. Beim Umbau: Aufbau wie in Abschnitt 9, Buttons `ef-btn`, `confirmLabel` ohne Vorgabe (Pflichtangabe). Empfehlung: ein Test nach dem Muster von `tests/js/icon-satz.js`, der Aufrufe von `window.confirm`, `window.alert` und `confirm(` zählt und nicht wachsen lässt.

---

## 11. Meldungen und Banner

Zwei Bausteine mit klar getrennten Aufgaben:

| | Meldung (Toast) | Banner |
| --- | --- | --- |
| Wofür | Rückmeldung auf eine Aktion: angelegt, gespeichert, gelöscht, fehlgeschlagen | Zustand, der bleibt, bis er behoben ist: Verbindung getrennt, Testmodus, fehlende Freigabe |
| Wo | Unten rechts, über allem | Oben im Inhalt unter dem Seitenkopf, oder oben in Dialog und Formular |
| Dauer | verschwindet von selbst | bleibt, bis der Zustand weg ist oder es weggeklickt wird |
| Baustein | `this.$ui.success()`, `.info()`, `.warning()`, `.error()` | `ef-banner` mit `ef-banner-info`, `-warning`, `-danger` |

### Meldungen nach Anlegen, Bearbeiten und Löschen

```js
this.$ui.success('Verkaufschance angelegt.', { action: { label: 'Öffnen', fn: () => this.open(opp) } });
this.$ui.success('Änderungen gespeichert.');
this.$ui.success('Verkaufschance gelöscht.', { timeout: 8000, action: { label: 'Rückgängig', fn: () => this.restore(opp) } });
this.$ui.error('Die Verkaufschance konnte nicht gespeichert werden.', { detail: 'Bitte prüfen Sie Ihre Verbindung und versuchen Sie es erneut.' });
```

| Anlass | Text | Aktion | Dauer |
| --- | --- | --- | --- |
| Angelegt | „Verkaufschance angelegt.“ | „Öffnen“, wenn der neue Datensatz nicht schon offen ist | 4 s |
| Gespeichert | „Änderungen gespeichert.“ | keine | 4 s |
| Gelöscht (Soft Delete) | „Verkaufschance gelöscht.“ | „Rückgängig“ | 8 s |
| Gelöscht (endgültig) | „Verkaufschance gelöscht.“ | keine | 4 s |
| Fehler | Was passiert ist, darunter was zu tun ist | „Erneut versuchen“, wenn sinnvoll | bleibt, bis geschlossen |

**Aussehen:** Karte `surface-raised`, Rahmen `border`, `radius-md`, `shadow-overlay`, Breite 360px. Links das Zustandsicon (`success`, `info`, `warning`, `error`) in der Zustandsfarbe, daneben der Text in `body`, Zusatz in `caption`. Aktion als Text-Button in `navy`, rechts `close`. Keine farbige Vollfläche.

**Regeln**
- Höchstens drei Meldungen übereinander, die neueste unten.
- Beim Überfahren mit der Maus bleibt die Meldung stehen.
- Keine Meldung für Dinge, die man ohnehin sieht (ein Filter wurde gesetzt) und kein Meldungsschwall beim automatischen Speichern. Dort reicht ein stiller Hinweis „Gespeichert“ neben dem Feld.
- Fehler eines einzelnen Feldes gehören ans Feld, nicht in eine Meldung.

### Banner

```html
<div class="ef-banner ef-banner-warning" role="status">
    <ef-icon name="warning" />
    <div class="ef-banner-text"><strong>Outlook ist nicht verbunden.</strong> Termine werden nicht abgeglichen.</div>
    <a class="ef-banner-action" href="/einstellungen/kalender">Verbinden</a>
</div>
```

**Aussehen:** volle Inhaltsbreite, Fläche in der `-soft`-Farbe, Icon und Text in der Zustandsfarbe, `radius-md`, Innenabstand `space-3` `space-4`. Rechts höchstens eine Aktion und, wenn es wegklickbar ist, `close`.

**Regeln:** Höchstens ein Banner pro Ansicht. Kein Banner für Erfolg (dafür ist die Meldung da). Keine farbigen Banner als Seitenkopf oder Dekoration.

**Mobil:** Meldungen unten über die volle Breite mit `space-3` Rand, über der Navigationsleiste. Banner brechen um, die Aktion rutscht unter den Text.

**Bestand:** `$ui` und `UiNotifications.vue` sind die Grundlage (Meldungen unten rechts, Warteschlange, Pause beim Überfahren). Anzupassen: Icons auf `<ef-icon>`, Fehler ohne Zeitlimit, Gestaltung nach den Tokens oben. `GlobalSnackbar.vue` (`EventBus` mit `show-snackbar`, oben rechts) wird nicht mehr verwendet und beim Umbau auf `$ui` umgestellt.

---

## 12. Tooltips und Hilfetexte

**Tooltip:** `v-tooltip` mit `content-class="ef-tooltip"`.
- Nur für Icon-Buttons (Name der Aktion), abgekürzte Werte (genaue Zahl) und gekürzte Texte (ganzer Text).
- Nie für Informationen, die man zum Arbeiten braucht. Die stehen sichtbar auf der Seite.
- Fläche `navy-800`, Text weiß 12px, `radius-sm`, höchstens 240px breit. Erscheint nach 300ms.

**Hilfetext unter einem Feld:** `ef-field-hint` in `caption` und `ink-muted`, ein Satz.

**Längere Erklärung:** Icon `help` neben der Beschriftung, Klick öffnet ein Menü mit bis zu drei Sätzen und optional dem Link „Mehr im Handbuch“.

**Mobil:** Es gibt kein Überfahren. Tooltips erscheinen nach langem Drücken. Deshalb steht nichts nur im Tooltip, und Icon-Buttons haben immer ein `aria-label`.

---

## 13. Tabellen, Filter und Sortierung

Aufbau, Spaltenmenü und Kennzeichnung stehen in `DESIGN.md` unter „Tabellen: Sortieren und Filtern“. Ergänzend gilt:

**Kopf und Werkzeugzeile**
- Ist die Tabelle der einzige Inhalt der Seite, hat sie keinen eigenen Titel. Der Seitentitel reicht.
- Suche und gespeicherte Ansichten stehen in einer Zeile: links `ef-search`, rechts `ef-seg`. Darunter, nur wenn Filter aktiv sind, die Filter-Chips mit „Alle Filter zurücksetzen“.

**Spalten**
- Die erste Spalte ist der Name des Datensatzes als Link. Eine zweite Zeile darunter in `ink-muted` nur, wenn sie etwas Neues sagt (nicht dieselbe Bezeichnung noch einmal).
- Status: genau eine Spalte, darin genau ein Badge. Eigenschaften wie „Exklusiv“ oder „Standard“ sind Text in einer eigenen Spalte oder ein Hinweis in der Detailansicht.
- Zahlen: nur Zahlen, rechtsbündig. Fehlt ein Wert, bleibt die Zelle leer oder zeigt „0“ in `ink-muted`. Kein Warn-Badge in einer Zahlenspalte. Ein Problem zeigt die Statusspalte.
- Farbbudget: In einer Tabelle tragen nur Status-Badges Farbe, und nur dort, wo jemand handeln muss. Alles andere bleibt neutral.
- Breiten nach Inhalt: Texte bekommen den Platz, Zahlen, Datum und Status sind so schmal wie ihr längster Wert. Die Zeile endet mit `chevron-right`, wenn ein Klick die Detailansicht öffnet.

**Mobil**
- Listen von Datensätzen (Kontakte, Verkaufschancen, Aufgaben) werden unter 600px zu Karten: Name, zwei bis drei wichtige Werte, Status-Badge. Die Sortierung wählt man über einen Button „Sortieren“ über der Liste.
- Datentabellen mit vielen Zahlen (Preislisten, Auswertungen) scrollen waagerecht, die erste Spalte bleibt stehen.

---

## 14. Pagination

**Baustein:** `ef-pagination` in der Fußzeile der Tabelle, rechts.

```html
<nav class="ef-pagination" aria-label="Seiten">
    <span class="ef-muted">1 bis 50 von 312</span>
    <v-select v-model="perPage" :items="[25, 50, 100]" class="ef-vselect ef-vselect-sm" dense hide-details />
    <button class="ef-icon-btn" aria-label="Vorherige Seite" :disabled="page === 1"><ef-icon name="chevron-left" /></button>
    <button class="ef-icon-btn" aria-label="Nächste Seite" :disabled="page === last"><ef-icon name="chevron-right" /></button>
</nav>
```

**Regeln**
- Ab mehr als 50 Einträgen. Standard 50 pro Seite, wählbar 25, 50, 100.
- Keine Reihe von Seitenzahlen. Anzeige „1 bis 50 von 312“ plus Vor und Zurück.
- Seite, Sortierung und Filter stehen in der URL (`filterUrl.js`), damit Zurück im Browser die Liste wiederherstellt.
- Endloses Nachladen nur für Verläufe und Chats, nicht für Tabellen.

**Mobil:** „Seite 2 von 7“ mit Vor und Zurück, ohne Auswahl der Seitengröße.

---

## 15. Badges und Status

**Baustein:** `ef-badge` mit Varianten, `ef-count` für Zähler.

| Variante | Bedeutung | Beispiele |
| --- | --- | --- |
| `ef-badge` (neutral) | Ruhezustand, nichts zu tun | Entwurf, Inaktiv, Archiviert |
| `ef-badge-success` | Läuft oder abgeschlossen | Aktiv, Gewonnen, Versendet |
| `ef-badge-info` | In Arbeit, wartet auf das System | In Prüfung, Wird erstellt |
| `ef-badge-warning` | Braucht Aufmerksamkeit | Freigabe nötig, Läuft bald ab |
| `ef-badge-danger` | Fehler oder überfällig | Fehlgeschlagen, Überfällig |
| `ef-badge-neon` | Positive Veränderung, Markenbezug | +12 %, Neu |
| `ef-count` | Anzahl in Navigation und Tabs | 3 |

**Regeln**
- Ein bis zwei Wörter, Großschreibung wie im Satz („In Prüfung“, nicht „IN PRÜFUNG“).
- Jedes Objekt hat eine feste Liste von Status mit fester Farbe. Dieselbe Bezeichnung hat überall dieselbe Farbe.
- Ein Badge pro Zelle. Zählt man mehr, gehört die Information nicht in ein Badge.
- Zustandsfarben immer mit Wort, nie als bloßer Punkt.
- `ef-count` verschwindet bei 0.
- Technische Kennungen (`IT_OPERATIONS`, `accounts_payable`) erscheinen nie in einem Badge. Angezeigt wird die Bezeichnung aus den Sprachdateien.

---

## 16. Kennzahlen-Kacheln und große Zahlen

**Baustein:** `ef-kpi-grid` als Raster, darin `ef-card ef-kpi`. Eine Kachel pro Ansicht darf `ef-kpi-highlight` (Fläche `navy-800`) sein.

```html
<div class="ef-kpi-grid">
    <div class="ef-card ef-kpi">
        <span class="ef-kpi-label">Pipeline-Wert</span>
        <span class="ef-kpi-value" :title="exact">{{ formatKpi(1264000, 'EUR') }}</span>
        <span class="ef-badge ef-badge-neon ef-kpi-delta">+12 % zum Vormonat</span>
    </div>
</div>
```

**Zahlen formatieren**

| Wert | Anzeige | Regel |
| --- | --- | --- |
| 0 bis 999.999 | 124.300 € | ausgeschrieben mit Tausenderpunkt |
| ab 1 Million | 1,3 Mio. € | eine Nachkommastelle |
| ab 1 Milliarde | 1,2 Mrd. € | eine Nachkommastelle |
| Prozent | 38 % | ganze Zahl, unter 10 % eine Nachkommastelle (4,5 %) |
| negativ | -12 % | Minuszeichen ohne Leerzeichen |
| kein Wert | leer, darunter „Noch keine Daten“ | nie „0,00 €“ als Platzhalter |

- Der genaue Wert steht immer im Tooltip, sobald abgekürzt wird.
- Eine Funktion für alle: `formatKpi(wert, einheit)` auf Basis von `Intl.NumberFormat('de-DE', { notation: 'compact' })` ab 1 Million, darunter normales Format. Keine eigenen Rundungen in Komponenten.
- Zahl in `kpi` (28px, 700, `tabular-nums`), Bezeichnung darüber in `ink-muted`, Veränderung als `ef-badge-neon` (positiv) oder `ef-badge` (neutral oder negativ) darunter.

**Raster statt kleinerer Schrift:** Die Zahl bricht nie um und wird nie abgeschnitten. Wird es eng, rückt das Raster um (`repeat(auto-fit, minmax(200px, 1fr))`): vier Kacheln nebeneinander ab 1264px, zwei auf Tablet, zwei unter 600px mit der Zahl in 22px. Die Schrift wird nie stufenlos verkleinert.

**Nicht:** Verläufe als Hintergrund, bunte Icon-Kacheln in der Karte, eine Farbe pro Kachel, kleingeschriebene Aktionslinks wie „filtern“ in der Kachel. Klickbare Kacheln sind als ganze Karte klickbar und filtern die Liste darunter. Das Icon `chevron-right` zeigt, dass sie klickbar sind.

---

## 17. Ladeanzeigen und Fortschritt

| Lage | Anzeige |
| --- | --- |
| Unter 300ms | nichts |
| Seite oder Tabelle lädt | Platzhalter in Form der Inhalte (`v-skeleton-loader`, Fläche `border`), keine kreisende Anzeige mitten auf der Seite |
| Button-Aktion | Button deaktiviert, Icon `loading` mit `spin` statt des Icons, Beschriftung bleibt |
| Länger mit bekanntem Fortschritt (Import, Upload) | `ef-progress`: Balken `navy` auf `border`, 4px hoch, daneben „62 %“ und was gerade passiert („312 von 500 Kontakten importiert“) |
| Länger ohne bekannten Fortschritt (KI erstellt Briefing) | `ef-progress` ohne Wert (läuft durch), darunter ein Satz, was passiert: „Briefing wird erstellt. Das dauert etwa 20 Sekunden.“ |

**Regeln:** Nie die ganze Seite sperren, wenn nur ein Teil lädt. Dauert etwas länger als zehn Sekunden, kann man weiterarbeiten und bekommt eine Meldung, wenn es fertig ist.

---

## 18. Mobil: Regeln für alle Ansichten

Jede neue oder umgebaute Ansicht funktioniert ab 375px Breite. Die einzelnen Abschnitte oben nennen das Verhalten je Baustein, zusammengefasst:

| Element | Unter 960px | Unter 600px |
| --- | --- | --- |
| Seitennavigation | wird zur Kopfleiste mit Menü-Button, Navigation als Panel von links | wie unter 960px |
| Seitenkopf | Buttons unter den Titel | Hauptaktion als Button volle Breite, weitere Aktionen ins Menü „Weitere“ |
| Kennzahlen | zwei Kacheln nebeneinander | zwei Kacheln, Zahl 22px |
| Tabellen | waagerecht scrollen, erste Spalte steht | Listen als Karten, Datentabellen scrollen |
| Dialoge | Vollbild | Vollbild, Buttons untereinander |
| Formulare | eine Spalte | eine Spalte, Felder 16px Schrift |
| Meldungen | unten rechts | unten volle Breite |

**Grundsätze**
- Nichts funktioniert nur mit der Maus. Was beim Überfahren erscheint, ist auch per Tippen erreichbar.
- Kein waagerechtes Scrollen der ganzen Seite. Nur Tabellen dürfen waagerecht scrollen.
- Seitenrand `space-5` ab 600px, darunter `space-4`.
- Abfragen der Breite in Komponenten über `$bp` (`efBreakpoint`), in CSS über die Schwellen oben. Keine eigenen Werte wie 768px.
