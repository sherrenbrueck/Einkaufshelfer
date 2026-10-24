# Mein Einkaufsgeld

Einkaufs-App für einen jungen Erwachsenen mit Förderschwerpunkt geistige Entwicklung. Er fotografiert den Strichcode eines Lebensmittels, die App erkennt das Produkt und zeigt:

- ob das Wochenbudget (Standard: 20 €) noch reicht, mit Geldbalken und Warnung, wenn das Geld alle ist,
- eine Lebensmittel-Ampel (grün = gesund, gelb = ab und zu, rot = nur selten, abgeleitet vom Nutri-Score),
- alles zusätzlich als Sprachausgabe.

## Dateien

| Datei | Zweck |
| --- | --- |
| `index.html` | Eigenständige Version, läuft in jedem Browser (z. B. über GitHub Pages). |
| `artifact.html` | Quelle des Claude-Artefakts (ohne HTML-Gerüst, das claude.ai beim Veröffentlichen ergänzt). |

## Betreuer-Bereich

Unten auf „Betreuer“ tippen. Dort lassen sich Budget, Produkte, Preise, Barcodes, Ampel und Fotos pflegen. Mit „Schnell einscannen“ erfasst man im Laden ein Produkt nach dem anderen.

**Speichern:** Der Knopf „Speichern“ funktioniert nur in der claude.ai-Version (er veröffentlicht die App neu). In `index.html` ändert man die Produktliste direkt im Block `<script type="application/json" id="daten">`.

## Hinweise

- Preise sind Schätzwerte (Stand Oktober 2026), Getränke inklusive Pfand. Bitte mit dem Kassenbon abgleichen.
- Der Einkaufskorb wird nur im Browser des Geräts gespeichert.
- Barcode-Erkennung: BarcodeDetector des Browsers, sonst die Bibliothek ZXing (per CDN).
