# PHES-ODM manuscripts

This repository holds manuscript sources for papers related to the Public Health Environmental Surveillance Open Data Model (PHES-ODM).

Each subfolder is a self-contained Quarto project for one manuscript.

## Manuscripts

| Folder | Title | Target | Status |
|---|---|---|---|
| [`phes-odm-v3/`](phes-odm-v3/) | Expanding the PHES-ODM: a comprehensive, open-source data model for the uncertain future of wastewater-based epidemiology | arXiv preprint | Draft |

## Quickstart

```bash
cd phes-odm-v3
quarto render phes-odm-v3-arxiv.qmd
```

See [`.claude/skills/manuscript/`](.claude/skills/manuscript/) for team skills (render, add ORCIDs, check formatting).

## Requirements

- Quarto ≥ 1.4 — https://quarto.org
- LaTeX (TinyTeX is fine): `quarto install tinytex`

## Adding a new manuscript

1. Create a new folder `my-manuscript/` at the repo root.
2. Add the `.qmd` source, `references.bib`, `images/`, and a `_quarto.yml`.
3. If using the arXiv format, copy `_extensions/mikemahoney218/` from `phes-odm-v3/`.
4. Add a row to the table above.

## Contributing

- Branches: feature branches per manuscript revision (e.g., `v3-reviewer-round-1`)
- Do not commit rendered `.pdf` or `.tex` — these are gitignored
- Canadian English spelling in prose
