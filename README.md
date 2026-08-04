# Digital-signature-creator
# Penned — Digital Signature Creator

A single-file, no-build web app for drawing, styling, and exporting a handwritten digital signature. Everything runs client-side in the browser — nothing is uploaded anywhere.

## Getting started

1. Download `digital-signature-creator.html`.
2. Open it in any modern browser (Chrome, Safari, Firefox, Edge). Double-click the file, or drag it into a browser window.
3. No server, no install, no build step required.

If you want to host it, just upload the single HTML file to any static host (GitHub Pages, Netlify, S3, etc.) — it has no dependencies besides two Google Fonts loaded over the network.

## How to use it

| Action | How |
|---|---|
| Draw | Click/tap and drag on the canvas. Works with mouse, touch, and stylus (pressure-sensitive on supported devices). |
| Change ink color | Click a swatch, or use the custom color picker (rainbow circle). |
| Change pen style | Fountain / Ballpoint / Marker — changes width variance and opacity. |
| Change line weight | Drag the Thin ↔ Thick slider. |
| Undo / Redo | Toolbar buttons, or `Ctrl+Z` / `Cmd+Z` to undo. |
| Clear | Toolbar trash icon (asks for confirmation first). |
| Stamp mode | Toggle in the side panel — adds a gold border and "Digitally Signed · [date]" seal to your export. |
| Transparent background | Toggle in the side panel — off exports on a white background. |
| Resolution | Standard (1×), High (2×), or Ultra (4×, print-quality). |
| Preview | Opens a modal showing exactly what will be downloaded. |
| Copy | Copies the export as a PNG to your clipboard. |
| Download | Saves the export as a PNG file. |
| Save | Adds the current signature to your local History gallery (also `Ctrl+S` / `Cmd+S`). |
| Dark mode | Sun/moon icon in the header. |

## Where things are stored

Saved signatures live in your browser's `localStorage` under the key `penned-signature-history`, capped at the 24 most recent. Clearing your browser data, using a different browser, or switching to a private/incognito window will not show previously saved signatures. Nothing is ever sent over the network — the app has no backend.

## File structure

It's intentionally one file so it's easy to share or host:

```
digital-signature-creator.html
├─ <style>   all CSS — design tokens (colors/type/spacing) at the top, then components
├─ <body>    header, hero, studio (canvas + control panel), history gallery, footer, modals
└─ <script>  vanilla JS, organized into commented sections:
    1. Theme (light/dark)
    2. Toast notifications
    3. SignaturePad class — the drawing engine
    4. Studio UI wiring (color/weight/style/stamp controls)
    5. Export system (download / copy / preview)
    6. LocalStorage history gallery
    7. Modals, keyboard shortcuts, scroll-reveal
```

## Customizing

- **Colors, fonts, spacing** — edit the CSS custom properties in the `:root` and `[data-theme="dark"]` blocks near the top of the `<style>` section.
- **Add a pen style** — add a case to `_effectParams()` in the `SignaturePad` class and a matching button in the `#penStyle` segmented control.
- **Change history limit** — edit the `.slice(0, 24)` call in `saveHistory()`.
- **Change export defaults** — edit the `resSelect` `<option>` values or the `transparentToggle`'s default `checked` state in the HTML.

## Browser support notes

- Clipboard copy (`btnCopy`) requires the [Clipboard API](https://developer.mozilla.org/en-US/docs/Web/API/Clipboard) with image support — available in current Chrome, Edge, and Safari. Firefox support varies; the app shows a toast if it's unsupported.
- Pressure sensitivity depends on the input device and browser reporting `PointerEvent.pressure`. Mouse input always uses a fixed mid-level pressure.
- `prefers-reduced-motion` is respected — animations are minimized for users who have that OS setting on.

## No dependencies

No npm, no build tooling, no external JS libraries — the smoothing, variable-width ink rendering, undo/redo, and export logic are all hand-written against the native Canvas and Pointer Events APIs. The only network requests are for the Google Fonts (Fraunces, Inter, Caveat); remove the `<link>` tags in `<head>` and swap in system fonts if you need it fully offline.
