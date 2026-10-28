# Themes

Ein Theme ist ein Ordner unter `resources/themes/<name>/` mit zwei Dateien:

| Datei | Inhalt |
| --- | --- |
| `DESIGN.md` | Die Vorgabe: Grundsaetze, Farben, Typografie, Layout, Sprache. Verbindlich fuer jede Oberflaeche im Theme. |
| `theme.scss` | Tokens (CSS-Variablen) und Bausteine (Klassen), gekapselt unter `[data-ef-theme="<name>"]`. |
| `ICONS.md` | Regeln fuer den Icon-Satz (echofront: Lucide, Liste in `resources/js/icons.js`). |
| `KOMPONENTEN.md` | Katalog aller UI-Elemente: Aufbau, Zustaende, Verhalten auf dem Handy, Texte. Verbindlich. |
| `tokens.json` | Alle Werte maschinenlesbar (Farben in beiden Themes, Schrift, Abstaende, Radien, Schatten). |

`resources/sass/app.scss` importiert jedes Theme. Weil alles gekapselt ist,
aendert ein Theme nichts, solange kein Layout es einschaltet.

## Einschalten

Das Attribut auf den Wurzelknoten des Layouts oder der Ansicht setzen:

```html
<div data-ef-theme="echofront"> ... </div>
```

Dunkle Variante (Website, Folien) zusaetzlich mit `data-theme="dark"` auf
demselben Knoten oder einem Unterknoten.

## Vorsicht mit alten Klassen

Die App nutzt `ef-btn`, `ef-table`, `ef-card`, `ef-seg`, `ef-card-head` und
`ef-page-title` schon mit eigenem Aussehen und BEM-Modifikatoren
(`ef-btn--primary`, `ef-seg--active`). Innerhalb eines eingeschalteten Themes
greifen die Theme-Regeln zuerst, die alten Modifikatoren passen dann nicht
mehr. Darum: Theme nur auf Ansichten einschalten, die vollstaendig mit den
Bausteinen des Themes gebaut oder umgebaut sind, nicht ueber alte Ansichten
stuelpen.

## Themes

| Name | Gilt fuer |
| --- | --- |
| `echofront` | echofront-Layouts: Software und Sales Hubs (hell), Website und Folien (dunkel). Siehe `echofront/DESIGN.md`. |

## Offene Punkte echofront

- Erledigt 10/2026: `tokens.json` liegt im Theme-Ordner. Navy-Logo fuer helle
  Flaechen liegt unter `public/images/echofront-logo-navy.png`, die Pfade in
  `DESIGN.md` zeigen auf `public/images/`.
- `EfFormDialog` (Kopf mit Verlauf), `UiNotifications.vue` (Bootstrap-Buttons,
  `confirmLabel` "OK") und `GlobalSnackbar.vue` entsprechen noch nicht
  `KOMPONENTEN.md`, Abschnitte 9 bis 11.
- Im Theme kollidieren zusaetzlich `ef-field`, `ef-label`, `ef-input`,
  `ef-textarea`, `ef-field-hint`, `ef-field-error` und `ef-vselect` mit den
  gleichnamigen Klassen aus `app.scss`. Innerhalb des Themes gelten die
  Theme-Regeln; Fehler werden dort ueber `aria-invalid` statt
  `ef-input--error` markiert.
- Roboto liegt lokal nur in 400, 500 und 700 vor, 600 rechnet der Browser
  aus den Nachbarschnitten.
