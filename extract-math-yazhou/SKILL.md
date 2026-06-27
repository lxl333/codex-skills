---
name: extract-math-yazhou
description: Extract high-school math challenge problems from exam papers and answer PDFs/images into polished Xiaolanben-style HTML study pages. Use when Codex is given math exam PDFs/images and needs pressure-problem extraction, complete question crops, regenerated answers, full worked explanations, MathJax formulas, crop-quality review sheets, or one-HTML-per-paper专题 pages.
---

# Extract Math Yazhou

## Core Contract

- Default pressure problems are `7, 8, 11, 14, 18, 19`, unless the user specifies another set.
- Produce one standalone HTML per paper when multiple papers are provided, unless the user explicitly asks for a combined page.
- Use the user-approved reference page or template style when provided. Preserve its reading order: question image, answer strip, red key ideas, complete worked explanation, common-error reminder.
- Treat the artifact as a student learning page, not an answer dump.

## Required Workflow

1. Inventory the workspace with `rg --files`; identify papers, answer keys, existing outputs, and any reference template.
2. Render PDF pages to images before cropping. Use text extraction only as an aid; scanned formulas and diagrams must be checked visually.
3. Before changing any crop coordinate, first inspect the original PDF page with a 5% or 10% grid overlay, then edit coordinates, regenerate the HTML, and make a final crop review sheet to confirm no previous/next problem is included.
4. Crop every requested question from the paper, not from the answer key.
5. Write or reconstruct answers and explanations in a consistent style. Do not paste official answer screenshots into the answer or explanation sections unless the user explicitly asks for them.
6. Regenerate the HTML after every crop or explanation change.
7. Validate the output: all requested cards exist, all image paths resolve, no unwanted answer screenshots are referenced, and no generic placeholder explanation remains.

## Crop Quality Rules

A valid crop must:

- Include the problem number, full stem, all subquestions, all choices/blanks, and every diagram/table belonging to the problem.
- Exclude unrelated previous/next problems, section headings, page footers, answer-key content, and large blank blocks.
- Prefer a little clean margin over a tight crop that clips punctuation, formula tails, or right-edge text.
- Leave extra bottom margin below the last option or final blank, especially when fractions, radicals, subscripts, descenders, or underlines appear near the bottom. Do not accept crops where the D option or last line only “mostly” shows.
- Match the quality of the best long-answer crops: clean top boundary, clean bottom boundary, no neighboring problem fragments.
- If a problem crosses pages, or its diagram sits in the previous/adjacent area such as the upper-right of the preceding block, use multiple precise crop blocks and stitch them vertically. Do not use one large rectangle that pulls in neighboring questions or section headings.

When a crop is bad:

- First inspect the original PDF page with a 5% or 10% grid overlay, then change coordinates, regenerate the HTML, and make a crop review sheet proving no upper/lower neighboring problem is included.
- Do not guess repeatedly from thumbnails. Render the full page with 5% or 10% horizontal grid lines, inspect it, then set fractional crop coordinates.
- For cross-page or separated figure/stem layouts, crop the stem, choices, and diagram as separate blocks in their natural reading order, then stitch with clean white spacing. Recheck the stitched image at full size before accepting it.
- Make a contact sheet of the final requested crops and inspect it visually before final response.
- Inspect the last option/final line at full size; if the bottom of a formula, denominator, radical, or underline is close to the crop edge, widen the bottom boundary and regenerate.
- If only one paper is being corrected, adjust only that paper’s crop coordinates and regenerate that paper’s HTML.

## Explanation Rules

- Put the answer immediately after the question image.
- The answer strip must contain a real answer or a precise conclusion, not “见解析” or “see solution”.
- The red key-idea block must name the actual turning point of that problem, not a generic topic label.
- The worked explanation must be problem-specific. Do not leave generic filler such as “先识别题型核心”, “从题面截图中圈出条件”, or “按答案反推”.
- For objective questions, include enough process for a student to learn why the option is right. If the official key gives only the final option, derive a concise learning explanation from the problem statement.
- For long-answer questions, split by subquestion and follow the official answer’s sequence when recoverable. Reconstruct with clean MathJax instead of screenshots.
- Use yellow highlighting for decisive steps in the worked explanation, such as an equivalent transformation, monotonicity conclusion, key recurrence, sample-space count, final range, or geometric model switch.
- The common-error reminder must be problem-specific and name the actual trap in that problem. Never use production/meta reminders such as “本页只保留题目截图”, “按模板重写”, “逐行对照题面”, or other generic study advice.
- Render formulas with MathJax/LaTeX. Avoid rough ASCII like `3/7`, `C(9,4)`, `X~B(3,1/3)`, `e^(a-4)`, or `f(n+1)/f(n)` when mathematical notation is appropriate.
- In HTML text, never leave raw comparison symbols inside formulas where they can be parsed as tags, such as `\(x_1<x_2\)`. Write `\lt`, `\gt`, `\le`, `\ge`, or safe HTML entities so formulas render correctly.

## HTML Requirements

- Build the study page directly; do not make a marketing page.
- Keep the Xiaolanben-style layout: warm paper background, restrained blue/gold accents, red key-idea block, readable Chinese fonts, bordered full-width scans.
- Cards should follow this exact order:
  1. 题目截图
  2. 答案
  3. 题眼 / 关键步骤
  4. 解析过程
  5. 易错提醒
- Use relative image paths that open locally from the HTML.
- Include source file names in the footer.

## Validation Checklist

Before final response, verify:

- One HTML per requested paper exists.
- Requested question numbers are all included.
- Every card has a resolved question image.
- No objective-question answer-key screenshots are shown unless explicitly requested.
- No image `src` contains answer-key crops when the user asked to regenerate answers.
- No answer strip says “见解析”.
- No worked explanation contains generic placeholder language.
- Every common-error reminder states a concrete mathematical pitfall for that exact problem.
- Formula-heavy answers render through MathJax syntax.
- Formula-heavy cards have no mojibake or browser-parsed comparison signs; spot-check the rendered HTML around answers and highlighted solution steps.
- A final crop review contact sheet has been visually inspected, especially for adjusted questions.
