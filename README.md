# CQA 2.1 Assessment Plugin for Claude Code

A Claude Code plugin for running CQA 2.1 (Content Quality Assessment) against Red Hat modular documentation repositories. Covers 54 assessment parameters across three tabs: Pre-migration, Quality, and Onboarding to docs.redhat.com.

## Installation

```bash
# Install the plugin in Claude Code
claude plugins add /path/to/cqa-assessment
```

## Skills

| Skill | Parameters | Description |
|-------|-----------|-------------|
| `cqa-assess` | All | Main orchestrator. Guides through the full CQA assessment. |
| `cqa-vale-check` | P1 | Vale DITA linting (0 errors, 0 warnings) |
| `cqa-modularization` | P2-P7 | Assembly structure, module prefixes, templates, nesting |
| `cqa-titles-descriptions` | P8-P11, Q11 | Titles, short descriptions, assembly intros |
| `cqa-procedures` | P12, Q12-Q16 | Prerequisites, step count, verification, examples |
| `cqa-editorial` | P13-P14, Q1-Q5, Q18, Q20 | Grammar, readability, style guide, tone |
| `cqa-links` | P15-P17, Q24-Q25 | Broken links, redirects, interlinking |
| `cqa-legal-branding` | P18-P19, Q17, Q23, O1-O5 | Product names, disclaimers, conscious language |
| `cqa-user-focus` | Q6-Q10 | Persona targeting, acronyms, admonitions |
| `cqa-tables-images` | Q19, Q21-Q22 | Screenshots, table captions, image alt text |
| `cqa-onboarding` | O6-O10 | Publishing readiness, SME verification |
| `cqa-report` | Final | Generate the assessment report |

## Usage

### Full assessment

Invoke the `cqa-assess` skill to walk through all parameters:

```
Use the cqa-assess skill to run a full CQA assessment
```

### Individual parameter

Run a specific check:

```
Use the cqa-vale-check skill to assess Vale DITA compliance
```

### Recommended order

1. `cqa-vale-check` (foundational structural compliance)
2. `cqa-modularization` (module structure)
3. `cqa-titles-descriptions` (metadata quality)
4. `cqa-procedures` (procedure structure)
5. `cqa-editorial` (writing quality)
6. `cqa-links` (cross-references)
7. `cqa-legal-branding` (compliance)
8. `cqa-user-focus` (content quality)
9. `cqa-tables-images` (visual elements)
10. `cqa-onboarding` (publishing readiness)
11. `cqa-report` (generate final report)

## Prerequisites

- Claude Code CLI
- `vale` v3.x+ (for P1)
- `asciidoctor-dita-vale` styles (for P1)
- `python3` (for validate-refs.py, cqa-audit.py)
- Access to the target documentation repository

## Scoring

| Score | Label | Meaning |
|-------|-------|---------|
| 4 | Meets criteria | Zero issues |
| 3 | Mostly meets | Minor issues, not blocking |
| 2 | Mostly does not meet | Significant issues |
| 1 | Does not meet | Critical blockers |

## References

- `references/scoring-guide.md` - Detailed scoring rules and parameter-to-skill mapping
- `references/checklist.md` - Full 54-parameter checklist

## License

MIT
