# echofront Design System

Diese Datei ist die Vorgabe für jede Oberfläche der Software und der Sales Hubs. Sie liegt unter `resources/themes/echofront/` zusammen mit `KOMPONENTEN.md` (alle UI-Elemente im Einzelnen), `ICONS.md` (Icons), `theme.scss` (Tokens und Bausteine) und `tokens.json` (alle Werte maschinenlesbar).

**Arbeitsregeln für Entwicklung und KI**
- Vor jeder UI-Änderung diese Datei und den passenden Abschnitt in `KOMPONENTEN.md` lesen. Beide sind verbindlich.
- Farben, Abstände, Radien und Schatten nur über die CSS-Variablen unten. Keine Hex-Werte, keine neuen Farben, keine Verläufe im Code.
- Bausteine aus `theme.scss` verwenden (Klassen mit Präfix `ef-`), bevor eigene Styles entstehen.
- Jede Ansicht funktioniert ab 375px Breite (siehe „Mobil“).
- Bei Abweichung oder fehlendem Baustein: nachfragen statt erfinden.

Dieses System gilt für alles, was echofront zeigt: die Software, Sales Hubs, die Website, Folien und Dokumente. Werte stehen immer als Token, nie als Hex-Wert im Code.

## Grundsätze

1. **Ruhig und klar.** Helle, aufgeräumte Arbeitsflächen; Farbe nur dort, wo sie etwas bedeutet.
2. **Navy trägt, Neon zeigt.** `navy` und `navy-800` geben Halt, `neon` markiert die eine Sache, auf die es ankommt.
3. **Ein Muster pro Aufgabe.** Gleiche Dinge sehen überall gleich aus: ein Seitenkopf, eine Tabelle, ein Button-Satz.
4. **Inhalt vor Dekoration.** Keine Verläufe, keine Emojis, kein nachgebautes Logo-Motiv als Schmuck.

## Logo

- Zwei Varianten, je nach Grund. Auf `navy` und `navy-800`: Neon-Logo (`public/images/echofront-logo-neon-breit-freigestellt.png`).
- Auf Weiß und hellem Grund (`surface`, `surface-raised`): Navy-Logo (`public/images/echofront-logo-navy.png`).
- Ein weißes Logo gibt es nicht.
- Auf Fotos und Kundenfarben im Sales Hub das Logo nicht direkt auf die Fläche setzen, sondern auf eine ruhige Fläche in `navy` oder Weiß und dort die passende Variante verwenden.
- Das Logo nie nachzeichnen, verzerren, umfärben oder seine Bögen als Deko-Element verwenden.
- Mindesthöhe 20px in der Software, 28px auf Website und Folien. Schutzraum rundum mindestens die Höhe des Buchstabens „e“.
- Schreibweise im Text immer klein: echofront, auch am Satzanfang.

## Farben

Zwei Themes: **Software (hell)** für die Software, Sales Hubs und Dokumente; **Website & Folien (dunkel)** für Website, Folien und Social Media.

- **Grund und Text.** Seiten auf `surface`, Karten auf `surface-raised`, Text in `ink`, Sekundärtext in `ink-muted`.
- **Navy.** `navy-800` für die Seitennavigation und die eine Akzentkarte einer Ansicht; `navy` für Primärbuttons und Text.
- **Neon.** Nur auf drei Arten: als Fläche mit Text in `on-neon`, als Linie oder Markierung auf Navy, als Text auf Navy. Nie als Textfarbe auf hellem Grund. Auf hellen Flächen für positive Werte `neon-soft` mit `neon-ink`.
- **Mengen.** Auf einer Ansicht der Software ist höchstens ein Zehntel der Fläche Neon.
- **Status.** `success`, `warning`, `danger`, `info` nur für Zustände, immer mit Wort oder Icon. Nie als Dekoration oder um Karten zu unterscheiden.
- **Verboten.** Verläufe aller Art, Lila, Pink, Türkis und weitere Akzentfarben. Wer eine neue Farbe braucht, stellt sie hier zur Entscheidung.

## Typografie

- Eine Schrift: Roboto in 400, 500, 600 und 700. Keine Monospace- oder Zweitschrift.
- Software: `page-title` einmal pro Ansicht, `section-title` für Karten und Tabellen, `body` als Standard, `label` nur für Navigationsgruppen und Tabellenköpfe, `caption` für Zeitangaben und Hilfetexte, `kpi` für Kennzahlen.
- Website und Folien: `display` für den Hero, `headline` für Abschnitte, `lead` für Einleitungen.
- Höchstens drei Schriftgrößen pro Karte oder Folie.
- Zahlen in Tabellen und Kennzahlen mit `tabular-nums`, deutsch formatiert: 864.000 €, 38 %, 10.10.2026.

## Abstände, Radien, Schatten

- Abstände nur aus der Skala `space-1` bis `space-7`. Karten innen `space-5`, zwischen Karten `space-4`, zwischen Abschnitten `space-6`.
- Ein Radius pro Ansicht: `radius-md` für Karten, Tabellen, Buttons und Dialoge. `radius-sm` für Badges und Felder, `radius-pill` nur für Segmentschalter und Zähler.
- Karten haben `border` und `shadow-card`, sonst keinen Schatten. `shadow-overlay` nur für Dialoge und Menüs.
- Höhe eines Containers ergibt sich aus dem Inhalt plus Innenabstand, nie aus dem Restplatz.

## Software-Layout

- Links die Seitennavigation (Komponente Navigation), rechts der Arbeitsbereich auf `surface` mit Seitenrand `space-5` (unter 600px `space-4`).
- Listen, Tabellen und Dashboards nutzen die volle Breite des Arbeitsbereichs. Formulare und Detailansichten stehen linksbündig mit höchstens 880px Inhaltsbreite. Kein zentrierter Inhalt mit breiten leeren Rändern.
- Jede Ansicht beginnt mit dem Seitenkopf (PageHeader): Titel, Kontextzeile, rechts Zeitraum und genau ein Primärbutton. Keine farbigen Banner.
- Darunter Kennzahlen (KpiCard), dann Inhalte in Karten und Tabellen (DataTable).
- Leere Zustände sagen, was hier erscheint und wie man anfängt: „Noch keine Aufgaben. Neue Aufgaben erscheinen hier, sobald sie Ihnen zugewiesen werden.“
- Endgültige Aktionen wie Löschen erst nach Bestätigung im Dialog, dort in `danger` (siehe „Löschen bestätigen“).

**Seitenkopf der Detailansicht** (ein einzelner Datensatz, zum Beispiel ein Provider oder eine Verkaufschance)
1. Oben der Zurück-Link in `caption` mit Icon `back`: „LLM-Provider“.
2. Titel in `page-title` (24px, 700). Der Titel ist Text, kein Auswahlfeld. Wechseln zu einem anderen Datensatz geht über die Liste oder einen eigenen `ef-icon-btn` mit `chevron-down` neben dem Titel.
3. Rechts neben dem Titel genau ein Status-Badge (`Aktiv`, `Inaktiv`).
4. Darunter eine Kontextzeile in `ink-muted` mit höchstens zwei Angaben, getrennt durch „ | “. Keine Wiederholung des Titels, keine technischen Kennungen, keine Links in der Zeile.
5. Rechts: Primärbutton „Speichern“ und das Menü „Weitere“ (`more`) mit seltenen Aktionen. Löschen steht nur dort, nie als eigener Button. Ein Status wie „Aktiv“ wird im Inhalt umgeschaltet, nicht mit einem Switch im Kopf.

**Zustände von Bedienelementen:** Deaktiviert heißt Fläche `surface`, Rand `border`, Schrift `ink-muted`, Zeiger `not-allowed`, der Grund steht daneben. Nie ausgegraute Farbe mit weißer Schrift. Alle Zustände in `KOMPONENTEN.md`, Abschnitt 1.

## Tabellen: Sortieren und Filtern

Jede Tabelle der Software sortiert und filtert nach demselben Muster: über das Menü im Spaltenkopf. Ausprobieren lässt es sich im Entwurf `docs/ENTWURF-TABELLE-SORTIERUNG-FILTER.html`, Variante D.

**Aufbau, von oben nach unten**
1. Kartenkopf (`ef-card-head ef-toolbar`): links das Suchfeld (`ef-search`), rechts die gespeicherten Ansichten als `ef-seg`, zum Beispiel Alle | Meine | Kurz vor Abschluss. Höchstens vier Ansichten. Einen Tabellentitel in `section-title` gibt es nur, wenn mehrere Karten auf der Seite stehen; ist die Tabelle der einzige Inhalt, reicht der Seitentitel.
2. Filterzeile: nur wenn Filter aktiv sind. Jeder aktive Filter als Chip, ganz rechts „Alle Filter zurücksetzen“.
3. Tabelle (`ef-table`): jeder Spaltenkopf ist ein Button und öffnet sein Menü.
4. Fußzeile in `caption` und `ink-muted`: links „4 von 15 Verkaufschancen | Summe 116.000 €“, rechts „Sortiert nach Wert, absteigend“.

**Spaltenmenü**
- Klick auf den Spaltenkopf öffnet das Menü unter dem Kopf, mit `shadow-overlay` und `radius-md`. Bei rechtsbündigen Spalten richtet es sich rechts aus.
- Oben immer „Aufsteigend sortieren“ und „Absteigend sortieren“ mit `chevron-up` und `chevron-down`. Darunter, durch eine Linie in `border` getrennt, die Filter dieser Spalte unter der Überschrift „Filtern“ in `label`.
- Filter je Spaltentyp: Auswahlwerte (Phase, Zuständig) als Checkboxen mit Trefferzahl rechts in `ink-muted`; Beträge als feste Stufen (Jeder Wert, ab 5.000 €, ab 10.000 €, ab 25.000 €); Datum als Zeiträume (Alle Termine, Diesen Monat, Dieses Quartal, Überfällig). Freitext und Namen werden über das Suchfeld gefunden, nicht im Spaltenmenü.
- Ein Filter wirkt sofort, ohne Button „Anwenden“. Das Menü bleibt bei Checkboxen offen und schließt bei Sortierung, Klick außerhalb oder Escape.
- Spalten ohne sinnvollen Filter bieten nur die Sortierung an.

**Kennzeichnung**
- Es gibt genau eine Sortierung. Der Kopf der sortierten Spalte steht in `ink` und trägt `chevron-up` oder `chevron-down`, alle anderen Köpfe in `ink-muted`; sie zeigen `chevron-down` als Hinweis auf das Menü erst beim Überfahren und bei Fokus. Mit `aria-sort` auszeichnen.
- Eine gefilterte Spalte trägt neben dem Titel ein kleines Feld in `navy` mit Icon `filter` in Weiß.
- Jeder aktive Filter steht zusätzlich als Chip in der Werkzeugzeile: „Phase: Negotiation, Closing“, Kante `border-strong`, `radius-sm`, Text 13px, Name des Werts in 500, rechts Icon `close` zum Entfernen. Wer einen Filter setzt oder entfernt, verlässt die gespeicherte Ansicht; im `ef-seg` ist dann nichts mehr gedrückt.

**Standards und Zustände**
- Standard-Sortierung ist die Spalte, nach der gearbeitet wird, meist das nächste Datum aufsteigend.
- Zahlen- und Datumsspalten rechtsbündig (`ef-right`), Zahlen mit `tabular-nums`.
- Findet ein Filter nichts, sagt die Tabelle, was zu tun ist, und bietet das Zurücksetzen an: „Keine Verkaufschancen für diese Filter. Entfernen Sie einen Filter oder setzen Sie die Ansicht zurück.“
- Nicht verwendet werden: Sortierung über mehrere Spalten, feste Filterleisten aus Auswahlfeldern, dauerhafte Filterpanels neben der Tabelle.

**Spalten**
- Erste Spalte: Name des Datensatzes als Link. Eine zweite Zeile darunter nur, wenn sie etwas Neues sagt.
- Genau eine Statusspalte mit genau einem Badge. Eigenschaften wie „Exklusiv“ stehen als Text in einer eigenen Spalte oder in der Detailansicht.
- Zahlenspalten enthalten nur Zahlen. Fehlt ein Wert, bleibt die Zelle leer oder zeigt „0“ in `ink-muted`, nie ein Warn-Badge. Ein Problem zeigt die Statusspalte.
- Farbbudget: Farbe tragen nur Status-Badges, und nur dort, wo jemand handeln muss.
- Spaltenbreiten nach Inhalt: Texte bekommen den Platz, Zahl, Datum und Status sind so schmal wie ihr längster Wert. Öffnet ein Klick die Zeile, endet sie mit `chevron-right`.
- Pagination ab mehr als 50 Einträgen (`KOMPONENTEN.md`, Abschnitt 14). Unter 600px werden Listen zu Karten (Abschnitt 13).

## Dialoge

Alle Dialoge haben denselben Aufbau und laufen über `<ef-dialog>`: Kopf mit Titel in `section-title` und Schließen-Icon, Inhalt mit `space-5` Innenabstand, der allein scrollt, Fuß mit Hinweis links und Buttons rechts (Ghost, dann Primär). Fläche `surface-raised`, `radius-md`, `shadow-overlay`, Hintergrund `navy` mit 40 % Deckkraft. Kein Verlauf, keine Icon-Kachel, kein farbiger Kopf.

- Drei Breiten: S 480px (Bestätigung), M 640px (Formular, Standard), L 880px (zweispaltig, Tabellen).
- Titel sagt, was passiert: „Kontakt bearbeiten“. Fokus ins erste Feld, Esc schließt, bei ungespeicherten Änderungen erst nach Rückfrage.
- Unter 960px Vollbild, Kopf und Fuß bleiben stehen.
- Kein Dialog über einem Dialog, außer der Bestätigung.

Einzelheiten und Umbau von `EfFormDialog` und `vue-js-modal`: `KOMPONENTEN.md`, Abschnitt 9.

## Löschen bestätigen

`window.confirm`, `window.alert` und `window.prompt` werden nicht verwendet: Sie lassen sich nicht gestalten, ihre Buttons heißen „OK“ statt „Löschen“, und sie sehen in jedem Browser anders aus. Stattdessen `await this.$ui.confirm(...)`.

- Dialog Stufe S. Titel als Frage mit Objekt: „Verkaufschance löschen?“
- Text nennt den Datensatz beim Namen, darunter die Folgen: „Die Verkaufschance „Muster AG | Erweiterung“ wird gelöscht. Zugehörige Aufgaben bleiben erhalten.“
- Buttons „Abbrechen“ (Ghost, hat den Fokus) und „Löschen“ (`ef-btn-danger`). Nie „OK“ oder „Ja“.
- Mehrere Datensätze: „3 Kontakte löschen?“. Ist es endgültig, steht im Text: „Das lässt sich nicht rückgängig machen.“
- Wo das Modell gelöschte Datensätze aufbewahrt (Soft Delete), folgt eine Meldung mit „Rückgängig“.

Einzelheiten: `KOMPONENTEN.md`, Abschnitt 10.

## Rückmeldungen nach Anlegen, Bearbeiten und Löschen

Jede Aktion bekommt eine kurze Meldung über `this.$ui` unten rechts (unter 600px unten in voller Breite). Banner sind für Zustände, die bleiben.

| Anlass | Meldung | Aktion | Dauer |
| --- | --- | --- | --- |
| Angelegt | „Verkaufschance angelegt.“ | „Öffnen“ | 4 s |
| Gespeichert | „Änderungen gespeichert.“ | keine | 4 s |
| Gelöscht | „Verkaufschance gelöscht.“ | „Rückgängig“ bei Soft Delete | 8 s |
| Fehler | Was passiert ist und was zu tun ist | „Erneut versuchen“ | bis geschlossen |

Meldung: Karte `surface-raised` mit `border` und `shadow-overlay`, links das Zustandsicon in der Zustandsfarbe, keine farbige Vollfläche. Banner (`ef-banner`): unter dem Seitenkopf, Fläche in der `-soft`-Farbe, höchstens eins pro Ansicht, nie für Erfolg. Einzelheiten: `KOMPONENTEN.md`, Abschnitt 11.

## Große Zahlen in Kacheln

- Bis 999.999 ausgeschrieben mit Tausenderpunkt: „124.300 €“. Ab 1 Million mit einer Nachkommastelle: „1,3 Mio. €“, ab 1 Milliarde „1,2 Mrd. €“.
- Der genaue Wert steht im Tooltip, sobald abgekürzt wird. Eine gemeinsame Funktion `formatKpi()` formatiert alle Kennzahlen.
- Die Zahl bricht nie um und wird nie abgeschnitten. Wird es eng, rückt das Raster um (`ef-kpi-grid`: vier, zwei, zwei Kacheln je Zeile), statt die Schrift zu verkleinern. Unter 600px steht die Zahl in 22px.
- Kein Wert: Feld leer und „Noch keine Daten“, nie „0,00 €“.
- Keine Verläufe, keine bunten Icon-Kacheln, keine Farbe pro Kachel. Höchstens eine Kachel pro Ansicht als `ef-kpi-highlight`.

Einzelheiten: `KOMPONENTEN.md`, Abschnitt 16.

## Mobil

Jede neue oder umgebaute Ansicht funktioniert ab 375px. Breakpoints wie Vuetify: `xs` unter 600px, `sm` 600 bis 959px, `md` 960 bis 1263px, `lg` ab 1264px. In Komponenten über `$bp` (`efBreakpoint`), keine eigenen Werte.

- Unter 960px: Seitennavigation als Kopfleiste mit Menü, Dialoge im Vollbild, Tabellen scrollen waagerecht mit stehender erster Spalte.
- Unter 600px: Listen als Karten, Formulare einspaltig mit 16px Schrift in Feldern, Buttons in Dialogen untereinander in voller Breite.
- Klickflächen mindestens 40 × 40px. Nichts funktioniert nur beim Überfahren mit der Maus.
- Getestet wird bei 375px, 768px und 1280px.

Die Tabelle je Baustein: `KOMPONENTEN.md`, Abschnitt 18.

## Diagramme

- Datenreihen in `chart-1`, `chart-2`, `chart-3`. Gitterlinien in `border`, Achsen in `ink-muted`.
- `chart-highlight` hebt genau eine Sache hervor, zum Beispiel die Arbeit der Digitalen Mitarbeiter, und trägt immer den Wert als Zahl.
- Balken statt Trichter, Kreis oder 3D. Achsen beginnen bei null.

## Iconografie

- Ein einziges Icon-Set: Lucide, Liste in `resources/js/icons.js`, Regeln in `ICONS.md`. Linien-Icons, 16px in Text und Navigation, 20px in Buttons ohne Text, Strichstärke 2.
- Icons in `ink-muted` oder `slate`, aktiv in `ink` oder `neon`. Keine bunten Icons, keine Emojis, keine gefüllten App-Kacheln.

## Sprache und Ton

- Deutsch, Anrede „Sie“, überall. Auch Hinweise wie „Wählen Sie eine Aufgabe aus“.
- Klar und sachlich, aktive Sätze, Buttons sagen, was passiert.
- Begriffe: Digitaler Lead-Researcher, Digitaler Telefonassistent, Digitale Mitarbeiter, Sales Hub, Verkaufschance, Wiedervorlage. Interne Namen von Agenten erscheinen nicht für Kunden.
- Verkaufsphasen bleiben so, wie sie konfiguriert sind (Lead / Qualification, Meeting Follow-up, Needs Analysis, Offer, Negotiation, Closing).
- KI im deutschen Text, AI nur in englischen Namen (AI Social Media Suite).
- Keine Gedankenstriche, kein Mittelpunkt als Trenner. Trenner ist „ | “ mit Leerzeichen.
- Fehlermeldungen sagen, was passiert ist und was zu tun ist: „Die Datei ist größer als 20 MB. Bitte laden Sie eine kleinere Datei hoch.“

## Dokumente und Folien

- Folien: Grund `navy`, Karten `navy-800`, Akzent `neon`, Sekundärtext `slate`, Roboto. Ein Radius und ein Innenabstand pro Folie, höchstens drei Schriftgrößen.
- Rechnungen, Auftragsbestätigungen und Angebote: Theme Software (hell), Text in `ink`, Tabellenköpfe in `label`. Fußzeile mit Amtsgericht Mannheim. Bezeichnung der Summe „RECHNUNGSBETRAG:“.
- Nummernkreise: Rechnungen nach dem Schema RE-20261042-1, Auftragsbestätigungen AB-JJJJNNNN.

## Farb-Token

| Token | Verwendung |
| --- | --- |
| `navy` | Markenfarbe. Grund von Website und Folien, Text und Primärbuttons auf hellen Flächen. 16,7:1 auf Weiß. |
| `navy-800` | Seitennavigation der Software, Karten auf Navy, Akzentkarte im hellen Arbeitsbereich. Weiß darauf 15:1. |
| `navy-700` | Aktiver Navigationspunkt und Hover auf Navy. Weiß darauf 12,9:1. |
| `neon` | Akzent. Nur als Fläche mit Text in on-neon, als Linie auf Navy oder als Text auf Navy (13,8:1). Nie als Textfarbe auf Weiß oder hellem Grund (1,2:1). |
| `on-neon` | Text und Icons auf neon. 13,8:1. |
| `slate` | Sekundärtext auf navy und navy-800 (6,2:1 und 5,6:1). Auf Weiß nur für Linien und Diagramme, nie für Text (2,7:1). |
| `surface` | Seitengrund. Hell: Arbeitsbereich der Software. Dunkel: Website und Folien. |
| `surface-raised` | Karten, Tabellen, Dialoge auf surface. |
| `border` | Kanten von Karten, Tabellenzeilen und Eingabefeldern. Dekorativ, trägt keine Bedeutung. |
| `border-strong` | Kante von Eingabefeldern und Sekundärbuttons, 3:1 auf surface-raised. |
| `ink` | Fließtext und Überschriften auf surface und surface-raised. |
| `ink-muted` | Sekundärtext, Labels, Achsen auf surface und surface-raised. Hell 5,3:1, dunkel 5,6:1. |
| `neon-soft` | Fläche für positive Kennzahlen und Hinweise mit Markenbezug. Text darauf in neon-ink. |
| `neon-ink` | Text auf neon-soft. Hell 7,1:1. |
| `focus` | Fokusrahmen, 2px durchgehend, 2px Abstand. Mindestens 3:1 auf jedem Grund. |
| `success` | Erfolg: Text und Icons. Immer mit Wort oder Icon, nie nur Farbe. |
| `success-soft` | Fläche für Erfolgs-Badges, Text in success. |
| `warning` | Warnung: Text und Icons, immer mit Wort oder Icon. |
| `warning-soft` | Fläche für Warn-Badges, Text in warning. |
| `danger` | Fehler und endgültige Aktionen. Nur im Bestätigungsdialog als Buttonfläche, sonst als Text oder Icon. |
| `danger-soft` | Fläche für Fehler-Badges und Fehlermeldungen, Text in danger. |
| `info` | Neutrale Hinweise und Systemmeldungen. |
| `info-soft` | Fläche für Info-Badges, Text in info. |
| `chart-1` | Erste Datenreihe, Balken und Linien. |
| `chart-2` | Zweite Datenreihe. |
| `chart-3` | Dritte Datenreihe und Vergleichswerte. |
| `chart-highlight` | Hebt genau eine Sache pro Diagramm hervor, zum Beispiel die Digitalen Mitarbeiter. Auf Weiß 1,2:1: deshalb steht der Wert immer als Zahl daneben. |

## Schriftstile

| Stil | Größe / Zeilenhöhe | Gewicht | Verwendung |
| --- | --- | --- | --- |
| `display` | 48px / 52px | 700 | Hero der Website. Eine Zeile pro Seite. |
| `headline` | 32px / 38px | 700 | Abschnittsüberschriften der Website, Folientitel. |
| `lead` | 18px / 28px | 400 | Einleitungstext unter display und headline. |
| `page-title` | 24px / 32px | 700 | Seitentitel im Seitenkopf. Einmal pro Ansicht. |
| `section-title` | 16px / 24px | 600 | Titel von Karten, Tabellen und Dialogen. |
| `kpi` | 28px / 32px | 700 | Kennzahl in KPI-Karten. Mit tabular-nums. |
| `body` | 14px / 22px | 400 | Standardtext der Software, Tabellenzellen. |
| `body-strong` | 14px / 22px | 500 | Hervorgehobener Text, Namen in Listen, Buttons. |
| `label` | 12px / 16px | 500 | Navigationsgruppen und Tabellenköpfe, in Großbuchstaben. Nie für Sätze. |
| `caption` | 12px / 16px | 400 | Zeitangaben, Hilfetexte unter Feldern, Fußnoten. |

## CSS-Variablen

```css
:root {
  --navy: #071F37;
  --navy-800: #0E2841;
  --navy-700: #123354;
  --neon: #BAFF00;
  --on-neon: #071F37;
  --slate: #8AA0B6;
  --surface: #F2F4F6;
  --surface-raised: #FFFFFF;
  --border: #E1E6EB;
  --border-strong: #848F9C;
  --ink: #071F37;
  --ink-muted: #56677A;
  --neon-soft: #EEF9D2;
  --neon-ink: #3F5A10;
  --focus: #071F37;
  --success: #1E7A4C;
  --success-soft: #E6F4EC;
  --warning: #9A5B00;
  --warning-soft: #FDF1DD;
  --danger: #B4372E;
  --danger-soft: #FBE9E7;
  --info: #1F5F99;
  --info-soft: #E7F0F9;
  --chart-1: #0E2841;
  --chart-2: #56677A;
  --chart-3: #A8B9CA;
  --chart-highlight: #BAFF00;
  --space-1: 4px;
  --space-2: 8px;
  --space-3: 12px;
  --space-4: 16px;
  --space-5: 24px;
  --space-6: 32px;
  --space-7: 48px;
  --radius-sm: 6px;
  --radius-md: 12px;
  --radius-lg: 16px;
  --radius-pill: 999px;
  --shadow-card: 0 1px 2px rgba(7, 31, 55, 0.06);
  --shadow-overlay: 0 12px 32px rgba(7, 31, 55, 0.18);
  --font-sans: Roboto, system-ui, -apple-system, "Segoe UI", Arial, sans-serif;
}

/* Website und Folien */
[data-theme="dark"] {
  --surface: #071F37;
  --surface-raised: #0E2841;
  --border: #1D3A57;
  --border-strong: #5F7B97;
  --ink: #FFFFFF;
  --ink-muted: #8AA0B6;
  --neon-soft: #22401A;
  --neon-ink: #BAFF00;
  --focus: #BAFF00;
  --success: #5FD39A;
  --success-soft: #12382A;
  --warning: #F5B547;
  --warning-soft: #3A2C10;
  --danger: #FF8A7F;
  --danger-soft: #3D1A1A;
  --info: #7DB6EE;
  --info-soft: #13304D;
  --chart-1: #E8EDF2;
  --chart-2: #8AA0B6;
  --chart-3: #3E5C7A;
}
```

## Bausteine (theme.scss)

| Baustein | Klassen |
| --- | --- |
| Button | `ef-btn`, `ef-btn-primary`, `ef-btn-accent`, `ef-btn-ghost`, `ef-btn-sm`, `ef-btn-danger` (nur im Bestätigungsdialog), `ef-icon-btn` |
| Badge | `ef-badge`, `ef-badge-neon`, `ef-badge-solid`, `ef-badge-success`, `ef-badge-warning`, `ef-badge-danger`, `ef-badge-info`, `ef-count` |
| Karte und KPI | `ef-card`, `ef-card-head`, `ef-kpi`, `ef-kpi-value`, `ef-kpi-label`, `ef-kpi-highlight` |
| Seitenkopf | `ef-page-head`, `ef-page-title`, `ef-page-sub`, `ef-seg` |
| Navigation | `ef-nav`, `ef-nav-logo`, `ef-nav-group`, Links mit `aria-current="page"` |
| Tabelle | `ef-table-wrap`, `ef-table`, `ef-right`, `ef-muted` |
| Werkzeugzeile und Suche | `ef-toolbar`, `ef-search`, `ef-chip`. Spaltenmenü und Filtermarke im Spaltenkopf wie im Entwurf `docs/ENTWURF-TABELLE-SORTIERUNG-FILTER.html` (Variante D) |
| Pagination | `ef-pagination` |
| Formular | `ef-field`, `ef-label`, `ef-label-tag`, `ef-req`, `ef-input`, `ef-textarea`, `ef-field-hint`, `ef-field-error`, `ef-grid-2`, `ef-vselect` |
| Auswahl | `ef-check`, `ef-radio`, `ef-switch`, `ef-switch-track` |
| Dialog | `ef-dialog-head`, `ef-dialog-title`, `ef-dialog-sub`, `ef-dialog-body`, `ef-dialog-foot`, `ef-dialog-note` (Hülle: `<ef-dialog>`) |
| Meldung und Banner | `ef-toast`, `ef-toast-success`, `-info`, `-warning`, `-error`, `ef-banner`, `ef-banner-info`, `-warning`, `-danger`, `ef-banner-text`, `ef-banner-action` |
| Tooltip | `ef-tooltip` (als `content-class` am `v-tooltip`) |
| Laden | `ef-progress`, `ef-progress-bar`, `ef-skeleton`, `ef-icon-spin` |
| Kennzahlen | `ef-kpi-grid` |

Wie jedes Element verwendet wird, steht in `KOMPONENTEN.md`.
