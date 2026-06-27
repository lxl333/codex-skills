---
name: xiaolanben-douyin-lesson
description: Use when a Xiaolanben math lesson needs source-faithful examples, per-example comic assets, a comic-based Douyin/TikTok cover, printable handout, vertical MP4, per-page mottos, or video QA from local PDFs/images.
---

# Xiaolanben Douyin Lesson

## Overview

Create a complete Xiaolanben example-lesson package from local source pages: UTF-8 HTML, optional PDF handout, 1080x1920 no-audio MP4 by default, cover PNG, extracted QA frames, and a concise report. Keep the math faithful first; make the video platform-ready second.

**Use together with:** `xiaolanben-study-guide` for lesson writing and `making-douyin-html-videos` for MP4 export.

## Workflow

1. Identify the chapter, section, and example numbers.
2. Locate source pages in local PDFs/images. Prefer rendered page inspection over OCR when formulas, Chinese text, or page continuations matter.
3. Restate each problem faithfully, then expand the proof into a guided lesson: problem, translation, core idea, step proof, warning, practice, takeaway.
4. Add a short motto/quote at the top of every lesson page when requested; use stable public-domain Chinese quotations or very short paraphrased mottos.
5. Create or reuse one distinct local comic/raster asset for each requested example unless the user explicitly says no comics.
6. Build one self-contained UTF-8 HTML file. Use ASCII filenames for export files even when the page text is Chinese.
7. Make the first viewport a real cover, not a generic title card. When comics exist, use the title-first comic cover pattern: title/hook at the top, then the lesson's actual per-example comics as the main visual body.
8. Verify the HTML in a browser at `1080x1920`: title, scroll height, scroll width, client width, horizontal overflow, first screenshot, loaded image sizes, and visible key answers.
9. Export PDF when useful, then render its first page with PyMuPDF or another PDF renderer.
10. Export a no-audio MP4 first. Add music only if the user explicitly asks and rights are clear.
11. Verify video metadata and inspect frames near start, middle, late section, and end. Regenerate after any math, encoding, cover, image, overflow, or overlay failure.

## Source And Math Checks

- Search workspace files with `rg`; use PyMuPDF or existing rendered pages for PDF inspection.
- Treat extracted text as suspect when it contains mojibake, missing subscripts, unnatural Chinese, or clipped formulas.
- Preserve original variables, conditions, conclusions, inequalities, set relations, and constants.
- For examples spanning pages, inspect the continuation pages before finalizing the answer.
- For maximum-value problems, prove both directions: an upper-bound counterexample and a lower-bound guarantee.
- For shared-element or pairwise-intersection arguments, explicitly justify why a proposed common element is forced; do not assume pairwise intersections automatically share the same element.
- For sliding-window or discrete-continuity arguments, state why adjacent windows change by at most the claimed amount and why the target value is crossed.

## HTML Contract

- Single self-contained `.html`; no network assets.
- UTF-8 meta tag and Chinese-friendly fonts: `Microsoft YaHei`, `PingFang SC`, `Noto Sans CJK SC`, fallback sans-serif.
- Phone-safe typography: body `18-19px`, formulas `20-22px`, generous line-height.
- Use color for meaning: blue setup, violet method, green result, orange warning, red contradiction.
- Put a `.motto` or equivalent quote strip at the top of every page/section when the user asks for famous quotations.
- Make each example visually distinct with its own comic/raster asset plus any needed CSS diagrams, tables, number lines, interval strips, or set pictures.
- Keep cards at `8px` radius or less. Do not put cards inside cards.
- Keep the first viewport readable and let it hint at the next section.
- Add print CSS if exporting a PDF.

## Comic Asset Contract

When the user asks for comics, illustrations, or Douyin-ready lesson visuals:

- Create or reuse a separate local raster comic for every requested example. For example, examples 7-8 need at least two files such as `sec2_ex7_comic.png` and `sec2_ex8_comic.png`.
- A CSS diagram, Venn picture, table, or number line can support the proof, but it does not satisfy the per-example comic requirement by itself.
- Each comic must encode the specific method or conclusion of that example, not a generic student-with-book scene.
- Keep in-image text minimal and robust: prefer labels such as `a`, `45+45-89=1`, `34 red`, `17 blue`, or `k=17`; put full Chinese explanation in HTML text.
- Copy generated images from `$CODEX_HOME/generated_images/...` into the workspace before referencing them. Never leave a project-referenced image only in the generator cache.
- Inspect every comic before export for wrong text, extra random text, low readability, repeated composition, or mismatch with the math.
- In HTML, reference every comic with `img { max-width: 100%; height: auto; display: block; }` or an equivalent class, and verify `naturalWidth` and `naturalHeight` are positive.

## Cover Differentiation

When the user mentions repeated/batch publishing, do all of the following:

- Compare the current cover with the previous batch in the workspace.
- Change at least three signals: background structure, dominant palette, title composition, visual diagram, hook wording, badge shape, or formula callout.
- Put the requested example numbers and a concrete math hook in the first viewport.
- If comics exist, compose the cover from the actual example comics instead of using a pure CSS title card.
- For multi-example lessons, show at least one visual cue from each example on the cover, such as split panels, stacked panels, or a collage.
- Avoid a reusable generic "Xiaolanben lesson" cover. The cover should reveal this lesson's actual method or result.
- Save a cover PNG and inspect it for Chinese text rendering, crop, overlap, and similarity to previous uploads. If it looks like the last upload series or contains `????`, redesign before exporting the final MP4.
- If composing a cover with Pillow or another local renderer, read Chinese title strings from a UTF-8 file or use Unicode-safe strings; do not pass Chinese cover text through a PowerShell heredoc.

## Comic Cover Layout Pattern

When comics exist, prefer this reusable cover structure:

- Put a strong title block first: chapter/section or topic name, requested example numbers, and one concrete math hook. The top block should be text-forward and readable before any image detail.
- Place the per-example comics after the title block as the main visual body. For two examples, stack two wide comics vertically or use a balanced split layout; for more examples, use a tidy collage that still shows one visual cue from each example.
- Treat the cover as an entrance to the lesson, not as a replacement for per-example comics. Each requested example still needs its own separate comic asset in the body.
- Add a short bottom takeaway only if it helps, such as a one-line method summary. Keep it outside the comic images so it can be edited safely.
- Generate composite covers with a UTF-8 Python helper file when Chinese text is involved. Avoid PowerShell heredocs or shell-passed Chinese strings for cover text because they can turn into `????`.
- Inspect the cover before export for: title readability, Chinese rendering, comic crop, spacing between title and comics, and whether every requested example is represented.

## Phone-Safe CSS Baseline

```css
html, body {
  margin: 0;
  max-width: 100%;
  overflow-x: hidden;
}

main {
  width: 100%;
  max-width: 940px;
  margin: 0 auto;
  padding: 44px 54px 76px;
}

.formula {
  max-width: 100%;
  overflow-wrap: anywhere;
  word-break: break-word;
}

img {
  max-width: 100%;
  height: auto;
  display: block;
}
```

Treat either `documentElement.scrollWidth > clientWidth` or `body.scrollWidth > clientWidth` as a blocker.

## Video Export Rules

Use the bundled exporter:

```powershell
python "C:\Users\Administrator\.codex\skills\making-douyin-html-videos\scripts\html_to_douyin_video.py" `
  --html "D:\path\lesson.html" `
  --out "D:\path\lesson_no_audio.mp4" `
  --cover "D:\path\lesson_cover.png" `
  --duration 60 `
  --fps 10 `
  --port 0
```

Guidelines:

- Default to no audio. If no music was requested, confirm the final MP4 has no audio stream.
- Use `60-90s` for two dense examples; adjust if scrolling misses the conclusion.
- Use ASCII output filenames to avoid Python, Playwright, ffmpeg, or PowerShell path encoding issues.
- Do not trust Chinese overlay arguments passed through PowerShell heredocs. If Chinese top labels, hook lines, or captions render as `????`, regenerate using a UTF-8 `.py` helper file, Unicode escapes, or disable script overlays and let the HTML carry the Chinese text.
- If bottom captions cover formulas or proof steps, remove captions and rely on the lesson page text.
- If the script's default top badge is stale or garbled, pass `--no-top-badge` or provide a verified UTF-8-safe `--top-label`.

## Optional Music

Add music only when the user asks for it.

- If the user asks for current/trending music, browse and cite the concrete chart/date.
- Do not download or embed copyrighted chart audio unless the user supplied the file and rights are clear.
- Prefer a quiet original style-match bed for learning videos.
- Keep music low; captions, formulas, and proof flow remain primary.

Mux only after the no-audio video passes QA:

```powershell
$ffmpeg = python -c "import imageio_ffmpeg; print(imageio_ffmpeg.get_ffmpeg_exe())"
& $ffmpeg -y -i "input_no_audio.mp4" -i "music.wav" `
  -map 0:v:0 -map 1:a:0 -c:v copy -c:a aac -b:a 128k `
  -shortest -movflags +faststart "output_with_music.mp4"
```

## Verification Checklist

Before reporting completion:

- HTML exists, is nonzero, UTF-8, and contains every requested example label and key answer.
- Every requested page has a visible motto/quote when requested.
- Every requested example has its own loaded comic/raster asset unless the user explicitly waived comics.
- The cover is generated from the actual lesson visuals when comics exist, and the first frame of the MP4 shows that cover without unwanted script overlays.
- Browser render at `1080x1920` reports `overflowX: false`, `scrollWidth == clientWidth`, readable first viewport, and no cropped text.
- Browser image audit reports every expected image loaded with nonzero `naturalWidth` and `naturalHeight`.
- PDF exists and first page renders if a PDF was requested or useful.
- MP4 is `1080x1920`, H.264, `yuv420p`, no rotation, and about `30-120s`.
- Audio status matches the request: no audio stream by default; AAC audio only if music was requested and added.
- Frames near start, middle, late section, and end show correct comic cover, per-example comics, no mojibake, no stale example numbers, no repeated old cover style, no obscured formulas, and the final conclusion is reached.
- If `ffprobe` is unavailable, use `ffmpeg -hide_banner -i <video>` to inspect streams.

## Reporting

Return paths for the HTML, PDF if created, per-example comic assets, cover PNG, final MP4, and QA frames when useful. Mention the core verification evidence: `overflowX: false`, image load audit, PDF page count/render, video size/duration/codec, audio-stream status, and inspected frame times.
