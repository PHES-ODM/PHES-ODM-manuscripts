---
name: add-orcid
description: Add or update ORCID identifiers for authors in a Quarto manuscript's YAML front matter. Use when asked to add an ORCID, add an author ORCID ID, fill in missing ORCIDs, or update author identifiers in the QMD header.
---

# Add ORCID IDs to a manuscript

ORCIDs appear as superscript badges next to author names in the rendered arXiv PDF (via the `\orcidlink` LaTeX command bundled with the arXiv extension).

## YAML syntax

In the `author:` block of the QMD:

```yaml
author:
  - name: Jane Doe
    orcid: 0000-0002-1234-5678
    affiliations:
      - ref: uni
    email: jane@example.org
```

The `orcid:` field is optional per author. When present, a badge renders beside the name. When absent, nothing renders (no placeholder).

## Format rules

- 16 digits in groups of 4 separated by hyphens: `XXXX-XXXX-XXXX-XXXX`
- The final character can be `0–9` or `X` (it is a checksum digit)
- Do **not** include the `https://orcid.org/` prefix — the extension adds the link

## Verifying an ORCID

**Do not accept ORCIDs from AI search summaries.** We have been burned by hallucinated values (e.g., one author's ORCID returned for another).

Confirm by visiting `https://orcid.org/XXXX-XXXX-XXXX-XXXX` and matching:
- Name
- Affiliation history
- Publication list overlap with the author's known work

When asking a user for an ORCID, ask them to paste the URL or the 16-digit identifier directly.

## PHES-ODM v3 — current status

Confirmed (already in YAML):
- Peter Vanrolleghem
- François Therrien
- Douglas Manuel

Missing (currently `# orcid: TODO` in `phes-odm-v3-arxiv.qmd`):
- Mathew Thomson
- Nikho Hizon
- Janet Lin
- Martin Wellman
- Eugen-Sorin Sion
- Carol Bennett

When a value is confirmed, replace the comment line with:
```yaml
    orcid: XXXX-XXXX-XXXX-XXXX
```

## After editing

Re-render to verify the badge appears:
```bash
cd phes-odm-v3
quarto render phes-odm-v3-arxiv.qmd
```
The badge is a small superscript link next to the author name in the author plate.
