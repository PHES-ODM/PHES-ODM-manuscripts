---
name: render
description: Render a Quarto manuscript to arXiv PDF. Use when asked to render the manuscript, build the PDF, generate the preprint, or compile the document. Also handles common Quarto/LaTeX errors.
---

# Render a manuscript to arXiv PDF

## Quick start

```bash
cd phes-odm-v3
quarto render phes-odm-v3-arxiv.qmd
```

Output: `phes-odm-v3-arxiv.pdf` (gitignored — do not commit)

## Prerequisites

- **Quarto** ≥ 1.4 — check with `quarto --version`
- **LaTeX** — TinyTeX recommended: `quarto install tinytex`
- **Pandoc** — bundled with Quarto, no separate install needed

## What's in a manuscript folder

Each manuscript subfolder is a self-contained Quarto project:

```
phes-odm-v3/
├── phes-odm-v3-arxiv.qmd         # Source
├── references.bib                 # Bibliography
├── images/                        # Figures
├── _extensions/mikemahoney218/    # arXiv Quarto extension
└── _quarto.yml                    # Project config
```

The arXiv extension bundles its own `arxiv.sty` and `orcidlink.sty` LaTeX files — no system-level LaTeX package install is needed beyond a working TeX distribution.

## Common errors and fixes

### `! LaTeX Error: File 'arxiv.sty' not found`
The extension folder is missing or incomplete. Verify:
```bash
ls _extensions/mikemahoney218/arxiv/
```
Should show: `_extension.yml`, `arxiv.sty`, `orcidlink.sty`, `partials/`, `shortcodes.lua`

### `Error: Unable to find pandoc citeproc`
Update Quarto: `quarto update` or reinstall from https://quarto.org

### Figure not rendering
Ensure standard Pandoc syntax:
```markdown
![Caption text.](images/imageN.png){#fig-N fig-align="center"}
```
The arXiv format does NOT support docstyle `.figure` divs — those only render in Word output.

### Figure renders huge / pixelated
Source image is small and getting upscaled. Add a width constraint:
```markdown
![Caption.](images/small.png){#fig-X width="45%"}
```

### Bold or italic markers showing as literal `**` or `*`
Missing space before/after the marker. Common patterns:
- `text;**bold**` → `text; **bold**`
- `**bold**word` → `**bold** word`

Use the `check-formatting` skill to scan for these.

## Iterative editing workflow

For frequent edits with quick preview:
```bash
quarto preview phes-odm-v3-arxiv.qmd
```
This watches the file and re-renders on save. Opens a browser preview of the HTML version (faster than PDF for content review).

For final PDF check:
```bash
quarto render phes-odm-v3-arxiv.qmd --to arxiv-pdf
```

## After rendering

1. Open the PDF and visually check:
   - Author plate (names, affiliations, ORCID badges, correspondence)
   - Figure numbering matches text references (Figure 5a, 6b, etc.)
   - Tables render correctly
   - References are linked
2. If submitting to arXiv: upload the QMD-generated `.tex` plus `images/` and `references.bib`. arXiv compiles LaTeX server-side.
