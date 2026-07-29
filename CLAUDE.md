# LibGuides Bootstrap 3 → 5 migration

Custom templates and code for the Aubrey R. Watzek Library (Lewis & Clark College)
LibGuides site, being migrated from Bootstrap 3 to Bootstrap 5.

## Goals

1. Make the BS5 preview match the BS3 production look.
2. Improve accessibility — aim for high WAVE scores (labels, landmarks, contrast,
   alt text, heading order, etc.).

## File layout

Files are paired: `*-bs3.*` is the **production reference**, `*-bs5.*` is the
**work in progress**. Edit only the `-bs5` files.

- `header-bs3.html` / `header-bs5.html`
- `footer-bs3.html` / `footer-bs5.html`
- `homepage-bs3.html` / `homepage-bs5.html`
- `custom-js-bs3.js` / `custom-js.bs5.js`
- `watzek-style-bs3.css` / `watzek-style-bs5.css`

**All CSS/style changes go in `watzek-style-bs5.css`.** It `@import`s Bootstrap 5
and Bootstrap Icons at the top. `html { font-size: 62.5% }` (10px root) is
intentional — every rem value in the file is designed around it; do not remove it.

## Testing

These are backend fragments, not standalone pages — they have no `<link>`/`<script>`
to Bootstrap or the stylesheet, so **they cannot be previewed on their own**. Each
file is pasted into a different spot in the LibGuides backend by the user.

- Live BS5 preview: https://library.lclark.edu/?bs5=1
- Production (BS3): https://library.lclark.edu/

To verify a change, inspect the **live** pages in the browser (measure the DOM via
JS, compare BS5 against production). A local test harness that links the CSS and
wraps a fragment can confirm the CSS parses, but won't reproduce the LibGuides
system stylesheets (e.g. `lg-public.min.css`) that affect container widths and the
navbar — don't trust its layout, only the live pages.

## BS3 → BS5 gotchas seen so far

- **Removed float-grid clearfix.** BS3's `.container`/`.container-fluid` had a
  `::before` clearfix that stopped child top-margins from collapsing through. BS5
  dropped it, so `.header-brand`'s `margin-top` collapsed up and pushed the header
  background down. Fix: `display: flow-root` on the containing element.
- **Root font-size.** The LibGuides BS3 template set the `html` root to 10px; the
  BS5 template reverts to 16px, scaling every rem 1.6×. Restored with
  `html { font-size: 62.5% }`.
- **Boxed vs full-width containers.** BS3 nav used `.container` (boxed, aligns with
  the brand block); a BS5 draft used `.container-fluid`, breaking alignment.
- Removed BS3 utility/grid classes to translate: `form-inline`/`form-group`,
  `hidden-*`/`visible-*` → `d-none`/`d-*-block`, `col-xs-*` → `col-*`,
  `navbar-right` → `ms-auto`, `glyphicon` → Bootstrap Icons (`bi`),
  `sr-only` → `visually-hidden`, `data-toggle` → `data-bs-toggle`.
