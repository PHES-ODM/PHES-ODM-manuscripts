# PHES-ODM manuscripts — project context

This repo holds Quarto sources for PHES-ODM manuscripts. Each subfolder is one paper.

## Structure

```
manuscripts/
├── README.md
├── .claude/
│   ├── CLAUDE.md                      # this file
│   └── skills/manuscript/             # team skills
│       ├── render.md                  # render QMD → arXiv PDF
│       ├── add-orcid.md               # add ORCID IDs to YAML
│       └── check-formatting.md        # scan QMD for harvest artifacts
└── phes-odm-v3/                       # first manuscript
    ├── phes-odm-v3-arxiv.qmd
    ├── references.bib
    ├── images/
    ├── _extensions/mikemahoney218/    # bundled arXiv Quarto extension
    └── _quarto.yml
```

## Render

From a manuscript folder:
```bash
quarto render phes-odm-v3-arxiv.qmd
```
Output PDF is gitignored — do not commit rendered artifacts.

## Adding a new manuscript

1. Create a subfolder at the repo root.
2. Drop in your `.qmd`, `references.bib`, `images/`, and `_quarto.yml`.
3. For arXiv PDF output, copy `_extensions/mikemahoney218/` from `phes-odm-v3/`.
4. Update the manuscript table in the root `README.md`.

## Style

- **Canadian English** in prose (colour, behaviour, centre, -ize endings).
- **Figures** use standard Pandoc syntax only:
  ```markdown
  ![Caption.](images/imageN.png){#fig-N fig-align="center"}
  ```
  Do **not** use `::: {.figure}` divs — those are docstyle-only and do not render in arXiv PDF.
- Keep figure reference IDs (`@fig-5`) aligned with declared IDs (`{#fig-5}`).

## Dependencies

- **Quarto** ≥ 1.4
- **LaTeX** — `quarto install tinytex` is sufficient
- The `mikemahoney218/quarto-arxiv` extension is bundled under `_extensions/` per manuscript — no separate install needed.
- docstyle is **not** required (arXiv format uses its own template).

## Known issues — phes-odm-v3

- Figure 7 (Rosetta Stone, `image15.png`) source is low-resolution (~10 KB). Currently constrained with `width="45%"` as a workaround; needs a higher-res replacement.
- 6 authors are missing ORCIDs — flagged with `# orcid: TODO` in the QMD YAML. See the `add-orcid` skill.
