---
name: paper-intro-writer
description: Draft, revise, cite, and format Chinese or English academic paper introductions, literature-review openings, and research-status sections. Use when the user is writing a论文/academic paper and asks for 引言, 第一章, 研究背景, 研究现状, 文献综述, reference ordering, citation numbering, citation cleanup, rewriting an introduction based on later chapters/results, or generating/repairing a Word/.docx introduction deliverable including Chinese乱码 fixes.
---

# Paper Intro Writer

## Purpose

Write, revise, cite, and export academic paper introductions from manuscript context. The core job is to identify the research gap, organize the introduction into logical layers, align the contribution paragraph with the paper's actual methods/results, order references by first appearance, and produce validated Word output when requested.

This skill is especially useful for Chinese academic writing where the user wants: 分层研究现状, 精简正文, 根据第三章重写第一章, 统一参考文献编号, or a `.docx` draft.

## Core Workflow

1. **Read the manuscript context first**
   - Extract the paper title, abstract, current introduction, methods, results, discussion, conclusion, and any user-mentioned chapter.
   - When the user says "根据第三章", "根据后文", "根据结果", or similar, read that later section before rewriting the introduction.
   - Preserve unfinished placeholders such as `[待补充]`; do not fabricate experimental results, best models, metric values, sample counts, or thresholds.

2. **Clarify the thesis and layers**
   - Identify the central problem, target method, evaluation object, and contribution boundary.
   - Turn the introduction into 3-5 logical layers, usually:
     - Engineering/scientific background and why the problem matters.
     - Limitations of traditional or existing methods.
     - Current technical route and model/method research status.
     - Research gap that the paper actually addresses.
     - This paper's work, written in the same order as the methods/results chapters.
   - Skip layers the user explicitly says not to write.

3. **Align with later chapters**
   - Pull forward only themes supported by later manuscript content.
   - For model comparison or experimental papers, align the introduction with:
     - segmentation or prediction metrics such as Precision, Recall, F1, IoU, Dice, accuracy, MAE, RMSE;
     - typical-sample or case visual analysis;
     - parameter count, model size, inference time, or other efficiency measures;
     - task-specific quantitative metrics such as area-ratio error;
     - downstream decision tasks such as grade classification, stability, or engineering applicability.
   - Avoid making the introduction a results summary. Use later chapters to define motivation and contribution, not to announce unsupported conclusions.

4. **Build the literature map**
   - Prefer reliable sources: review papers, high-impact journal papers, DOI-backed articles, standards/reports, and the user's own prior work when relevant.
   - If the user asks for latest/current references or precise bibliographic metadata, browse or use the requested research tool.
   - Classify each reference by the claim it supports; avoid using a paper merely because it is related.
   - If the user's own paper is used, describe it as prior work or previous research basis, not as an anonymous external finding.

5. **Draft compact research-status prose**
   - Write in a problem-driven order: importance -> limitations -> existing methods -> unresolved gap -> this paper.
   - Avoid long paper-by-paper summaries. Group papers by what they establish.
   - Keep each research-status layer to 1-3 focused paragraphs unless the user requests a long review.
   - End each layer with a sentence that naturally leads to the next layer or to the research gap.

6. **Apply citation numbering**
   - Use one continuous reference list for the whole introduction unless the user explicitly asks for per-section lists.
   - Number references by first appearance in the body, starting from `[1]`.
   - Do not introduce a later topic's specialist paper in an early broad citation range if that would break first-appearance order.
   - Prefer precise grouped citations only when the papers first appear together and support the same sentence.
   - After drafting, verify the first-appearance order programmatically or manually.

7. **Generate Word output when requested**
   - Use the `docx` skill when producing or editing `.docx` files.
   - Preserve the user's original document unless they ask to overwrite it; create a clearly named new version otherwise.
   - For Chinese text, avoid passing large Chinese scripts through PowerShell pipes because console encoding can turn Chinese into `?`.
   - Prefer writing a UTF-8 `.py` script file, then executing it.
   - Set Chinese fonts explicitly, for example 宋体 for body text and 黑体 for headings.
   - Validate the generated document by extracting `word/document.xml` and checking for readable Chinese text and exactly one final `参考文献` section when using unified references.

## Citation Checklist

Before finalizing, check:

- Every in-text citation has a matching reference entry.
- Reference entries are ordered by first appearance, not by topic, date, or the order in which they were found.
- There is only one final reference list unless the user asked for section-level references.
- Ranges such as `[6-8]` do not prematurely introduce references whose first real discussion appears later.
- Repeated citations reuse the same original number.
- Bibliographic metadata is internally consistent: author names, journal/conference/report, year, volume, pages/article number, DOI/arXiv where available.

## DOCX Validation Checklist

Before telling the user a Word file is ready:

- Open the `.docx` as a ZIP archive and read `word/document.xml`.
- Confirm representative Chinese phrases from the introduction are present.
- Confirm `参考文献` and all expected citation numbers are present when references were requested.
- Confirm Chinese text was not replaced with question marks; count or inspect `?` if encoding corruption is suspected.
- If validation fails, fix the generation path before reporting completion.

Recommended validation pattern:

```python
from zipfile import ZipFile
import re
from xml.sax.saxutils import unescape

with ZipFile(path) as z:
    xml = z.read("word/document.xml").decode("utf-8")
plain = "".join(unescape(t) for t in re.findall(r"<w:t[^>]*>(.*?)</w:t>", xml))
```

## Writing Heuristics

- Use "已有研究表明..." for grouped findings, but follow with the specific mechanism, capability, or limitation.
- Use "然而..." or "但..." to pivot from advantages to limitations.
- Use "总体而言..." to summarize a research layer.
- Use "仍缺乏系统研究" only after naming what is missing: model applicability, unified dataset comparison, parameter matching, quantitative-evaluation error propagation, grade-stability analysis, mechanism coupling, etc.
- Keep claims modest when evidence is limited: "有助于", "能够在一定程度上", "可能改善", "仍需进一步研究".
- When later chapters contain placeholders, write "本文拟/本文从..." or neutral methodological statements rather than definitive performance claims.

## Preferred Output Shape

For a full introduction:

1. `引言` or `1 引言` title.
2. 4-6 compact paragraphs unless the target journal requires subsections.
3. Contribution paragraph ordered to match the methods/results chapters.
4. One final `参考文献` section when references are requested.

For a partial section:

- Provide the drafted paragraph(s).
- Provide a small reference list or citation notes if the user is still gathering sources.
- If requested, produce a standalone Word document for that section.

For a revised manuscript file:

- Update only the requested section unless the user asks for broader editing.
- Preserve existing placeholders, headings, tables, figures, and unrelated sections.
- Read back the edited section to verify the chapter boundary was not damaged.

## Example Patterns

When the user says:

> 根据第三章的内容重新写第一章引言

Do:

- Read Chapter 3 and identify its analytical threads.
- Rewrite the introduction so the gap and contribution paragraph anticipate those threads.
- Do not add final result claims if Chapter 3 still has `[待补充]` values.

When the user says:

> 帮我加上参考文献

Do:

- Add citations only where they support specific claims.
- Number by first appearance from `[1]`.
- Add one final reference list.
- Verify citation numbers and bibliography entries match.

When the user says:

> 帮我更新一下 Word / 打开是乱码

Do:

- Use or create a UTF-8 generation script rather than piping Chinese through PowerShell.
- Generate or update the `.docx`.
- Validate `word/document.xml` for readable Chinese and no encoding corruption before responding.
