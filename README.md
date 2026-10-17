# RathOS – Starter

Ein Familien-Organizer als **einzelne HTML-Datei**: Essensplanung mit Rezepten
und Einkaufsliste, To-dos, Wochenplan und ein optionaler Kinder-Bereich.
Kein Build, kein Server – die Datei läuft direkt im Browser und ist für das
iPhone (Safari, „Zum Home-Bildschirm") gebaut.

Diese Starter-Version kommt mit einem Satz Demo-Rezepten und einem
Ersteinrichtungs-Modus. Namen, Kinder und Standard-Portionen werden beim ersten
Start abgefragt – nichts ist fest verdrahtet.

**Vorab zum Überblick:** `rathos-intro.html` ist eine kurze, bebilderte
Einführung in die App – was sie kann, warum der Essensplan so tickt und was im
Hintergrund läuft. Ideal, um sie dem neuen Nutzer einmal zu zeigen, bevor er die
App zum ersten Mal öffnet.

---

## Schnellstart

1. `rathos-starter.html` im Browser öffnen (Doppelklick genügt), oder online
   stellen (siehe *Hosting*).
2. Beim ersten Start führt ein kurzes Onboarding durch: eine Begrüßung, dann
   die Einrichtung (Erwachsene, Kinder, Standard-Portionen).
3. Fertig. Alles wird lokal im Browser gespeichert.

Erneut einrichten geht jederzeit über **Menü (oben links) → Einstellungen →
Namen & Portionen ändern**.

---

## Aufbau

- **Drei Bereiche**, oben links übers Menü umschaltbar: **Küche** (Rezepte,
  Plan, Einkauf), **Orga** (To-dos, Wochenplan), **Kinder** (Beikost,
  Merkzettel). Beim Einrichten wird mindestens ein Kind angelegt; wer die App
  ganz ohne Kinder-Bereich will, kann in der Datei unter `obFinish` die
  Kind-Pflicht entfernen – fehlt ein Kind, blendet sich der Bereich automatisch
  aus.
- **Parameter:** Frühstück zählt als **1 Portion**, andere Gerichte nach der im
  Onboarding gewählten **Standard-Zahl**. Die Portionen lassen sich pro Rezept
  antippen und überschreiben.
- **Speicherung:** standardmäßig lokal im Browser (localStorage). Für zwei
  Geräte gibt es die optionale Airtable-Sync (unten).

---

## Hosting

**Netlify (am einfachsten):** Datei in `index.html` umbenennen und per
Drag-and-drop auf app.netlify.com/drop ziehen – fertige URL in Sekunden.
Alternativ ein GitHub-Repo mit `index.html` anlegen und in Netlify unter
*Add new site → Import from Git* verbinden; jeder Push deployed automatisch.

Das Repo darf **privat** sein und trotzdem per Mitarbeiter („Collaborators")
geteilt werden.

---

## Airtable-Sync einrichten (optional)

Nur nötig, wenn zwei Geräte denselben Stand teilen sollen (z. B. zwei Handys).
Ohne Airtable bleibt alles lokal auf dem jeweiligen Gerät.

**1. Base anlegen.** Eine neue Airtable-Base mit **einer Tabelle namens `Sync`**
und genau drei Feldern:

| Feld   | Typ                        |
|--------|----------------------------|
| `Key`  | Single line text (primär)  |
| `Num`  | Number                     |
| `Data` | Long text                  |

Am schnellsten über den mitgelieferten CSV-Import: in Airtable *Add a table →
Import data → CSV* und `rathos-sync-template.csv` hochladen. Danach einmal
prüfen, dass das Feld `Num` auf **Number** und `Data` auf **Long text** steht
(der CSV-Import legt sie sonst als Text an) und die Tabelle `Sync` heißt.

Die Datenzeilen (eine je Schlüssel) muss niemand von Hand anlegen – fehlende
legt die App beim ersten Speichern selbst an.

**2. Token erstellen.** Auf airtable.com/create/tokens:
- Scopes: `data.records:read` **und** `data.records:write`
- unter *Access* die eigene Base hinzufügen

**3. Verbinden.** In der App unter **Einstellungen → Airtable-Sync** die
**Base-ID** (der Teil der Airtable-URL, der mit `app…` beginnt) und den **Token**
eintragen. Auf dem zweiten Gerät dieselbe Base-ID und denselben Token – beide
sind dann synchron.

Die Sync läuft **über die Feldnamen**, nicht über interne IDs. Dadurch
funktioniert sie auch in einer frisch kopierten Base ohne Code-Anpassung.

**Synchronisierte Schlüssel:** `aktWoche`, `startTag`, `auswahl`, `eingekauft`,
`portionen`, `vorhanden`, `beilagenWahl`, `wochenplan`, `todos`, `beikost`,
`kinderdaten`, `rezeptReihenfolge`.

---

## Selber anpassen

Alles steckt im `<script>`-Block der einen Datei:

- **Rezepte:** die Rezept-Datenbank liegt in `RZ`, der rotierende Wochen-Pool in
  `P`. Dort Gerichte ergänzen oder ersetzen.
- **Namen / Portionen:** über die Einstellungen (kein Code nötig).
- **Kinder-Bereich:** standardmäßig wird beim Einrichten ein Kind verlangt;
  ohne Kind blendet sich der Bereich automatisch aus (Pflicht in `obFinish`).
- **Farben & Akzente:** als CSS-Variablen in `:root` und je Bereich über
  `html[data-bereich="…"]` – hell und dunkel getrennt.

---

## Datenschutz

Der Airtable-Token wird nur lokal im Browser (localStorage) abgelegt, **nicht**
im Code. Wer die HTML-Datei weitergibt, gibt damit keine Zugangsdaten weiter.
Ohne Airtable verlassen die Daten das Gerät nicht.
