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

## Automation Scripts

Reusable Python scripts in `scripts/` automate CQA parameter checks. Each script accepts a docs repo path as argument, prints structured output, and exits 0 (pass) or 1 (issues found). Python 3.9+ stdlib only, no dependencies.

| Script | Skill | Parameters | What it checks |
|--------|-------|-----------|----------------|
| `check-product-names.py` | `cqa-legal-branding` | P18, O1, O3 | Hardcoded product names in prose and image alt text |
| `check-conscious-language.py` | `cqa-legal-branding` | Q23, O4 | Exclusionary terms with whole-word matching and exception handling |
| `check-content-types.py` | `cqa-modularization` | P3, P4, P5 | Filename prefix vs content type match, required elements, block titles |
| `check-tp-disclaimers.py` | `cqa-legal-branding` | P19, O5 | TP/DP mentions, snippet existence, disclaimer compliance |
| `check-external-links.py` | `cqa-legal-branding` | Q17 | External URL extraction and domain categorization |
| `check-legal-notices.py` | `cqa-legal-branding` | O2 | LICENSE file and docinfo.xml existence |
| `check-scannability.py` | `cqa-editorial` | Q1 | Sentence length >30 words, file average >22 words/sentence |
| `check-simple-words.py` | `cqa-editorial` | Q3 | Complex word patterns (utilize, leverage, in order to, etc.) |
| `check-readability.py` | `cqa-editorial` | Q4 | Flesch-Kincaid Grade Level per file and overall |
| `check-fluff.py` | `cqa-editorial` | Q5 | Self-referential patterns, forward-referencing, filler phrases |

Run all scripts:

```bash
for script in scripts/check-*.py; do
  python3 "$script" /path/to/docs-repo/
  echo "---"
done
```

## Prerequisites

- Claude Code CLI
- `vale` v3.x+ (for P1)
- `asciidoctor-dita-vale` styles (for P1)
- `python3` 3.9+ (for automation scripts)
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
