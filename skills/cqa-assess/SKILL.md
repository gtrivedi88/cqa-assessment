---
name: cqa-assess
description: Use when starting or continuing a full CQA 2.1 content quality assessment. Guides through all three tabs (Pre-migration, Quality, Onboarding) parameter by parameter.
---

# CQA 2.1 Content Quality Assessment

## Overview

Run a complete CQA 2.1 assessment against a Red Hat modular documentation repository. The assessment covers three tabs with 54 total parameters across Pre-migration, Quality, and Onboarding.

See [scoring-guide.md](../../references/scoring-guide.md) for score definitions and [checklist.md](../../references/checklist.md) for the full parameter list.

## Workflow

1. **Pick a parameter** from the checklist (or let the user specify one)
2. **Invoke the matching skill** for that parameter group
3. **Run the check** — automated where possible, manual review where needed
4. **Fix issues** found during the check
5. **Verify the fix** — re-run the check to confirm 0 issues
6. **Score the parameter** with evidence
7. **Report results** to the user: score, evidence, files changed
8. **Mark parameter complete** on the checklist
9. **Repeat** for the next parameter

## Skill Map

| Skill | Parameters Covered |
|-------|-------------------|
| `cqa-vale-check` | P1 |
| `cqa-modularization` | P2, P3, P4, P5, P6, P7 |
| `cqa-titles-descriptions` | P8, P9, P10, P11, Q11 |
| `cqa-procedures` | P12, Q12, Q13, Q14, Q15, Q16 |
| `cqa-editorial` | P13, P14, Q1, Q2, Q3, Q4, Q5, Q18, Q20 |
| `cqa-links` | P15, P16, P17, Q24, Q25 |
| `cqa-legal-branding` | P18, P19, Q17, Q23, O1, O2, O3, O4, O5 |
| `cqa-user-focus` | Q6, Q7, Q8, Q9, Q10 |
| `cqa-tables-images` | Q19, Q21, Q22 |
| `cqa-onboarding` | O6, O7, O8, O9, O10 |
| `cqa-report` | Final report generation |

## Assessment Order

Recommended order (dependencies flow downward):

1. `cqa-vale-check` — foundational; fixes here affect other checks
2. `cqa-modularization` — structural compliance
3. `cqa-titles-descriptions` — metadata quality
4. `cqa-procedures` — procedure structure
5. `cqa-editorial` — writing quality
6. `cqa-links` — cross-references and URLs
7. `cqa-legal-branding` — compliance
8. `cqa-user-focus` — content quality
9. `cqa-tables-images` — visual elements
10. `cqa-onboarding` — publishing readiness
11. `cqa-report` — final summary

## Important Rules

- **Fix everything, then score.** Do not score a parameter until all fixable issues are resolved.
- **0 errors, 0 warnings for automated checks.** Never suppress or ignore.
- **Evidence before claims.** Run the verification command, read the output, then state the score.
- **Update CLAUDE.md** with any new rules discovered during the assessment.
