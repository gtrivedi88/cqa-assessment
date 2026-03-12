---
name: cqa-links
description: Use when assessing CQA parameters P15-P17, Q24-Q25 (links and URLs). Checks for broken xrefs, missing includes, missing images, redirect integrity, and content interlinking.
---

# CQA P15-P17, Q24-Q25: Links and URLs

## Parameters

| # | Parameter | Level |
|---|-----------|-------|
| P15 | No broken links (xrefs, includes, images) | Required |
| P16 | Redirects work correctly | Required |
| P17 | Content interlinked within 3 clicks of domain home | Important |
| Q24 | Content includes links to relevant content journey | Important |
| Q25 | Pages interlinked within 3 clicks | Important |

## Checks

<!-- TODO: Add detailed check procedures as this parameter is assessed -->

### P15: Broken links

Run the reference validation script:
```bash
python3 scripts/validate-refs.py
```

This checks:
1. Every `xref:ID_{context}[]` has a matching `[id="ID_{context}"]`
2. Every `include::path[]` resolves to an existing file
3. Every `image::path[]` resolves to a file under `images/`
4. No duplicate `[id="..."]` declarations

### P16: Redirects

- No Antora module prefixes (`administration-guide:`) in links
- `{prod-ag-url}` and `{prod-ug-url}` resolve correctly
- Cross-guide links use `link:` not `xref:`

### P17/Q24/Q25: Interlinking

- Assembly navigation provides topic discovery
- Related topics cross-reference each other
- Concepts link to related procedures
- Procedures link back to parent concepts

## Scoring

See [scoring-guide.md](../../references/scoring-guide.md).
