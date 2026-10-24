# Einkaufshelfer – Mein Einkaufsgeld

Einkaufs-App für einen jungen Erwachsenen mit Förderschwerpunkt geistige Entwicklung (Wohngruppe). Er fotografiert den Strichcode eines Lebensmittels bei Edeka; die App erkennt das Produkt, prüft das Wochenbudget (Standard 20 €), zeigt eine Lebensmittel-Ampel und spricht alles laut aus. Betreuer pflegen Produkte, Preise, Barcodes, Ampel und Fotos im Betreuer-Bereich.

## Dateien

- `artifact.html` – die Quelle. Sie wird als Claude-Artefakt veröffentlicht: https://claude.ai/artifact/KgsYc6JiWKiupBbaeHvkGE (claude.ai ergänzt beim Veröffentlichen das HTML-Gerüst, deshalb hat die Datei kein `<!doctype>`, `<html>`, `<head>` oder `<body>`).
- `index.html` – eigenständige Version: dasselbe wie `artifact.html`, umschlossen vom HTML-Gerüst. Nie von Hand bearbeiten, immer aus `artifact.html` erzeugen (Gerüst siehe unten).
- `README.md` – Beschreibung für Menschen.

## Aufbau von artifact.html

Alles steckt in einer Datei:

1. `<style id="app-style">` – Farben als Tokens in `:root` mit Dunkel-Varianten (`prefers-color-scheme` und `[data-theme]`). Schrift: Atkinson Hyperlegible.
2. `<input id="scan-input">` und `<input id="foto-input">` – Datei-Eingaben für Kamera-Fotos; sie liegen außerhalb von `#app`, damit sie das Neuzeichnen überstehen.
3. ZXing (`@zxing/library@0.21.3`, UMD von jsDelivr) – Barcode-Erkennung, falls der Browser keinen `BarcodeDetector` hat.
4. `<script type="application/json" id="daten">` – **die Produktdatenbank**: `{budget, kategorien:[{id,name}], produkte:[{id,name,preis,emoji,kat,ean,ampel,foto?}]}`. `ampel` ist `gruen`, `gelb`, `rot` oder leer (abgeleitet vom Nutri-Score: A/B grün, C gelb, D/E rot). `foto` ist eine JPEG-Data-URI (max. 260 px). `<` im JSON als `<` maskieren.
5. `<script id="app-script">` – die ganze Logik, ein IIFE ohne Framework. `render()` baut `#app` komplett neu; Klicks laufen über `data-act`, Vorlesen beim Darüberfahren über `data-sprich`.

## Speichern aus der App

Der Knopf „Speichern“ im Betreuer-Bereich ruft über die Artefakt-Fähigkeit `artifact` die Funktion `publish()` auf. `seiteBauen()` setzt die Seite dabei aus `#app-style`, `#app-script` und dem aktuellen Zustand neu zusammen. **Wer das Seitengerüst in artifact.html ändert (Kopf, Eingaben, Script-Tags), muss `seiteBauen()` genauso anpassen**, sonst geht die Änderung beim nächsten Speichern aus der App verloren.

Weil Betreuer in der App speichern, kann das live veröffentlichte Artefakt neuer sein als dieses Repository. Vor Änderungen an `artifact.html` immer zuerst den aktuellen Stand mit dem Artifact-Tool lesen (`action: "read"`) und die Produktdaten übernehmen. Ein geplanter Task („Einkaufshelfer synchronisieren“, täglich 19:57 Uhr) holt den Stand aus der App automatisch hierher.

## Veröffentlichen und index.html erzeugen

Nach einer Änderung an `artifact.html`:

1. Mit dem Artifact-Tool veröffentlichen, mit `url` = Link oben, damit der Link gleich bleibt.
2. `index.html` neu erzeugen:

```sh
{ printf '<!doctype html>\n<html lang="de">\n<head>\n<meta charset="utf-8">\n<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">\n<style>:root{color-scheme:light;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}body{margin:0;font:14px system-ui,sans-serif}img{max-width:100%%}[hidden]{display:none!important}</style>\n</head>\n<body>\n'; cat artifact.html; printf '\n</body>\n</html>\n'; } > index.html
```

3. Syntax prüfen: den Inhalt von `<script id="app-script">` in eine Datei ziehen und `node --check` darauf laufen lassen.

## Grenzen der Umgebung (nicht erneut versuchen)

- Das Artefakt darf keine Daten von fremden Servern abrufen. Open Food Facts oder Open EAN lassen sich deshalb nicht einbinden (Open EAN läuft außerdem nur über http).
- Keine Live-Kamera im Artefakt; gescannt wird über ein Foto (`capture="environment"`).
- `alert()`, `confirm()` und `prompt()` funktionieren im Artefakt nicht; Rückfragen werden in die Seite eingebaut.
- Edeka bietet keine vollständige Preisliste; Preise sind Schätzwerte oder werden im Laden eingetragen. Getränkepreise inklusive Pfand.

## Stil

- Alle Texte in einfachem Deutsch, kurze Sätze, große Schrift und große Knöpfe. Der Nutzer liest kaum: Bilder, Farben und Sprachausgabe tragen die Bedeutung.
- Für den Nutzer keine Zahlen, die er vergleichen muss. Geld zeigt der Balken; Euro-Beträge nur klein bzw. im Betreuer-Bereich.
- Commits auf Deutsch.
