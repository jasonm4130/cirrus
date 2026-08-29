# Cirrus

A terminal-register theme for [Cloudflare Nimbus](https://nimbus-docs.com) documentation
sites. Monospace as the structural voice, hairline rules instead of filled containers, and
a cool-chroma neutral ramp.

Named for the cloud genus: cirrus are the thin, high, fine-drawn ones — hairline strokes
across a lot of empty ground, which is the visual argument.

## What it looks like

Four decisions carry it, each falsifiable by looking at a rendered page:

- **Monospace is the display face.** Headings, nav items, table headers, breadcrumbs and
  figure captions are JetBrains Mono at 700. Only running prose is proportional. Nearly
  every docs theme, Nimbus's own included, sets headings in the sans and confines mono to
  code; committing mono to the heading role is the identity, and keeping body prose
  proportional is what stops it becoming a novelty.
- **No radii, no container fills.** Nimbus rounds code figures to `0.5rem` and tables to
  `0.75rem`, and fills both. Cirrus sets every one to `0` and replaces the fill with a
  rule. This is the single most visible departure.
- **The neutrals carry chroma.** Nimbus ships `oklch(0.99 0 0)` — literally zero chroma,
  pure grey. Cirrus sits at chroma 0.003–0.014 on hue 250. Imperceptible per swatch;
  across a page it reads cool rather than grey, and it is the difference between a palette
  that was chosen and one that was inherited.
- **Layout is on a character grid.** The mono cell is 8px by construction (13.3333px × 0.6
  advance). Content measure, spacing, table gutters and diagram coordinates are multiples
  of it.

What it deliberately is not: no CRT scanlines, no phosphor glow, no green-on-black, no
blinking caret, no faux window titlebar. Those are a costume worn over the register rather
than the register itself.

## Install

```sh
npm i @jasonm4130/cirrus
```

Then replace the body of your scaffolded `src/styles/globals.css` with:

```css
@import "@jasonm4130/cirrus/globals.css";
```

Cirrus is a full replacement for that file, not an overlay — it keeps Nimbus's own
structure (`--nb-*` on `:root`, re-declared under `[data-mode="dark"]`, re-exported
through Tailwind's `@theme`) and only re-points the values.

## What it overrides beyond tokens

Four rules, because Nimbus has no token for either concern — radii are Tailwind utility
classes on components, and headings inherit `--font-sans`:

| Rule | Why |
|---|---|
| `border-radius: 0` on everything | No radius token exists |
| `box-shadow: none` on everything | Shadows fight a hairline system |
| Mono on headings, nav, `th`, captions | No heading-font token exists |
| 12px type in the sidebar and TOC | Mono runs ~20% wider than Inter; real page titles wrap otherwise |

Everything else is token re-pointing. **No component file is edited** — verified by
building a scaffolded project, changing only this file, and confirming the change reached
the page ground, the selected nav item, every border, and the TOC active rail.

## Two things Nimbus 0.11 gets wrong that Cirrus patches

**`@custom-variant dark` is missing from the starter.** It is registered in Nimbus's own
`apps/www` but not in the template a scaffolded project receives, so Tailwind's `dark:`
utilities fall back to `prefers-color-scheme` while the tokens follow `data-mode` —
despite the theming docs saying they share one signal. Cirrus ships the line.

**No-JS visitors always render light.** Nimbus resolves the theme in a pre-paint inline
script that reads `localStorage['ui-mode']`, falls back to `matchMedia`, and sets or
removes `data-mode="dark"` — light being the *absence* of the attribute. There is no
`prefers-color-scheme` media query anywhere as a floor.

## Extending it for your project

Cirrus ships neutrals and a status set. Domain tones are yours: declare them after the
import and reference them from your own components and inline SVG.

```css
@import "@jasonm4130/cirrus/globals.css";

:root {
  --t-capture: #007935;
  --t-model:   #9649a7;
}
[data-mode="dark"] {
  --t-capture: #4daf67;
  --t-model:   #c783d7;
}
```

Colour reinforces, it never encodes. Every toned element must also carry a text label:
five hues in the AA-legible band do not survive colour-vision deficiency — under
deuteranopia and tritanopia the closest pairs collapse to near-zero separation in oklab,
and a lightness ladder makes light mode worse, not better.

## Status

Pre-1.0, and so is Nimbus. Pinned to `@cloudflare/nimbus-docs` 0.11.x because the token
file's structure is what Cirrus replaces, and that structure can move.

## Licence

MIT
