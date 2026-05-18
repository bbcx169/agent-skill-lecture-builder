# Media Workflow

Use this workflow when a course source includes videos, DOCX images, PDF pages, diagrams, or screenshots.

## Video Priority

1. Prefer local MP4 when a usable local video file exists.
2. Copy local MP4 files to `assets/videos/` with safe English kebab-case filenames before embedding them.
3. Use YouTube embed only as a backup when no local MP4 exists, the MP4 is unavailable, or the user explicitly requests YouTube embed.
4. If both exist, use `[video]` first and add a `[button]` link to the original YouTube source when useful.
5. Avoid unsafe video filenames in HTML paths, especially spaces, `#`, and long CJK names.

Recommended:

```markdown
[video src="assets/videos/demo-video.mp4" title="影片標題"]
[button href="https://www.youtube.com/watch?v=VIDEO_ID" label="在 YouTube 開啟原始影片"]
```

YouTube backup:

```markdown
[youtube id="VIDEO_ID" title="影片標題"]
```

## DOCX Image Extraction

1. Extract images into `assets/<source-stem>/`.
2. Derive `<source-stem>` from the source document using stable English kebab-case.
3. Preserve document order when naming images, such as `08-guidance-01.jpg`.
4. Do not rely on ZIP media filenames such as `image13.png`; those often do not match document order.
5. Place each image immediately after the matching clause, checklist item, table, or process step.
6. Use specific captions, not generic labels.

Good caption:

```markdown
![圖八-1：檢查壓力調整器墊片](assets/08-guidance/08-guidance-19.png)
```

Avoid:

```markdown
![照片 19](assets/08-guidance/08-guidance-19.png)
```

## PDF Image Extraction

1. Extract only the requested pages unless the user asks for the full PDF.
2. Store page-derived images in `assets/<source-stem>/`.
3. Use page-aware filenames, such as `09-page8-image-01.jpg`.
4. Inspect extracted images before inserting them; skip repeated decorative assets.
5. Pair each image with the corresponding extracted text or step.
6. Keep a source comment near the inserted content:

```markdown
<!-- source: source-file.pdf 第8頁至第10頁 -->
```

## Public-Sector Traceability

- Keep source comments for laws, dates, agency names, penalties, checklist requirements, and operating steps.
- Do not infer legal thresholds or penalties from images alone.
- Use exact source wording for critical procedural steps when possible, then summarize the implication.
