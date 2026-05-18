---
name: course-page-generator
description: 將原始講稿、法規、報告或非結構化筆記轉換為結構化 Markdown，支援生成單頁課程網頁，也可依公務簡報藍圖產生 HTML Deck 簡報。Use when Codex needs to create or update content.md, config.yaml, index.html, slides.html, or OG images for a course/lecture/briefing page.
---

# Course Page Generator

Turn source material into a maintainable course artifact:

`source material -> content.md + config/global.yaml + config.yaml -> index.html -> assets/og-image.jpg`

Use `content.md` as the single knowledge master. Every generated page or deck must derive from it.

## Core Workflow

1. Identify the input type.
   - Topic only: create a new course folder, `config/global.yaml`, `config.yaml`, `content.md`, and `assets/`.
   - Existing draft or transcript: convert it into structured `content.md`.
   - Existing course folder: read its `content.md`, `config/global.yaml`, `config.yaml`, and relevant assets before editing.
   - Public-sector briefing: use the public briefing rules below.

2. Create or update `content.md`.
   - Extract meaning instead of transcribing speech.
   - Use sections, cards, flows, insights, summaries, and images.
   - Avoid loose paragraphs when a structured component fits.
   - Keep evidence comments in `<!-- source: ... -->` when facts must be traceable.
   - Read [components.md](reference/components.md) when component syntax or rendering behavior matters.

3. Create or update `config/global.yaml` and `config.yaml`.
   - `config/global.yaml` is required for every course folder.
   - When creating a new course folder or when an existing course folder lacks `config/global.yaml`, automatically copy the fixed source config from `C:\Users\bbcx1\agent-skill-lecture-builder\config` into `<course-dir>/config/`.
   - Copy both `C:\Users\bbcx1\agent-skill-lecture-builder\config\global.yaml` and `C:\Users\bbcx1\agent-skill-lecture-builder\config\assets\` into the course folder.
   - Treat `C:\Users\bbcx1\agent-skill-lecture-builder\config` as read-only reference material; never modify files in that source directory.
   - Store shared instructor, socials, footer, shared SEO defaults, favicon, avatar, and shared config assets in `config/global.yaml`.
   - Store course-specific page title, subtitle, course SEO overrides, nav, and source list in `<course-dir>/config.yaml`.
   - The generator only reads `<course-dir>/config/global.yaml`; do not rely on a parent-folder `config/global.yaml`.
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
   ├── config/
   │   ├── global.yaml
   │   └── assets/
   ├── config.yaml
   ├── content.md
   └── assets/
   ```

6. Populate `<course-dir>/config/` by copying from the fixed source config directory:

   ```text
   C:\Users\bbcx1\agent-skill-lecture-builder\config\global.yaml
   C:\Users\bbcx1\agent-skill-lecture-builder\config\assets\
   ```

   Copy into:

   ```text
   <course-dir>/config/global.yaml
   <course-dir>/config/assets/
   ```

   Do not edit the fixed source config directory.

7. Generate a buildable `content.md` skeleton with 3-5 main sections, useful TODO comments, and a final `[summary]` block.

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
- Use `[video src="assets/videos/file.mp4" title="..."]` as the preferred video component when a local MP4 is available.
- Use `[youtube id="..." title="..."]` only as a backup when no local MP4 is available or when the user explicitly requests YouTube embed.
- Use `[button href="..." label="..."]` for important external source actions, such as opening the original YouTube video when YouTube is used as a backup or reference.
- Use `` ```prompt [label="..."] `` for prompts, commands, and terminal snippets.
- Use `![alt](assets/file.ext)` or `[image-text]...[/image-text]` for local images.
- Use `<!-- TODO: ... -->` for missing source material or suggested images.
- Before final build, resolve or remove `[visual-prompt]...[/visual-prompt]`; it is a planning artifact, not final page content.

When source material includes images or the content clearly needs images:

1. Search `<course-dir>/assets/` for `*.png`, `*.jpg`, `*.jpeg`, `*.gif`, `*.svg`, and `*.webp`.
2. Insert the best matching local image.
3. If no image exists, leave a TODO comment describing the needed image and mention it in the final response.

When source material includes DOCX/PDF images:

1. Extract images into `assets/<source-stem>/`, where `<source-stem>` is a stable English kebab-case name derived from the source file.
2. For DOCX, preserve document order when naming extracted images; do not rely on ZIP media filename order.
3. For PDF, extract only the relevant page images unless the user asks for all pages.
4. Place each extracted image next to the matching clause, step, table, or explanation in `content.md`; do not dump all images into one gallery.
5. Use specific captions that describe the image's role, such as `圖八-1：檢查壓力調整器墊片`, not generic labels like `照片 01`.

When source material includes videos:

1. Prefer local MP4 with `[video]` when a usable local video file exists.
2. Copy local MP4 files to `assets/videos/` with safe English filenames before using `[video]`.
3. Use YouTube embed with `[youtube]` as a backup when no local MP4 exists, when the MP4 is unavailable, or when the user explicitly asks for YouTube embed.
4. If both local MP4 and YouTube URL exist, embed the local MP4 first; optionally add a `[button]` link to open the original YouTube source.
5. Avoid unsafe video filenames in HTML paths, especially spaces, `#`, and long CJK names.

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
- Media extraction and placement workflow: [media-workflow.md](reference/media-workflow.md)
- Markdown example: [content-example.md](reference/content-example.md)
- Course page template: [base.html](reference/base.html)
- HTML deck rules: [html-deck-mode.md](reference/html-deck-mode.md)
- Standalone deck template: [deck-template.html](reference/deck-template.html)
