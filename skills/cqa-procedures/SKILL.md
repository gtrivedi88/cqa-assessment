---
name: cqa-procedures
description: Use when assessing CQA parameters P12, Q12-Q16 (procedure quality). Checks prerequisites, step counts, command examples, verification, and Additional resources.
---

# CQA P12, Q12-Q16: Procedures

## Parameters

| # | Parameter | Level |
|---|-----------|-------|
| P12 | Prerequisites: `.Prerequisites` label, unordered list, max 10 items | Required |
| Q12 | Procedures have <= 10 top-level steps | Important |
| Q13 | Procedures include command examples | Important |
| Q14 | Optional/conditional steps use correct formatting | Important |
| Q15 | Procedures include `.Verification` section | Important |
| Q16 | Procedures include `Additional resources` section | Important |

## Checks

<!-- TODO: Add detailed check procedures as this parameter is assessed -->

### P12: Prerequisites

- Must use `.Prerequisites` block title (not bold pseudo-heading)
- Must use unordered list (`*`) not ordered list
- Maximum 10 items
- Use declarative "You have..." phrasing, not "You must have..."

### Q12: Step count

- Maximum 10 top-level steps in `.Procedure`
- Steps use ordered list (`. `)
- Procedures with >10 steps should be split or restructured

### Q13: Command examples

- Procedures involving CLI should include `[source,bash]` blocks
- Use correct source language (`bash`, `yaml`, `json`, `terminal`, `sql`)
- Include `subs="+quotes"` with `__<placeholder>__` for replaceable text

### Q14: Conditional steps

- "Optionally" prefix for optional steps
- Proper AsciiDoc formatting for alternative paths

### Q15: Verification

- `.Verification` section after `.Procedure` (not as last procedure step)
- Uses unordered list

### Q16: Additional resources

- `[role="_additional-resources"]` `.Additional resources` at end of file
- Contains relevant xrefs and links

## Scoring

See [scoring-guide.md](../../references/scoring-guide.md).
