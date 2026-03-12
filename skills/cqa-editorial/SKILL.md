---
name: cqa-editorial
description: Use when assessing CQA parameters P13-P14, Q1-Q5, Q18, Q20 (editorial quality). Checks grammar, content types, readability, scannability, fluff, style guide compliance, and tone.
---

# CQA P13-P14, Q1-Q5, Q18, Q20: Editorial Quality

## Parameters

| # | Parameter | Level |
|---|-----------|-------|
| P13 | Grammatically correct American English | Required |
| P14 | Correct content type matches actual content | Required |
| Q1 | Scannable: sentences <= 22 words avg, paragraphs 2-3 sentences | Required |
| Q2 | Clearly written and understandable | Important |
| Q3 | Simple words (no "utilize", "leverage", "in order to") | Important |
| Q4 | Readability score (11-12th grade level) | Important |
| Q5 | No fluff ("This section describes...", "as mentioned") | Important |
| Q18 | Content follows Red Hat style guide | Required |
| Q20 | Appropriate conversational tone (2nd person, professional) | Important |

## Checks

<!-- TODO: Add detailed check procedures as this parameter is assessed -->

### Automated checks

Run `cqa-audit.py` if available:
```bash
python3 scripts/cqa-audit.py
```

This checks: sentence length, complex words, self-referential fluff, readability, and tone.

### P13: Grammar

- American English spelling
- No grammatical errors in titles or body text
- Correct article usage ("a" vs "an")

### P14: Content type

- `:_mod-docs-content-type:` matches actual content
- Filename prefix matches content type
- Procedures have `.Procedure` with ordered list
- Concepts have explanatory content, no `.Procedure`

### Q1: Scannability

- Average sentence length <= 22 words
- Flag sentences > 30 words for splitting
- Paragraphs: 2-3 sentences max

### Q3: Simple words

Flag and replace:
- "utilize" -> "use"
- "leverage" -> "use"
- "in order to" -> "to"
- "prior to" -> "before"
- "subsequent to" -> "after"

### Q5: Fluff

Flag and rewrite:
- "This section describes..."
- "This section provides..."
- "as mentioned above/below"
- "Learn how to..."
- "In this chapter..."

## Scoring

See [scoring-guide.md](../../references/scoring-guide.md).
