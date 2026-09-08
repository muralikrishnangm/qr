# QR codes for slides

**→ [muralikrishnangm.github.io/qr](https://muralikrishnangm.github.io/qr/)**

A QR code generator for presentation slides. Set your colours, drop a logo in
the middle, download vector or raster. It runs entirely in your browser — one
HTML file, no dependencies, no build step, and no data leaves the page.

## Features

- Foreground and background colours, by picker or hex value
- Transparent background for PNG and SVG
- Square, rounded, or dot modules, with an adjustable quiet-zone border
- A centre mark: an uploaded logo or a short text label, on a square or circular
  backdrop
- Export to **PNG**, **JPEG**, **SVG**, and **PDF** — SVG and PDF are true
  vector, so they stay sharp however large the projection

The page also flags choices that tend to break scanners: contrast below roughly
3:1, a background darker than the modules, a border narrower than the standard
four modules, or a centre mark larger than the error-correction level can
rebuild.

## Two things worth setting up once

**Your logo, already loaded.** Commit a `logo.png` next to `index.html` and the
page picks it up on load, so you don't re-upload it every time.

**Your colours, one bookmark away.** *Copy settings link* gives you a URL that
reopens the page with the same colours, shapes, and options — bookmark it as
your house style. The uploaded image isn't carried in the link; that's what
`logo.png` is for.

## Choosing settings for a talk

Error correction **M** is fine for a plain code. Once a centre mark is in play,
use **Q** or **H** — the mark covers real data, and the higher level is what
lets a scanner reconstruct it. The page tells you when the mark has outgrown
that budget.

Keep the URL short. A long link pushes the code to a higher version, meaning
more and smaller modules, which is what makes a code hard to catch from the back
of a lecture hall. Version 10 or below stays comfortably chunky.

Square modules scan fastest. Rounded and dots look better in a deck and still
read reliably, but test them with your own phone before the talk.

## Running it elsewhere

`index.html` is self-contained. Open it straight from disk, drop it on any
static host, or take it to a conference room with no network. Nothing else in
this repository is required.

Deployed here from the default branch root via **Settings → Pages**.

## Scripting it

The page exposes a small API on `window.QRPage` for driving it from the console
or generating a batch:

```js
Object.assign(QRPage.settings, { text: "https://example.org", ecl: "H", fg: "#123f6d" });
QRPage.syncControls();
QRPage.render();
const svg = QRPage.buildSVG();   // string
const pdf = QRPage.buildPDF();   // Uint8Array
```

## How it was checked

The QR encoder is written from scratch to ISO/IEC 18004 rather than loaded from
a CDN, so the page keeps working offline and can't break when a third party
changes something.

It was verified two ways. Output matrices were compared module-for-module
against two independent reference implementations across all 40 versions, all
four error-correction levels, and all three encoding modes — around a thousand
matrices, exact match. Then every export path was rendered back to an image and
decoded with ZXing, the library behind most phone scanning apps: PNG, SVG, and
PDF across ten configurations, each decoding to the exact source text.
