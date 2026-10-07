# Stempelkarte

Kleine Web-App für den persönlichen Gebrauch: Arbeitszeit stempeln, einzelne Tage ausrechnen und die Wochenstunden im Blick behalten.

## Look

Die App sieht aus wie ein kleines Gerät („SK-1“) mit LED-Display. Im Display wohnt ein Monster, das deinen Tag mitläuft:
es geht vom Häuschen zur 19-Std-Fahne, macht bei der Pause Kaffeepause, jubelt am Ziel, gerät über 19,5 Std in Panik und schläft nach Feierabend.
Gestempelt wird mit einem Schiebeschalter.

## Funktionen

- **Stempeluhr**: ein- und ausstempeln per Schiebeschalter, Netto-Zeit live, Uhrzeit, wann das Wochenziel erreicht ist, Woche als Stempelabdrücke
- **Rechner**: Beginn und Ende eintragen, Netto-Zeit ausrechnen und als Arbeitstag speichern
- **Verlauf**: alle Tage nach Kalenderwoche, mit Wochensumme
- Anzeige in Std/Min oder dezimal, Hell- und Dunkelmodus
- Funktioniert offline und lässt sich wie eine App auf den Home-Bildschirm legen

## Regeln

Stehen oben im Script in `index.html`:

| Konstante | Wert | Bedeutung |
|---|---|---|
| `PAUSE_AB_MIN` | 360 | Ab mehr als 6 Std Anwesenheit am Stück … |
| `PAUSE_MIN` | 45 | … werden 45 Min Pause abgezogen |
| `WOCHE_MIN` | 19 Std | Untere Grenze Wochenziel |
| `WOCHE_MAX` | 19,5 Std | Obere Grenze Wochenziel |
| `TAGE_PRO_WOCHE` | 3 | Arbeitstage pro Woche, an beliebigen Wochentagen |

Das **Tagesziel** ist das, was in der Woche noch bis 19 Std fehlt, geteilt durch die verbleibenden Arbeitstage (der heutige mitgezählt). Am ersten Tag sind das 6 Std 20 Min.

## Aufbau

Keine Abhängigkeiten, kein Build-Schritt. Alles ist reines HTML, CSS und JavaScript.

```
index.html             App (Markup, Styles, Logik)
manifest.webmanifest   App-Name, Farben, Icons für den Home-Bildschirm
sw.js                  Service Worker für Offline-Nutzung
icons/                 App-Icons (icon.svg ist die Vorlage)
```

## Lokal ausprobieren

Den Service Worker gibt es nur über `http://localhost` oder `https://`, nicht beim direkten Öffnen der Datei:

```bash
python3 -m http.server 8000
```

Dann <http://localhost:8000> öffnen.

## Auf dem Handy nutzen

Die App läuft über **GitHub Pages** (Settings → Pages → Branch `main`, Ordner `/ (root)`).
Adresse: <https://romerc-svg.github.io/stempelkarte/>

- **iPhone**: Adresse in Safari öffnen → Teilen → „Zum Home-Bildschirm“
- **Android**: Adresse in Chrome öffnen → Menü → „App installieren“

## Daten

Die Daten liegen nur im Browser-Speicher (`localStorage`) des jeweiligen Geräts. Sie werden nicht an einen Server geschickt.

- Auf dem iPhone hat die Home-Bildschirm-App einen **eigenen Speicher**, getrennt von Safari. Stempel am besten immer über das App-Icon.
- Wenn du die App löschst oder die Website-Daten leerst, sind die Einträge weg.

## Änderungen veröffentlichen

1. Ändern, committen, pushen. GitHub Pages aktualisiert sich nach ungefähr einer Minute.
2. Auf dem Handy die App einmal mit Internetverbindung öffnen. Der Service Worker holt dann zuerst die neue Version und nimmt den Cache nur, wenn du offline bist.
3. Neue Dateien (z. B. ein weiteres Icon) in `sw.js` bei `FILES` eintragen und die Versionsnummer bei `CACHE` erhöhen.
