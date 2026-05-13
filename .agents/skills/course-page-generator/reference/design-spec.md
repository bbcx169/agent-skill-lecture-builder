# Design Spec

This document defines the shared visual language for:

- `reference/base.html` long-form course pages
- `reference/deck-template.html` HTML Deck presentations

The default style is **Modern Magazine**: clean, editorial, confident, image-led, and highly structured.

## 1. Brand Direction

- Theme: `Modern Magazine`
- Tone: clean, editorial, confident
- Visual identity: bold imagery, strong hierarchy, generous white space
- Primary use: lecture pages, public briefings, policy explainers, case decks, and training materials that need a polished editorial feel.

The design should feel like a contemporary magazine feature, not a dark dashboard or a generic SaaS landing page.

## 2. Color Tokens

Use a restrained black, white, red, and neutral palette.

```text
--bg: #ffffff
--bg-card: #ffffff
--bg-card-alt: #f2f2f2
--text: #111111
--text-dim: #555555
--border: #d8d8d8
--accent: #e10600
--accent-rgb: 225 6 0
--accent2: #111111
--accent2-rgb: 17 17 17
--accent3: #777777
--accent3-rgb: 119 119 119
--code-bg: #f2f2f2
--radius: 8px
```

Usage rules:

- `--bg` is the page or slide background.
- `--bg-card` is used for content cards.
- `--bg-card-alt` is used for neutral bands, image placeholders, and secondary panels.
- `--text` is for primary headings and important body text.
- `--text-dim` is for supporting copy.
- `--accent` is the only strong color and should be used sparingly.
- `--accent2` supports black rules, labels, and high-emphasis metadata.
- `--accent3` is neutral support, not a third brand color.

## 3. Typography

Use sans-serif typography for both headings and body copy.

- Headings: `"Noto Sans TC", "Microsoft JhengHei", "PingFang TC", sans-serif`
- Body: `"Noto Sans TC", "Microsoft JhengHei", "PingFang TC", sans-serif`
- Mono/meta: `Consolas, "Courier New", monospace`

Rules:

- Use bold weight for editorial hierarchy.
- Keep letter spacing at `0` for normal headings and body text.
- Use uppercase metadata sparingly, only for small labels such as `QUESTION`, `DETAIL`, or `WORKFLOW`.
- Body line-height should stay near `1.6`.

## 4. Layout System

### Long Page

- Use a bright editorial reading surface.
- Hero should feel like a magazine cover: large title, short dek/subtitle, clear badge, and a strong accent rule.
- Section blocks should use generous vertical spacing.
- Cards should be clean and flat, not heavy glass panels.

### HTML Deck

- Use 16:9 slide composition with strong hierarchy.
- Cover and closing slides may use full-bleed imagery with headline overlay.
- Detail slides should favor editorial split layouts: one strong image, one strong text column.
- Data or policy slides should use comparison tables, action cards, and decision cards with restrained red emphasis.

## 5. Layout Variations

Use these named layout patterns when generating decks:

- `full_bleed_cover`: full-page image with large headline overlay.
- `editorial_split`: two-column text/image balance.
- `feature_highlight`: one key message with red accent emphasis.
- `image_grid`: grid-based image layout with captions.
- `quote_focus`: large quotation text with minimal supporting elements.

## 6. Cards And Panels

Base card:

```text
background: var(--bg-card)
border: 1px solid var(--border)
border-radius: var(--radius)
```

Rules:

- Keep cards at `8px` radius.
- Avoid heavy shadows; use borders, spacing, and accent rules for hierarchy.
- Use accent red only for key labels, numbers, rules, or active states.
- Do not nest decorative cards inside larger cards.

## 7. Imagery

- Prefer high-resolution editorial-style photography.
- Use images that show the actual topic, setting, object, process, or outcome.
- Avoid purely atmospheric or decorative stock-like images.
- Image containers should use stable aspect ratios such as `16 / 10`.
- Captions and image tags should be small, high contrast, and unobtrusive.

## 8. Emphasis

- Use accent red sparingly.
- Prefer a single red rule, badge, number, progress bar, or underline per visual group.
- Avoid red backgrounds behind large paragraphs.
- If everything is red, nothing is emphasized.

## 9. Tables And Data Visuals

- Use black text, neutral borders, and red table headers or active sort states.
- Use charts only when the source provides real quantitative data.
- If no real data exists, use action cards, comparison matrices, process flows, or decision cards.

## 10. Motion

- Motion should support navigation only.
- Keep hover movement subtle.
- Avoid constant decorative animation.

## 11. Accessibility

- Maintain strong contrast between black text and white/neutral backgrounds.
- Do not rely on color alone to indicate state.
- Keep click targets large enough for presentation use.
- Use readable image alt text.

## 12. Anti-Patterns

Avoid:

- Dark dashboard styling as the default.
- Purple or multi-hue gradient palettes.
- Decorative gradient blobs or orbs.
- Heavy glassmorphism.
- Overly rounded pill-heavy interfaces.
- Dense text slides without a clear visual hierarchy.
- Stock-like images that do not help explain the content.

## 13. Source Of Truth

Main implementation files:

- [base.html](C:/Users/bbcx1/agent-skill-lecture-builder/.agents/skills/course-page-generator/reference/base.html:42)
- [deck-template.html](C:/Users/bbcx1/agent-skill-lecture-builder/.agents/skills/course-page-generator/reference/deck-template.html:8)

When tokens, typography, card rules, or image rules change, update this spec and both templates together.
