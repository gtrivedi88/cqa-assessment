---
name: cqa-tables-images
description: Use when assessing CQA parameters Q19, Q21-Q22 (tables and images). Checks for excessive screenshots, table captions, and image alt text.
---

# CQA Q19, Q21-Q22: Tables and Images

## Parameters

| # | Parameter | Level |
|---|-----------|-------|
| Q19 | No excessive use of screen images | Important |
| Q21 | Tables have captions and are clearly labeled | Important |
| Q22 | Images have captions and meaningful alt text | Important |

## Checks

<!-- TODO: Add detailed check procedures as this parameter is assessed -->

### Q19: Screenshot usage

- Images should be architectural diagrams or essential UI screenshots
- No excessive step-by-step screenshots for simple UI actions
- Text instructions preferred over screenshots for simple navigation

### Q21: Table captions

Every table must have a `.Title` caption:
```asciidoc
.Supported platforms
[cols="1,1",options="header"]
|===
| Platform | Version
| OpenShift | 4.15+
|===
```

Search for tables without captions:
```
grep -n '^\|===' files | # then check preceding line for .Title
```

### Q22: Image alt text

Every `image::` must have meaningful alt text in brackets:
```asciidoc
image::architecture/overview.png[Dev Spaces architecture overview]
```

Empty brackets `[]` or generic alt text ("screenshot", "image") are violations.

## Scoring

See [scoring-guide.md](../../references/scoring-guide.md).
