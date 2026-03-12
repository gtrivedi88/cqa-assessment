---
name: cqa-user-focus
description: Use when assessing CQA parameters Q6-Q10 (user focus). Checks persona targeting, pain point coverage, acronym expansion, Additional resources quality, and admonition density.
---

# CQA Q6-Q10: User Focus

## Parameters

| # | Parameter | Level |
|---|-----------|-------|
| Q6 | Content applies to target persona (admin vs developer) | Important |
| Q7 | Content addresses user pain points | Important |
| Q8 | New terms and acronyms defined before use | Important |
| Q9 | Additional resources include useful links across RH sites | Important |
| Q10 | Admonitions not overused (max 3-4 per file) | Important |

## Checks

<!-- TODO: Add detailed check procedures as this parameter is assessed -->

### Q6: Persona targeting

- Admin Guide content targets cluster administrators
- User Guide content targets developers
- No misplaced content between guides

### Q7: Pain points

- Troubleshooting sections address common issues
- Known limitations documented
- Workarounds provided where applicable

### Q8: Acronyms

First use of each acronym per guide must be expanded. Common acronyms to check:
CLI, TLS, OAuth, DNS, API, HTTP, SSH, RBAC, OLM, PVC, UDI, CRD, CR, FQDN, IDE, OIDC, CA, CORS

### Q9: Additional resources quality

- Links point to relevant Red Hat documentation, knowledge base, or upstream docs
- Cross-guide links use correct `{prod-ag-url}` / `{prod-ug-url}` attributes

### Q10: Admonition density

- Maximum 3-4 admonitions per file
- Flag files exceeding this threshold
- Review TIP overuse

## Scoring

See [scoring-guide.md](../../references/scoring-guide.md).
