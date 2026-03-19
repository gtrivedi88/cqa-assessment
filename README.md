# CQA 2.1 Assessment Plugin for Claude Code

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) plugin that runs CQA 2.1 (Content Quality Assessment) against Red Hat modular documentation repositories. It covers all 54 assessment parameters across three tabs — Pre-migration, Quality, and Onboarding to docs.redhat.com — through 12 specialized skills and 10 automation scripts.

## Quick start

### 1. Clone the plugin

```bash
git clone git@github.com:gtrivedi88/cqa-assessment.git
```

### 2. Install into Claude Code

```bash
claude plugins add /path/to/cqa-assessment
```

### 3. Run your first check

Open Claude Code in your docs repo directory and type:

```
/cqa-vale-check
```

Claude walks you through Vale DITA linting, fixes any issues, and scores the parameter.

## Prerequisites

| Requirement | Version | Used by |
|-------------|---------|---------|
| [Claude Code CLI](https://docs.anthropic.com/en/docs/claude-code) | Latest | All skills |
| [Vale](https://vale.sh/) | v3.x+ | `cqa-vale-check` (P1) |
| [asciidoctor-dita-vale](https://github.com/redhat-documentation/vale-at-red-hat) styles | Latest | `cqa-vale-check` (P1) |
| Python | 3.9+ | Automation scripts |

The automation scripts use Python stdlib only — no `pip install` needed.

## Usage

### Run a full assessment

The `cqa-assess` skill is the main orchestrator. It walks you through every parameter in dependency order, invokes the right sub-skill for each group, runs automated checks, fixes issues, and scores each parameter with evidence.

```
/cqa-assess
```

**Example prompt:**

```
Use the cqa-assess skill to run a full CQA 2.1 assessment on /home/user/docs/devspaces-documentation
```

Claude will:
1. Ask for your docs repo path (if not provided)
2. Start with P1 (Vale DITA linting) and work through all 54 parameters
3. Run automated scripts where available
4. Flag issues, fix them, and re-verify
5. Score each parameter (1-4) with evidence
6. Generate a final report at the end

### Run a specific parameter group

Each parameter group has its own skill. Invoke it directly to check just that area:

```
/cqa-vale-check
```

**Example prompts:**

```
# Check Vale DITA compliance (P1)
Use the cqa-vale-check skill to run Vale against /home/user/docs/devspaces-documentation

# Check modularization (P2-P7)
Use the cqa-modularization skill to verify module structure in /home/user/docs/devspaces-documentation

# Check titles and short descriptions (P8-P11, Q11)
Use the cqa-titles-descriptions skill to assess abstract quality in /home/user/docs/devspaces-documentation

# Check procedures (P12, Q12-Q16)
Use the cqa-procedures skill to check prerequisites, step count, and verification sections

# Check editorial quality (P13-P14, Q1-Q5, Q18, Q20)
Use the cqa-editorial skill to check scannability, readability, and tone

# Check links (P15-P17, Q24-Q25)
Use the cqa-links skill to find broken xrefs, missing includes, and dead URLs

# Check legal and branding (P18-P19, Q17, Q23, O1-O5)
Use the cqa-legal-branding skill to check product names, disclaimers, and conscious language

# Check user focus (Q6-Q10)
Use the cqa-user-focus skill to check persona targeting, acronyms, and admonitions

# Check tables and images (Q19, Q21-Q22)
Use the cqa-tables-images skill to check screenshots, table captions, and alt text

# Check onboarding readiness (O6-O10)
Use the cqa-onboarding skill to check Pantheon publishing, SME verification, and stage branches

# Generate final report
Use the cqa-report skill to compile the assessment report
```

### Recommended assessment order

Run skills in this order — fixes in earlier checks prevent false positives in later ones:

| Order | Skill | Parameters | Why this order |
|-------|-------|------------|----------------|
| 1 | `cqa-vale-check` | P1 | Foundational — Vale fixes affect all other checks |
| 2 | `cqa-modularization` | P2-P7 | Structural compliance must be correct before content checks |
| 3 | `cqa-titles-descriptions` | P8-P11, Q11 | Titles and abstracts set the context for everything else |
| 4 | `cqa-procedures` | P12, Q12-Q16 | Procedure structure (prerequisites, steps, verification) |
| 5 | `cqa-editorial` | P13-P14, Q1-Q5, Q18, Q20 | Writing quality (scannability, readability, tone) |
| 6 | `cqa-links` | P15-P17, Q24-Q25 | Cross-references and URL validation |
| 7 | `cqa-legal-branding` | P18-P19, Q17, Q23, O1-O5 | Branding, disclaimers, conscious language |
| 8 | `cqa-user-focus` | Q6-Q10 | Audience targeting and content quality |
| 9 | `cqa-tables-images` | Q19, Q21-Q22 | Visual elements (screenshots, tables, alt text) |
| 10 | `cqa-onboarding` | O6-O10 | Publishing readiness (Pantheon, stage branches) |
| 11 | `cqa-report` | Final | Compile scores and evidence into a report |

## Automation scripts

Ten Python scripts in [`scripts/`](scripts/) automate repeatable CQA checks. Each script:

- Accepts a docs repo path as its only argument
- Prints structured, human-readable output
- Exits `0` (pass) or `1` (issues found)
- Uses Python 3.9+ stdlib only — zero dependencies

### Run a single script

```bash
python3 scripts/check-product-names.py /home/user/docs/devspaces-documentation
```

**Example output:**

```
=== Product Name Check (P18, O1, O3) ===

Checking for hardcoded product names in prose and image alt text...

ISSUES FOUND:

  topics/administration_guide/proc_configuring-oauth.adoc:42
    Found: "OpenShift Dev Spaces" → use {prod-short}

  topics/user_guide/con_workspaces.adoc:15
    Found: "OpenShift Container Platform" → use {ocp}

Summary: 2 issues in 2 files
Exit code: 1
```

### Run all scripts at once

```bash
for script in scripts/check-*.py; do
  python3 "$script" /home/user/docs/devspaces-documentation
  echo "---"
done
```

### Script reference

| Script | Parameters | What it checks |
|--------|-----------|----------------|
| [`check-product-names.py`](scripts/check-product-names.py) | P18, O1, O3 | Hardcoded product names in prose and image alt text. Auto-excludes code blocks, comments, UI labels, and plugin names. |
| [`check-conscious-language.py`](scripts/check-conscious-language.py) | Q23, O4 | Exclusionary terms (master, slave, whitelist, blacklist, dummy, sanity check) with whole-word matching. Auto-excludes URLs, filenames, and code blocks. |
| [`check-content-types.py`](scripts/check-content-types.py) | P3, P4, P5 | Filename prefix vs `:_mod-docs-content-type:` match, required elements (`[role="_abstract"]`, `[id="..._{context}"]`), invalid block titles in wrong module types, `==` subsections in procedures. |
| [`check-tp-disclaimers.py`](scripts/check-tp-disclaimers.py) | P19, O5 | Technology Preview and Developer Preview mentions classified by context (prose, table, link, comment, code block). Validates snippet existence and disclaimer compliance. |
| [`check-external-links.py`](scripts/check-external-links.py) | Q17 | Extracts all external URLs and categorizes domains (Red Hat, Upstream/Community, Authoritative, Third-party). Filters placeholders and unresolved attributes. |
| [`check-legal-notices.py`](scripts/check-legal-notices.py) | O2 | Verifies LICENSE/LICENCE file existence and `docinfo.xml` in each `titles/*/` directory. Detects copyright year. |
| [`check-scannability.py`](scripts/check-scannability.py) | Q1 | Flags sentences over 30 words and files averaging over 22 words/sentence. Handles AsciiDoc structure (list items, code blocks, tables, definition lists). |
| [`check-simple-words.py`](scripts/check-simple-words.py) | Q3 | Flags complex word patterns: utilize, leverage, in order to, prior to, subsequent to, commence, terminate, facilitate, aforementioned, in the event that. |
| [`check-readability.py`](scripts/check-readability.py) | Q4 | Calculates Flesch-Kincaid Grade Level per file and overall. Resolves AsciiDoc attributes for accurate syllable counting. Pass threshold: overall grade level 12 or below. |
| [`check-fluff.py`](scripts/check-fluff.py) | Q5 | Flags self-referential patterns (This section describes, Learn how to), forward-referencing (The following describes), and filler phrases (It is important to note that, Please note that). |

## Scoring

Every parameter is scored on a 4-point scale:

| Score | Label | Meaning |
|-------|-------|---------|
| **4** | Meets criteria | Zero issues found |
| **3** | Mostly meets | Minor issues, not blocking publication |
| **2** | Mostly does not meet | Significant issues requiring remediation |
| **1** | Does not meet | Critical blockers preventing publication |

The `cqa-report` skill compiles all scores into a summary table with per-parameter evidence and an overall rating.

## Project structure

```
cqa-assessment/
├── .claude-plugin/
│   └── plugin.json              # Plugin metadata
├── skills/                      # 12 assessment skills
│   ├── cqa-assess/SKILL.md      # Main orchestrator
│   ├── cqa-vale-check/SKILL.md  # P1: Vale DITA linting
│   ├── cqa-modularization/      # P2-P7: Module structure
│   ├── cqa-titles-descriptions/ # P8-P11, Q11: Titles and abstracts
│   ├── cqa-procedures/          # P12, Q12-Q16: Procedure quality
│   ├── cqa-editorial/           # P13-P14, Q1-Q5, Q18, Q20: Writing
│   ├── cqa-links/               # P15-P17, Q24-Q25: Links
│   ├── cqa-legal-branding/      # P18-P19, Q17, Q23, O1-O5: Legal
│   ├── cqa-user-focus/          # Q6-Q10: User focus
│   ├── cqa-tables-images/       # Q19, Q21-Q22: Visual elements
│   ├── cqa-onboarding/          # O6-O10: Publishing readiness
│   └── cqa-report/              # Final report generation
├── scripts/                     # 10 automation scripts
│   ├── check-product-names.py
│   ├── check-conscious-language.py
│   ├── check-content-types.py
│   ├── check-tp-disclaimers.py
│   ├── check-external-links.py
│   ├── check-legal-notices.py
│   ├── check-scannability.py
│   ├── check-simple-words.py
│   ├── check-readability.py
│   └── check-fluff.py
└── references/
    ├── scoring-guide.md         # Scoring rules and parameter mapping
    └── checklist.md             # Full 54-parameter checklist
```

## How it works

Each skill in `skills/` contains a `SKILL.md` file that teaches Claude Code how to assess a group of CQA parameters. When you invoke a skill, Claude:

1. **Reads the skill definition** to understand the rules, checks, and scoring criteria
2. **Identifies your docs repo** (asks for the path or uses the current directory)
3. **Runs automated checks** using the Python scripts in `scripts/` where available
4. **Performs manual review** for parameters that require contextual understanding (tone, audience targeting, content quality)
5. **Fixes issues** directly in the source files
6. **Re-runs verification** to confirm 0 issues remain
7. **Scores the parameter** with evidence (command output, file diffs, review notes)

The skills reference Red Hat's modular documentation framework, DITA 1.3 spec, IBM Style guide, and Red Hat supplementary style guide as authoritative sources.

## References

- [`references/scoring-guide.md`](references/scoring-guide.md) — Scoring rules, thresholds, and parameter-to-skill mapping
- [`references/checklist.md`](references/checklist.md) — Full 54-parameter CQA 2.1 checklist
- [Red Hat modular docs guide](https://redhat-documentation.github.io/modular-docs/) — Module types, templates, and assembly rules
- [DITA 1.3 spec](https://docs.oasis-open.org/dita/dita/v1.3/dita-v1.3-part3-all-inclusive.html) — Short description and structural requirements
- [Vale](https://vale.sh/) — Prose linting engine used for P1

## License

MIT
