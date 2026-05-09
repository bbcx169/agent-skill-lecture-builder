# HTML Deck Mode

Use HTML deck mode only when the requested output is an interactive slide deck,
not a long-form course page. The normal Course Page Generator flow remains:
`content.md` + `config.yaml` -> `index.html` + OG image.

HTML deck mode is a separate optional artifact, but it must use `content.md` as
the single source of truth. First produce or update `content.md` as the complete
knowledge master, then derive `slides.html` from that file. The slide deck may
compress, reorder, and rewrite the wording for live presentation, but it must
not add claims, examples, decisions, or conclusions that conflict with or are
unsupported by `content.md`.

Deck mode should not depend on the course page `base.html`, presentation mode,
or print/PDF conversion logic.

## Source Workflow

1. Create or update `<course-dir>/content.md` from the user's source material.
2. Treat `content.md` as the canonical knowledge master.
3. Build the course page when requested with the normal `build.mjs` flow.
4. Create `slides.html` by summarizing and staging the same `content.md`.
5. Before delivery, compare the deck against `content.md` for contradictions or
   unsupported additions.

## Output Contract

- Produce one standalone `.html` file, usually `slides.html`.
- Keep `slides.html` in the same course directory as `content.md` unless the
  user asks for a different output location.
- Keep CSS and JavaScript inline unless a CDN is clearly useful, such as
  Chart.js or web fonts.
- Use real HTML text for slide titles, paragraphs, tables, chart labels,
  decision nodes, and CTA text.
- Do not turn the deck into a sequence of full-slide images. Images are visual
  assets, except for intentional cover or closing hero backgrounds.
- Embed local images as `data:image/...;base64,...` before final delivery.
- Keep the deck browser-openable without a dev server.

## Recommended Sequence

Use 10-12 slides unless the user gives a different count:

1. Cover: full-bleed hero image or strong title page.
2. Question: one large problem statement.
3. Thesis: the core reframing.
4. Overview: three clickable cards.
5. Detail: split image/text.
6. Detail: split image/text, flipped.
7. Detail: split image/text.
8. Comparison: visual plus sortable table.
9. Data: lazy Chart.js chart.
10. Decision: clickable decision tree or choice cards.
11. Workflow: 2x3 method grid.
12. CTA: full-bleed closing page.

## Required Interaction

Include at least two meaningful interactions when the content permits:

- Clickable overview cards that jump to detail slides.
- Sortable comparison table columns.
- Lazy chart rendering when the chart slide is first opened.
- Clickable decision tree or choice cards.
- Feature toggle buttons that change visible content.

## Navigation Rules

- ArrowRight, Space, PageDown: next slide.
- ArrowLeft, PageUp: previous slide.
- F: fullscreen.
- Click right 30% of viewport: next slide.
- Click left 30% of viewport: previous slide.
- Ignore click-to-navigate on buttons, links, table headers, cards, and images.
- Update progress, section label, and page count on every slide change.

## Layout Rules

- Use full viewport slides with `position:absolute; inset:0`.
- Do not use a fixed 1920x1080 stage with global transform scaling.
- Use responsive grids that collapse to one column on small screens.
- Each slide should have one core message.
- Keep titles short and scannable.
- Alternate split layouts to create a Z-pattern.
- Match the course page `base.html` visual system: dark `#0f1117`
  background, `#181b25` / `#1e2130` panels, warm coral/gold accents
  (`#e8845a`, `#e8b878`, `#e08898`), `Noto Serif TC` headings, and
  `Noto Sans TC` body text.

## Template

Start from `reference/deck-template.html`. Replace slide content, images,
table rows, chart data, and decision choices with material derived from
`content.md`. Keep the navigation functions and validation checklist unless
there is a specific reason to change them.

## Validation Checklist

Before delivery, inspect the generated deck for:

- Planned number of `<section class="slide">` elements.
- Every slide's claim is traceable to `content.md`.
- No slide introduces a point that contradicts `content.md`.
- Every local `<img>` source is a base64 data URI.
- `goto(n)` updates the active slide, progress bar, section tag, and page count.
- Table sorting changes actual table rows.
- Chart.js initializes only when the chart slide becomes active.
- Decision choices are real clickable HTML elements.
- No references to missing global DOM ids.
- Text does not overflow controls or overlap adjacent content.
