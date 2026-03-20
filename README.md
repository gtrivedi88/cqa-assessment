# CQA 2.1 Assessment Toolkit

Assess, fix, and score Red Hat modular documentation against all 54 CQA 2.1 parameters.

```text
                         ┌─────────┐
                         │  Assess  │  Run checks against your docs
                         └────┬─────┘
                              │
                         ┌────▼─────┐
                         │   Fix    │  Fix issues found (optional)
                         └────┬─────┘
                              │
                         ┌────▼─────┐
                         │  Score   │  Score each parameter (1-4) with evidence
                         └────┬─────┘
                              │
                         ┌────▼─────┐
                         │  Report  │  Before-and-after summary for JIRA/MRs
                         └──────────┘
```

**What it checks**: 54 parameters across three CQA tabs — Pre-migration (P1-P19), Quality (Q1-Q25), and Onboarding (O1-O10) to docs.redhat.com.

**Choose your scope**: entire docs repo, a single assembly and its topics, or one topic file.

**Choose your mode**: assess only (report issues, don't touch files) or assess and fix (fix issues, re-verify, then score).

**Works with any AI assistant**: Claude Code, Cursor, GitHub Copilot, Windsurf, Gemini CLI — or use manually with no AI at all.

> **Plugin version**: This toolkit is also available as a plugin for
> [redhat-docs-agent-tools](https://github.com/redhat-documentation/redhat-docs-agent-tools)
> under `plugins/cqa-tools/`, with native command support (`/cqa-tools:cqa-assess`).

## Quick start

### 1. Clone the repo

```bash
git clone git@github.com:gtrivedi88/cqa-assessment.git
```

### 2. Use with your AI assistant

Point your AI assistant at the assessment guide for the parameter group you want to check. The guides in `skills/` are plain markdown files that any AI tool can read and follow. The AI runs the automation scripts automatically as part of the assessment.

**Claude Code** (native plugin support):

```bash
claude plugins add /path/to/cqa-assessment
```

Then invoke skills directly:

```
/cqa-vale-check
/cqa-modularization
/cqa-assess           # Full assessment, all 54 parameters
```

**Cursor, Copilot, Windsurf, Gemini CLI, or any AI assistant:**

Paste or reference the relevant `skills/*/SKILL.md` file in your prompt:

```
Read skills/cqa-vale-check/SKILL.md and follow its instructions
to run a Vale DITA check on /path/to/your-docs-repo
```

```
Read skills/cqa-editorial/SKILL.md and assess the editorial quality
of /path/to/your-docs-repo
```

**Manual (no AI)**:

Open any `skills/*/SKILL.md` file and follow the check procedures, commands, and scoring rubrics manually. The guides are written as step-by-step methodology documents.

## Prerequisites

| Requirement | Version | Used by |
|-------------|---------|---------|
| Python | 3.9+ | Automation scripts (stdlib only, no pip install needed) |
| [Vale](https://vale.sh/) | v3.x+ | P1: Vale DITA linting |
| [asciidoctor-dita-vale](https://github.com/redhat-documentation/vale-at-red-hat) styles | Latest | P1: Vale DITA linting |

Optional for Claude Code integration:

| Requirement | Version | Used by |
|-------------|---------|---------|
| [Claude Code CLI](https://docs.anthropic.com/en/docs/claude-code) | Latest | Native plugin and skill invocation |

## Assessment guides

Twelve guides in `skills/`, each covering a group of CQA parameters with detailed check procedures, scoring rubrics, automation references, and fix examples. Run them in this order — fixes in earlier steps prevent false positives in later ones:

| Order | Guide | Parameters | What it assesses |
|-------|-------|------------|------------------|
| — | [`cqa-assess`](skills/cqa-assess/SKILL.md) | All | Main orchestrator — runs all guides below in order |
| 1 | [`cqa-vale-check`](skills/cqa-vale-check/SKILL.md) | P1 | Vale DITA linting — foundational, fixes here affect all other checks |
| 2 | [`cqa-modularization`](skills/cqa-modularization/SKILL.md) | P2-P7 | Assembly structure, module prefixes, content types, nesting rules |
| 3 | [`cqa-titles-descriptions`](skills/cqa-titles-descriptions/SKILL.md) | P8-P11 | Title quality, short descriptions, DITA abstracts |
| 4 | [`cqa-procedures`](skills/cqa-procedures/SKILL.md) | P12, Q12-Q16 | Prerequisites, step count, command examples, verification sections |
| 5 | [`cqa-editorial`](skills/cqa-editorial/SKILL.md) | P13-P14, Q1-Q5, Q18, Q20 | Scannability, readability, complex words, fluff, tone, style guide |
| 6 | [`cqa-links`](skills/cqa-links/SKILL.md) | P15-P17, Q24-Q25 | Broken xrefs, missing includes, dead URLs, content journey |
| 7 | [`cqa-legal-branding`](skills/cqa-legal-branding/SKILL.md) | P18-P19, Q17, Q23, O1-O5 | Product names, TP/DP disclaimers, conscious language, legal notices |
| 8 | [`cqa-user-focus`](skills/cqa-user-focus/SKILL.md) | Q6-Q11 | Persona targeting, acronym expansion, admonition density, assembly intros |
| 9 | [`cqa-tables-images`](skills/cqa-tables-images/SKILL.md) | Q19, Q21-Q22 | Screenshots, table captions and headers, image alt text |
| 10 | [`cqa-onboarding`](skills/cqa-onboarding/SKILL.md) | O6-O10 | Support disclaimers, SME verification, Pantheon publishing |
| 11 | [`cqa-report`](skills/cqa-report/SKILL.md) | Final | Before-and-after summary report with scores and evidence |

## Usage examples

You can scope the assessment to an entire repo, a parameter group, a single assembly (and its topics), or a single topic file.

### Full repo assessment

```
Read skills/cqa-assess/SKILL.md and run a full CQA 2.1 assessment
on /path/to/docs-repo
```

### One parameter group across the repo

```
Read skills/cqa-editorial/SKILL.md and check editorial quality
across /path/to/docs-repo
```

### One assembly and its topics

An assembly includes multiple topics. Scoping to an assembly assesses the assembly file and every topic it includes. Use `resolve-includes.py` to list all included files:

```bash
python3 skills/cqa-assess/scripts/resolve-includes.py \
  /path/to/docs-repo/assemblies/admin/assembly_configuring-oauth.adoc \
  --base-dir /path/to/docs-repo
```

Then pass the file list to the assessment:

```
Read skills/cqa-titles-descriptions/SKILL.md and assess titles and
abstracts in assemblies/administration_guide/assembly_configuring-oauth.adoc
and all the topics it includes
```

### One topic file

```
Read skills/cqa-editorial/SKILL.md and check editorial quality in
topics/administration_guide/proc_configuring-proxy.adoc
```

### Assess only vs assess and fix

By default, the AI will report issues and offer to fix them. You can control this:

```
# Report only — don't modify any files
Read skills/cqa-editorial/SKILL.md and assess editorial quality in
/path/to/docs-repo. Report issues but do not fix them.

# Assess and fix
Read skills/cqa-editorial/SKILL.md and assess editorial quality in
/path/to/docs-repo. Fix all issues found.
```

## Automation scripts

Ten check scripts and one utility in [`skills/cqa-assess/scripts/`](skills/cqa-assess/scripts/) automate repeatable CQA checks. When using an AI assistant, these scripts are **run automatically** — the assessment guides reference the right script for each parameter, and the AI executes them as part of the assessment workflow. No manual invocation needed.

For manual use or CI integration, each script accepts a docs repo path, prints structured output, and exits `0` (pass), `1` (issues found), or `2` (invalid arguments). Python 3.9+ stdlib only — zero external dependencies.

**Directory support:** All scripts scan `assemblies/`, `modules/`, `topics/`, and `snippets/` by default. Repos using `modules/` instead of `topics/` work without configuration. Use `--scan-dirs` to override.

| Script | Parameters | What it checks | Extra flags |
|--------|-----------|----------------|-------------|
| `check-product-names.py` | P18, O1, O3 | Hardcoded product names in prose and image alt text | `--config`, `--fix` |
| `check-conscious-language.py` | Q23, O4 | Exclusionary terms (master, slave, whitelist, blacklist, dummy) | |
| `check-content-types.py` | P3, P4, P5 | Filename prefix vs content type declaration, required elements | `--no-prefix-check` |
| `check-tp-disclaimers.py` | P19, O5 | Technology Preview and Developer Preview disclaimer compliance | |
| `check-external-links.py` | Q17 | External URL extraction and domain categorization | `--details` |
| `check-legal-notices.py` | O2 | LICENSE file and docinfo.xml existence | `--repo-root` |
| `check-scannability.py` | Q1 | Sentence length, paragraph length, list conversion opportunities | |
| `check-simple-words.py` | Q3 | Complex word patterns (utilize, leverage, in order to, etc.) | |
| `check-readability.py` | Q4 | Flesch-Kincaid Grade Level per file and overall | |
| `check-fluff.py` | Q5 | Self-referential patterns, forward-referencing, filler phrases | |
| `resolve-includes.py` | (utility) | Recursively resolve `include::` tree from any `.adoc` file | `--format`, `--include-root` |

### Common flags

| Flag | Scripts | Description |
|------|---------|-------------|
| `--scan-dirs` | All `check-*` scripts except `check-legal-notices.py` | Override default scan directories (e.g., `--scan-dirs topics assemblies`) |
| `--config FILE` | `check-product-names.py` | JSON config for repo-specific product names and attributes |
| `--fix` | `check-product-names.py` | Auto-replace hardcoded product names with attributes |
| `--no-prefix-check` | `check-content-types.py` | Detect content type from `:_mod-docs-content-type:` instead of filename prefix |
| `--repo-root DIR` | `check-legal-notices.py` | Repo root for LICENSE file check (auto-detects `.git` if omitted) |
| `--details` | `check-external-links.py` | Show per-URL breakdown by domain |
| `--format` | `resolve-includes.py` | Output format: `files` (default), `tree`, or `json` |
| `--include-root` | `resolve-includes.py` | Include the root file itself in the output |

### Running scripts manually

```bash
# Run a single check
python3 skills/cqa-assess/scripts/check-product-names.py /path/to/docs-repo

# Run all checks at once
for script in skills/cqa-assess/scripts/check-*.py; do
  python3 "$script" /path/to/docs-repo
  echo "---"
done

# Resolve an assembly's include tree
python3 skills/cqa-assess/scripts/resolve-includes.py \
  /path/to/docs-repo/assemblies/admin/assembly_installing.adoc \
  --base-dir /path/to/docs-repo --format tree
```

<details>
<summary><strong>Example output: check-product-names.py</strong></summary>

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
</details>

<details>
<summary><strong>Example output: check-scannability.py</strong></summary>

```
=== Scannability Check (Q1) ===

Scanning topics/ and assemblies/ for sentence length issues...

LONG SENTENCES (>30 words):

  topics/administration_guide/con_architecture-overview.adoc:28
    34 words: "The gateway uses OpenShift OAuth to authenticate users and..."

  topics/user_guide/proc_starting-a-workspace.adoc:15
    31 words: "When you start a workspace from a devfile the system creates..."

FILES WITH HIGH AVERAGE (>22 words/sentence):

  topics/administration_guide/con_running-at-scale.adoc
    Average: 24.3 words/sentence (18 sentences)

Summary: 2 long sentences, 1 high-average file
Exit code: 1
```
</details>

<details>
<summary><strong>Example output: check-readability.py</strong></summary>

```
=== Readability Check (Q4) ===

Calculating Flesch-Kincaid Grade Level...

GRADE DISTRIBUTION:
  Grade 6-8:   ████████████████████ 42 files (22%)
  Grade 9-10:  ████████████████████████████████████ 89 files (47%)
  Grade 11-12: ██████████████████████ 52 files (27%)
  Grade 13+:   ███ 7 files (4%)

FILES ABOVE GRADE 12:
  topics/administration_guide/ref_checluster-fields.adoc       Grade 14.2
  topics/administration_guide/con_security-best-practices.adoc  Grade 13.1
  ...

Overall average: 10.4 (target: ≤12)
PASS
```
</details>

## Scoring

Every parameter is scored on a 4-point scale:

| Score | Label | Meaning |
|-------|-------|---------|
| **4** | Meets criteria | Zero issues found |
| **3** | Mostly meets | Minor issues, not blocking publication |
| **2** | Mostly does not meet | Significant issues requiring remediation |
| **1** | Does not meet | Critical blockers preventing publication |

The `cqa-report` guide compiles all scores into a summary table with per-parameter evidence and an overall rating.

## Project structure

```
cqa-assessment/
├── .claude-plugin/                        # Claude Code integration (optional)
│   └── plugin.json
├── commands/                              # Command definitions
│   └── cqa-assess.md                      # Full assessment orchestrator
├── skills/                                # 12 assessment guides (plain markdown)
│   ├── cqa-assess/                        # Main orchestrator
│   │   ├── SKILL.md
│   │   └── scripts/                       # 11 automation scripts (Python 3.9+)
│   │       ├── check-product-names.py
│   │       ├── check-conscious-language.py
│   │       ├── check-content-types.py
│   │       ├── check-tp-disclaimers.py
│   │       ├── check-external-links.py
│   │       ├── check-legal-notices.py
│   │       ├── check-scannability.py
│   │       ├── check-simple-words.py
│   │       ├── check-readability.py
│   │       ├── check-fluff.py
│   │       └── resolve-includes.py
│   ├── cqa-vale-check/SKILL.md            # P1: Vale DITA linting
│   ├── cqa-modularization/SKILL.md        # P2-P7: Module structure
│   ├── cqa-titles-descriptions/SKILL.md   # P8-P11: Titles and abstracts
│   ├── cqa-procedures/SKILL.md            # P12, Q12-Q16: Procedure quality
│   ├── cqa-editorial/SKILL.md             # P13-P14, Q1-Q5, Q18, Q20: Writing
│   ├── cqa-links/SKILL.md                 # P15-P17, Q24-Q25: Links
│   ├── cqa-legal-branding/SKILL.md        # P18-P19, Q17, Q23, O1-O5: Legal
│   ├── cqa-user-focus/SKILL.md            # Q6-Q11: User focus
│   ├── cqa-tables-images/SKILL.md         # Q19, Q21-Q22: Visual elements
│   ├── cqa-onboarding/SKILL.md            # O6-O10: Publishing readiness
│   └── cqa-report/SKILL.md               # Final report generation
└── reference/
    ├── scoring-guide.md                   # Scoring rules and parameter mapping
    └── checklist.md                       # Full 54-parameter checklist
```

## How it works

Each guide in `skills/` is a `SKILL.md` file — a plain markdown document containing:

- **Rules** — what the CQA parameter requires
- **Check procedures** — step-by-step instructions with commands to run
- **Scoring rubrics** — criteria for each score (4, 3, 2, 1)
- **Common issues** — anti-patterns, violations, and fix examples
- **Automation references** — which Python scripts to run for automated checks

You can use these guides three ways:

1. **With an AI assistant** — feed the SKILL.md to your AI tool and let it execute the checks, fix issues, and score parameters
2. **As a Claude Code plugin** — install the repo as a plugin for native skill invocation (`/cqa-vale-check`, `/cqa-assess`, etc.)
3. **Manually** — read the guide and follow the procedures yourself

The guides reference [Red Hat modular documentation framework](https://redhat-documentation.github.io/modular-docs/), [DITA 1.3 spec](https://docs.oasis-open.org/dita/dita/v1.3/dita-v1.3-part3-all-inclusive.html), [IBM Style guide](https://www.ibm.com/docs/en/ibm-style), and [Red Hat supplementary style guide](https://redhat-documentation.github.io/supplementary-style-guide/) as authoritative sources.

## References

- [`reference/scoring-guide.md`](reference/scoring-guide.md) — Scoring rules, thresholds, and parameter-to-skill mapping
- [`reference/checklist.md`](reference/checklist.md) — Full 54-parameter CQA 2.1 checklist
- [Red Hat modular docs guide](https://redhat-documentation.github.io/modular-docs/) — Module types, templates, and assembly rules
- [DITA 1.3 spec](https://docs.oasis-open.org/dita/dita/v1.3/dita-v1.3-part3-all-inclusive.html) — Short description and structural requirements
- [Vale](https://vale.sh/) — Prose linting engine used for P1

## License

MIT
