---
name: cqa-onboarding
description: Use when assessing CQA parameters O6-O10 (onboarding to docs.redhat.com). Checks support disclaimers, SME verification, source format, Pantheon publishing, and official site publication.
---

# CQA O6-O10: Onboarding

## Parameters

| # | Parameter | Level |
|---|-----------|-------|
| O6 | Content supported or clear disclaimers about support level | Required |
| O7 | Procedures tested and verified by SME or QE | Required |
| O8 | Source files in RH prescribed format (AsciiDoc) | Required |
| O9 | Content published through Pantheon | Required |
| O10 | Content published to official Red Hat site | Required |

## Checks

<!-- TODO: Add detailed check procedures as this parameter is assessed -->

### O6: Support disclaimers

- Community-supported components identified
- Unsupported configurations have disclaimers
- Technology Preview features have formal disclaimer blocks

### O7: SME/QE verification

This parameter requires human confirmation:
- Ask engineering/QE team if procedures have been tested
- Cannot be assessed through static analysis alone
- Score "No data" if confirmation unavailable

### O8: Source format

- All content files are `.adoc` (AsciiDoc)
- UTF-8 encoding, LF line endings
- Red Hat modular documentation structure

### O9: Pantheon publishing

- `tools/ccutil.sh` configured and functional
- `pantheon/` directory with symlinks to `titles/`
- CI pipeline includes ccutil build job
- Build produces html-single, html, pdf, epub

### O10: Official site

- Content published to docs.redhat.com
- Stage branch triggers Pantheon publishing pipeline
- Published content is accessible and renders correctly

## Scoring

See [scoring-guide.md](../../references/scoring-guide.md).
