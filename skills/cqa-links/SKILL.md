---
name: cqa-links
description: Use when assessing CQA parameters P15-P17, Q24-Q25 (links and URLs). Checks for broken xrefs, missing includes, missing images, redirect integrity, and content interlinking.
---

# CQA P15-P17, Q24-Q25: Links and URLs

## Parameters

| # | Parameter | Level |
|---|-----------|-------|
| P15 | No broken links (xrefs, includes, images) | Required |
| P16 | Redirects work correctly | Required |
| P17 | Content interlinked within 3 clicks of domain home | Important |
| Q24 | Content includes links to relevant content journey | Important |
| Q25 | Pages interlinked within 3 clicks | Important |

## Step 1: Identify the docs repo

Ask the user for the path to their Red Hat modular documentation repository. Store as `DOCS_REPO`.

## Step 2: P15 — No broken links

### Rule

Every cross-reference, include directive, and image reference must resolve to an existing target. No duplicate IDs are allowed.

### Check procedure

Run the reference validation script:
```bash
cd "$DOCS_REPO"
python3 scripts/validate-refs.py
```

This checks 4 things:
1. **Broken xrefs** — every `xref:ID_{context}[]` must have a matching `[id="ID_{context}"]` declared in any `.adoc` file
2. **Missing includes** — every `include::path[]` must resolve to an existing file on disk (paths resolve relative to the file via symlinks in `titles/*/`)
3. **Missing images** — every `image::path[]` must resolve to a file under `images/` (`:imagesdir:` is set in `common/attributes.adoc`)
4. **Duplicate IDs** — no two files should declare the same `[id="..."]` anchor

### Common causes of broken references

| Cause | Example | Fix |
|-------|---------|-----|
| File renamed without updating xrefs | `xref:old-name_{context}[]` → file now has `[id="new-name_{context}"]` | Update xref to match actual ID |
| File deleted without removing xrefs/includes | `xref:deleted-topic_{context}[]` | Remove the xref or redirect to replacement content |
| ID in file doesn't match filename prefix | File `proc_foo.adoc` has `[id="foo_{context}"]` (no `proc_` prefix) | Xref must match declared ID, not filename |
| Typo in image path | `image::architectre/diagram.png[]` | Fix spelling to match actual path |

### Scoring

| Score | Criteria |
|-------|----------|
| **4** | 0 broken xrefs, 0 missing includes, 0 missing images, 0 duplicate IDs |
| **3** | 1-3 broken references (minor oversights) |
| **2** | Multiple broken references across different files |
| **1** | Widespread broken references or not checked |

## Step 3: P16 — Redirects work correctly

### Rule

When content is renamed, moved, or restructured, old URLs must continue to resolve. The redirect mechanism depends on the publishing platform.

### Check procedure

1. **Identify the publishing platform** — Pantheon (ccutil), GitLab Pages, or other
2. **Check for redirect configuration** — search for `_redirects`, redirect maps, or platform-specific redirect config
3. **Identify recently changed IDs** — compare current `[id="..."]` anchors against any published version to find renamed/deleted pages

### Platform-specific notes

| Platform | Redirect mechanism | Repo responsibility |
|----------|-------------------|---------------------|
| **Pantheon** (ccutil) | Managed at CCS publishing infrastructure level | Repo maintains correct IDs; redirect requests filed with CCS publishing team during stage branch process |
| **GitLab Pages** | `_redirects` file in repo root | Repo must maintain redirect file |
| **Antora** | `page-aliases` attribute in page headers | Repo must set aliases |

### Scoring

| Score | Criteria |
|-------|----------|
| **4** | Redirects managed at platform level OR redirect configuration in place and verified; no Antora module prefixes in links; cross-guide links use `link:` not `xref:` |
| **3** | Minor redirect gaps (1-2 recently renamed pages without redirects) |
| **2** | Multiple broken URLs from recent restructuring with no redirect plan |
| **1** | No redirect strategy or widespread broken published URLs |

## Step 4: P17/Q24/Q25 — Content interlinking

### Rule

Content should be interlinked so that users can navigate between related topics. Every topic should be reachable within 3 clicks from the guide's domain home page.

### Check procedure

1. **Assembly navigation** — verify that assemblies group related topics logically
2. **Cross-references** — verify that related topics reference each other via xref or Additional resources
3. **Concept-procedure linking** — concepts should link to related procedures and vice versa
4. **Cross-guide links** — admin guide and user guide should cross-reference where relevant using `link:{prod-ag-url}` and `link:{prod-ug-url}`

### Scoring

| Score | Criteria |
|-------|----------|
| **4** | All topics reachable within 3 clicks; concepts link to procedures and vice versa; cross-guide links where relevant |
| **3** | Most topics interlinked; a few isolated topics without cross-references |
| **2** | Significant sections with no cross-references to related content |
| **1** | Content is siloed with minimal interlinking |

## Step 5: Verify

After fixing any violations, re-run the reference validation:

```bash
cd "$DOCS_REPO"
python3 scripts/validate-refs.py
```

Then run Vale to ensure no new warnings were introduced:

```bash
vale assemblies/ topics/ titles/administration_guide/master.adoc titles/user_guide/master.adoc
```
