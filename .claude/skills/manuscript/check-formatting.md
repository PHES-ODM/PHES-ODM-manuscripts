---
name: check-formatting
description: Scan a Quarto manuscript for common formatting issues introduced by Word→Markdown conversion. Use when asked to check the document for formatting problems, scan for spacing issues, find harvest artifacts, or audit the QMD before rendering.
---

# Check manuscript formatting

Scans a `.qmd` file for the common issues we see after harvesting from a Word source via markitdown/pandoc.

## What to check

Run each of these `Grep` patterns against the target QMD. Each pattern targets a real failure mode we have hit on the PHES-ODM manuscript.

### 1. Bold/italic markers missing surrounding spaces

The most common harvest artifact. Word inline runs collapse so `text **bold** text` becomes `text**bold**text`.

```
Pattern: [A-Za-z;:.,]\*\*[A-Za-z]
```

Examples we have fixed:
- `restrictions;**Semantic` → `restrictions; **Semantic`
- `them.**This table` → `them.** This table`
- `see**Figure5c **` → `see **Figure 5c**`

Same check for italics:
```
Pattern: [A-Za-z]\*[A-Za-z]
```
(may have false positives on legitimate words containing `*` — review each match)

### 2. Stray HTML comment artifacts

markitdown sometimes leaves placeholder comments where images or content blocks were stripped:
```
Pattern: <!-- IMAGE -->|<!-- TEXTBOX -->|<!-- TODO -->
```

### 3. Broken paragraphs

A paragraph fragmented across many short lines is usually a Word textbox or floating element that pandoc split. Look for runs of consecutive non-empty lines under ~50 characters that do not look like a list:
```
Pattern: ^.{1,50}$
```
Then visually inspect — legitimate short lines (headings, list items, captions) are fine.

### 4. docstyle `.figure` divs (will not render in arXiv PDF)

The arXiv format uses pure Pandoc figure syntax. Any `::: {.figure}` block must be converted:
```
Pattern: ::: \{\.figure
```
Convert to:
```markdown
![Caption.](images/imageN.png){#fig-N fig-align="center"}
```

### 5. Figure references that do not match figure IDs

List declared figure IDs:
```
Pattern: \{#fig-[a-zA-Z0-9-]+
```
List references in text:
```
Pattern: @fig-[a-zA-Z0-9-]+
```
Cross-check that every `@fig-X` has a corresponding `{#fig-X}`.

### 6. Tiny source images that will be upscaled

After rendering once, check `images/` file sizes:
```bash
ls -lhS images/ | tail
```
Anything under ~30 KB likely needs `width="45%"` (or smaller) on its figure to avoid pixelation, or a higher-resolution source.

### 7. Smart quotes / nbsp / em-dash inconsistencies

```
Pattern: [""'']|\xc2\xa0|—
```
Decide on a convention (we use straight quotes and `--` for en-dashes in source) and normalize.

## Reporting

When you finish the scan, report findings as a short list grouped by issue type, with file:line references. Do not auto-fix — show the user what you found and let them confirm before editing. Spacing issues in particular often have legitimate exceptions (e.g., inline code, URLs).

## Quick all-in-one scan

```bash
cd phes-odm-v3
# spacing around bold
grep -nE '[A-Za-z;:.,]\*\*[A-Za-z]|[A-Za-z]\*\*[;:.,]' phes-odm-v3-arxiv.qmd
# harvest artifacts
grep -nE '<!-- (IMAGE|TEXTBOX|TODO) -->' phes-odm-v3-arxiv.qmd
# docstyle divs that arXiv ignores
grep -n '::: {.figure' phes-odm-v3-arxiv.qmd
```
(Use the `Grep` tool rather than bash `grep` when running inside Claude Code.)
