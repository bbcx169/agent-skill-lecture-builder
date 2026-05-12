---
name: course-page-generator
description: 將原始講稿、法規、報告或非結構化筆記轉換為結構化 Markdown，支援生成單頁課程網頁，也可依公務簡報藍圖產生 HTML Deck 簡報。Use when Codex needs to create or update content.md, config.yaml, index.html, slides.html, or OG images for a course/lecture/briefing page.
---

# Course Page Generator

Turn source material into a maintainable course artifact:

`source material -> content.md -> config.yaml -> index.html -> assets/og-image.jpg`

Use `content.md` as the single knowledge master. Every generated page or deck must derive from it.

## Core Workflow

1. Identify the input type.
   - Topic only: create a new course folder, `config.yaml`, `content.md`, and `assets/`.
   - Existing draft or transcript: convert it into structured `content.md`.
   - Existing course folder: read its `content.md`, `config.yaml`, and relevant assets before editing.
   - Public-sector briefing: use the public briefing rules below.

2. Create or update `content.md`.
   - Extract meaning instead of transcribing speech.
   - Use sections, cards, flows, insights, summaries, and images.
   - Avoid loose paragraphs when a structured component fits.
   - Keep evidence comments in `<!-- source: ... -->` when facts must be traceable.
   - Read [components.md](reference/components.md) when component syntax or rendering behavior matters.

3. Create or update `config.yaml`.
   - Run `git remote get-url origin` before writing SEO URLs.
   - Derive GitHub Pages base URL when the remote is GitHub:
     - `git@github.com:user/repo.git` -> `https://user.github.io/repo`
     - `https://github.com/user/repo.git` -> `https://user.github.io/repo`
   - Set `seo.url` to `{GH_BASE}/{course-dir}/`.
   - Set `seo.image` to `{GH_BASE}/{course-dir}/assets/og-image.jpg`.
   - Leave SEO URL fields empty if no GitHub remote can be derived, and tell the user.
   - Use [config-example.yaml](reference/config-example.yaml) for complete field examples.

4. Build from the repo root.

   ```bash
   node .agents/skills/course-page-generator/scripts/build.mjs <course-dir>
   ```

5. Immediately generate the OG image after any successful build.

   ```bash
   node .agents/skills/course-page-generator/scripts/generate-og.mjs <course-dir>
   ```

   This writes `<course-dir>/assets/og-image.jpg`. Do not skip this step after build unless the user explicitly asks to skip it or the environment cannot run Puppeteer.

6. Verify the result.
   - Confirm `index.html` was generated.
   - Confirm `assets/og-image.jpg` exists after OG generation.
   - For visual or layout changes, open or inspect the generated HTML when practical.

## Course Folder Rules

When the user gives only a topic:

1. Convert the topic to an English kebab-case folder name.
2. If the user specified a path, use it.
3. Otherwise inspect existing repo folders and follow local convention such as `course/`, `lectures/`, or a root-level course folder.
4. If there is no convention, create the course folder at the repo root.
5. Create:

   ```text
   <course-dir>/
   ├── config.yaml
   ├── content.md
   └── assets/
   ```

6. Generate a buildable `content.md` skeleton with 3-5 main sections, useful TODO comments, and a final `[summary]` block.

## Content Rules

Use this structure by default:

```markdown
# LABEL：TITLE
> section lead

## Subsection

### Emoji Card Title
- concise point
- concise point

[summary]
- **Takeaway** | Practical implication
[/summary]
```

Key rules:

- `#` defines main sections; `##` defines subsections; `###` defines cards.
- Use `### Emoji Title` for teaching cards.
- Use `> **Title**` blockquotes for insights.
- Use `[flow]...[/flow]` for processes.
- Use `[tags]...[/tags]` for colored labels.
- Use `[summary]...[/summary]` for the final synthesis.
- Use `` ```prompt [label="..."] `` for prompts, commands, and terminal snippets.
- Use `![alt](assets/file.ext)` or `[image-text]...[/image-text]` for local images.
- Use `<!-- TODO: ... -->` for missing source material or suggested images.
- Before final build, resolve or remove `[visual-prompt]...[/visual-prompt]`; it is a planning artifact, not final page content.

When source material includes images or the content clearly needs images:

1. Search `<course-dir>/assets/` for `*.png`, `*.jpg`, `*.jpeg`, `*.gif`, `*.svg`, and `*.webp`.
2. Insert the best matching local image.
3. If no image exists, leave a TODO comment describing the needed image and mention it in the final response.

## General Course Vs Public Briefing

Use **general course mode** when the goal is to teach knowledge, explain concepts, demonstrate skills, or turn a lecture into a self-study page.

- Purpose: knowledge transfer, skill decomposition, case teaching.
- Structure: 3-5 topic sections that gradually build understanding.
- Writing: concise teaching cards, process flows, examples, prompts, insights, and a final learning summary.

Use **public briefing mode** when the source is a regulation, government report, policy memo, inspection case, meeting material, administrative guidance, ruling request, or when the user explicitly asks for a briefing/deck for decision makers.

- Purpose: information sync, option recommendation, or decision/ruling.
- Structure: Context -> Complication -> Resolution.
- Writing: every `###` title states a conclusion or decision-relevant judgment.
- Evidence: each law, number, date, agency name, penalty basis, or meeting conclusion gets `<!-- source: ... -->`.
- Visual principle: visuals guide attention; text preserves evidence.
- Deck principle: each slide answers one key question.

## Public Briefing Hard Rules

For public-sector content:

1. Use decision-oriented titles, not topic labels.
   - Prefer: `### 預算餘額足以支應第四季擴大辦理`
   - Avoid: `### 預算狀況`
2. Keep traceability in `content.md` with hidden source comments.
3. Do not invent numbers, legal interpretations, agencies, decisions, or examples.
4. Use charts only when the source has real quantitative data.
5. Prefer action cards, comparison matrices, before/after framing, and workflows when quantitative data is absent.
6. The final `[summary]` must synthesize takeaways and next actions.

## HTML Deck Mode

Use the normal course-page flow by default.

Use HTML Deck Mode only when the user explicitly asks for HTML slides, interactive web slides, a shareable slide deck, Chart.js/table/decision-tree slides, or a presentation separate from the long-form page.

Deck mode rules:

- `content.md` remains the single source of truth.
- Create or update `content.md` before deriving `slides.html`.
- The deck may compress, reorder, and rewrite wording for live presentation.
- The deck must not add claims, examples, decisions, or conclusions unsupported by `content.md`.
- Use [html-deck-mode.md](reference/html-deck-mode.md) and start from [deck-template.html](reference/deck-template.html).
- Produce one standalone `<course-dir>/slides.html` with inline CSS/JS and local images referenced relative to `slides.html`.

For public-sector decks:

- Preserve source comments in `content.md`, but do not render them in `slides.html`.
- Use charts only with real data.
- The closing slide must synthesize `[summary]` into takeaways and next actions.

## Reference Files

- Component syntax and rendering: [components.md](reference/components.md)
- Visual design rules: [design-spec.md](reference/design-spec.md)
- Config fields and examples: [config-example.yaml](reference/config-example.yaml)
- Markdown example: [content-example.md](reference/content-example.md)
- Course page template: [base.html](reference/base.html)
- HTML deck rules: [html-deck-mode.md](reference/html-deck-mode.md)
- Standalone deck template: [deck-template.html](reference/deck-template.html)
