# Cirrus

A terminal-register theme for [Cloudflare Nimbus](https://nimbus-docs.com) documentation
sites. Monospace as the structural voice, hairline rules instead of filled containers, and
a cool-chroma neutral ramp.

Named for the cloud genus: cirrus are the thin, high, fine-drawn ones — hairline strokes
across a lot of empty ground, which is the visual argument.

## What it looks like

Four decisions carry it, each falsifiable by looking at a rendered page:

- **Monospace is the display face.** Headings are JetBrains Mono at 700; nav items, table
  headers, badges, figure captions and inline code follow in mono at inherited weight.
  Only running prose is proportional. Nearly every docs theme, Nimbus's own included,
  sets headings in the sans and confines mono to code; committing mono to the heading
  role is the identity, and keeping body prose proportional is what stops it becoming a
  novelty.
- **No radii.** Nimbus rounds code figures to `0.5rem` and tables to `0.75rem`. Cirrus
  zeroes every radius and lets the existing hairline border carry the container. This is
  the single most visible departure.
- **The neutrals carry chroma.** Nimbus ships `oklch(0.99 0 0)` — literally zero chroma,
  pure grey. Cirrus sits at chroma 0.003–0.015 across the neutral ramp on hues around 250
  (the dark accent runs to 0.024). Imperceptible per swatch; across a page it reads cool
  rather than grey, and it is the difference between a palette that was chosen and one
  that was inherited.
- **Layout is on a character grid.** The consuming site's mono cell is 8px by construction
  at its root mono size; the content measure and TOC width this file sets are
  multiples of that cell.

What it deliberately is not: no CRT scanlines, no phosphor glow, no green-on-black, no
blinking caret, no faux window titlebar. Those are a costume worn over the register rather
than the register itself.

## Install

```sh
npm i github:jasonm4130/cirrus#v0.1.0   # pinned
npm i github:jasonm4130/cirrus          # tracks main
```

Cirrus is not published to npm, so install it from GitHub. Do not install the bare name
`cirrus` — that is an unrelated package on the registry.

Then replace the body of your scaffolded `src/styles/globals.css` with:

```css
@import "@jasonm4130/cirrus/globals.css";
```

Cirrus is a full replacement for that file, not an overlay — it keeps Nimbus's own
structure (`--nb-*` on `:root`, re-declared under `[data-mode="dark"]`, re-exported
through Tailwind's `@theme`) and only re-points the values.

## What it overrides beyond tokens

Four rules in the table below, because Nimbus has no token for either concern — radii
are Tailwind utility classes on components, and headings inherit `--font-sans`:

| Rule | Why |
|---|---|
| `border-radius: 0` on everything | No radius token exists |
| `box-shadow: none` on everything | Shadows fight a hairline system |
| Mono on headings, nav, `th`, captions | No heading-font token exists |
| 12px type in the sidebar and TOC | Mono runs ~20% wider than Inter; real page titles wrap otherwise |

Also shipped, outside the table above: a banded metadata header on
`article.docs-content > p:first-child:has(> strong:first-child)` — any docs page whose
first paragraph opens with a bold lead-in gets it styled as an ADR-style record header
(mono, uppercased label, bottom rule); and 1.9rem of top padding on `.nb-code-figure >
pre` to reserve the language-chip strip.

Everything else is token re-pointing. **No component file is edited** — verified by
building a scaffolded project, changing only this file, and confirming the change reached
the page ground, the selected nav item, every border, and the TOC active rail.

## Two Nimbus 0.11 gaps: one patched, one you should know about

**`@custom-variant dark` is missing from the starter.** It is registered in Nimbus's own
`apps/www` but not in the template a scaffolded project receives, so Tailwind's `dark:`
utilities fall back to `prefers-color-scheme` while the tokens follow `data-mode` —
despite the theming docs saying they share one signal. Cirrus ships the line.

**No-JS visitors always render light.** Nimbus resolves the theme in a pre-paint inline
script that reads `localStorage['ui-mode']`, falls back to `matchMedia`, and sets or
removes `data-mode="dark"` — light being the *absence* of the attribute. Cirrus does not
add a `prefers-color-scheme` floor for this: the tokens follow `data-mode` alone, and a
media-query floor would fight that signal the moment JS does run. Know this before
shipping — a no-JS visitor with a dark OS preference still gets light.

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

MIT — see [LICENSE](LICENSE).

globals.css is derived from the Nimbus starter theme
(`packages/nimbus-starter-source/src/styles/globals.css` in
[cloudflare/nimbus](https://github.com/cloudflare/nimbus)), Copyright (c) 2025
Cloudflare, Inc., MIT Licensed. Cirrus keeps that file's structure and comments and
re-points its values; the section headed CIRRUS at the end is new. Cloudflare's notice
is retained in LICENSE and in the file header.
