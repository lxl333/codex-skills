---
name: xiaolanben-html-lesson
description: Create source-faithful, beginner-friendly single-file HTML lessons from Xiaolanben math PDFs, scanned pages, images, chapters, sections, examples, or exercises. Use when the user wants only an HTML study handout/page and explicitly does not need video, MP4, cover images, PDF export, or external illustration assets.
---

# Xiaolanben HTML Lesson

## Overview

Turn local Xiaolanben source pages into one self-contained UTF-8 HTML learning page. Prioritize accurate math, clear teaching flow, phone-safe layout, and source-page verification. Do not generate videos, covers, PDF handouts, comic images, or other side artifacts unless the user explicitly adds them back.

## Workflow

1. Identify the requested chapter, section, example numbers, or exercises.
2. Locate source pages in local PDFs/images. Prefer rendered page inspection over OCR when formulas, Chinese text, or page continuations matter.
3. Restate each problem faithfully, but avoid copying long textbook passages beyond necessary problem statements.
4. Rewrite each proof as a guided lesson: problem, ordinary-language translation, core method, visual model, step proof, warning, mini practice, takeaway.
5. Build exactly one standalone `.html` file by default. Use an ASCII filename unless the workspace already has a stable Chinese naming pattern.
6. Use HTML/CSS diagrams, tables, number lines, set pictures, or relationship graphs instead of generated raster images unless the user asks for images.
7. Verify the HTML exists, is UTF-8, contains every requested example label and key result, and renders without horizontal overflow.
8. Report the HTML path and concise verification evidence. Mention any source uncertainty if a formula or page scan could not be fully confirmed.

## Source And Math Checks

- Search workspace files first; if a PDF has no usable text layer, render pages with PyMuPDF and inspect images.
- For examples spanning pages, inspect continuation pages before writing the final lesson.
- Preserve original variables, bounds, set relations, modular conditions, inequalities, and conclusions.
- Treat OCR and raw text extraction as suspect when formulas, subscripts, Chinese punctuation, or page headers look corrupted.
- For counting arguments, name the counted set or partition explicitly.
- For double counting, state both perspectives and why they count the same object.
- For contradiction arguments, separate the temporary assumption from the contradiction.

## HTML Contract

- Single self-contained `.html`; no network assets, CDN CSS, remote fonts, or script dependencies.
- Include `<meta charset="utf-8">` and a responsive viewport tag.
- Use Chinese-friendly fonts: `Microsoft YaHei`, `PingFang SC`, `Noto Sans CJK SC`, fallback sans-serif.
- Keep body text readable at 17-19px with generous line-height; formulas should be larger and wrap safely.
- Use color for meaning: blue setup, violet method, green result, orange warning, red contradiction.
- Prefer semantic sections and readable print CSS, even when no PDF export is requested.
- Keep cards at `8px` radius or less. Do not nest cards inside cards.
- Keep the first viewport useful: chapter/section, requested examples, and the actual method hook.
- Ensure formulas and long set-builder notation use wrapping styles such as `overflow-wrap: anywhere`.

## What Not To Do By Default

- Do not call video-export scripts.
- Do not create MP4 files, cover PNGs, QA video frames, or music/audio.
- Do not generate per-example comic/raster assets.
- Do not export PDF unless the user explicitly asks for it.
- Do not add hidden dependencies that make the HTML fail offline.

## Verification Checklist

Before reporting completion:

- Confirm the HTML file exists and is nonzero.
- Search the HTML for every requested example label, such as `例1` through `例5`.
- Confirm every key answer or proof conclusion appears in the HTML.
- Render or inspect the HTML with a browser or browser-like tool when available; treat horizontal overflow as a blocker.
- If browser automation is unavailable, at minimum inspect the CSS for `max-width: 100%`, `overflow-x: hidden`, and wrapping rules for formulas.
- State the source pages used, the output path, and whether video/PDF/image generation was intentionally skipped.
