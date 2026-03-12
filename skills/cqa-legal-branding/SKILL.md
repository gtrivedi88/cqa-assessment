---
name: cqa-legal-branding
description: Use when assessing CQA parameters P18-P19, Q17, Q23, O1-O5 (legal, branding, and compliance). Checks product names, Tech Preview disclaimers, conscious language, non-RH link disclaimers, and copyright.
---

# CQA P18-P19, Q17, Q23, O1-O5: Legal and Branding

## Parameters

| # | Parameter | Level |
|---|-----------|-------|
| P18 | Official product names used (attributes, not hardcoded) | Required |
| P19 | Tech Preview/Developer Preview disclaimers present | Required |
| Q17 | Non-RH links acknowledged or disclaimed | Important |
| Q23 | Conscious language guidelines followed | Required |
| O1 | Content follows RH brand and style guidelines | Required |
| O2 | Copyright and legal notices present | Required |
| O3 | Official product names used | Required |
| O4 | Conscious language guidelines followed | Required |
| O5 | Tech Preview disclaimers present | Required |

## Checks

<!-- TODO: Add detailed check procedures as this parameter is assessed -->

### P18/O1/O3: Product names

Search for hardcoded product names:
- "OpenShift Container Platform" -> `{ocp}`
- "OpenShift" (standalone) -> `{orch-name}`
- "Red Hat OpenShift Dev Spaces" -> `{prod}`
- "Dev Spaces" (standalone) -> `{prod-short}`
- "Openshift" (lowercase S) -> `OpenShift`

### P19/O5: Tech Preview disclaimers

Any mention of "Technology Preview" or "Developer Preview" must include a formal admonition block with the standard Red Hat disclaimer.

### Q17: Non-RH link disclaimers

External links to non-Red Hat sites should have appropriate disclaimers. Options:
- Per-link disclaimer
- Guide-level blanket disclaimer
- Link categorization (official upstream vs community)

### Q23/O4: Conscious language

Search for:
- "master" (except in upstream GitHub URLs with no `main` alternative)
- "slave"
- "whitelist" / "blacklist" -> "allowlist" / "denylist"
- "dummy" -> "placeholder" or "example"

### O2: Copyright

- LICENCE file present
- `docinfo.xml` provides document metadata

## Scoring

See [scoring-guide.md](../../references/scoring-guide.md).
