---
name: cqa-titles-descriptions
description: Use when assessing CQA parameters P8-P11 and Q11 (titles and short descriptions). Checks abstract quality, character limits, title conventions, and assembly intro targeting.
---

# CQA P8-P11, Q11: Titles and Short Descriptions

## Parameters

| # | Parameter | Level |
|---|-----------|-------|
| P8 | Short descriptions are clear and describe why the user should read the content | Required |
| P9 | Short descriptions: 50-300 chars, `[role="_abstract"]` present | Required |
| P10 | Titles support short, long, and descriptive forms (DITA) | Important |
| P11 | Titles are brief, complete, and descriptive | Required |
| Q11 | Assembly intros target audience and persona | Important |

## Checks

<!-- TODO: Add detailed check procedures as this parameter is assessed -->

### P8: Abstract quality

Every `[role="_abstract"]` paragraph must be:
- **Action-oriented**: tell the user what they can DO
- **Not self-referential**: no "This section describes...", "Learn how to..."
- **Customer-centric**: written from the user's perspective
- **Technically accurate**: use correct product attributes

### P9: Character limits

- Minimum: 50 characters
- Maximum: 300 characters
- `[role="_abstract"]` annotation required on every module

### P10-P11: Title conventions

- **Procedure titles**: gerund phrases ("Installing...", "Configuring...")
- **Concept titles**: noun phrases ("Architecture overview", "Server components")
- **Reference titles**: noun phrases ("Supported platforms", "CheCluster fields")
- Sentence case (capitalize only first word and proper nouns)
- Maximum ~80 characters raw

### Q11: Assembly intro quality

Assembly abstracts and introductory paragraphs should:
- Set context for the reader
- Explain what they will accomplish
- Be specific to the audience (admin vs developer)

## Scoring

See [scoring-guide.md](../../references/scoring-guide.md).
