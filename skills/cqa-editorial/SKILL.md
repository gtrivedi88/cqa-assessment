---
name: cqa-editorial
description: Use when assessing CQA parameters P13-P14, Q1-Q5, Q18, Q20 (editorial quality). Checks grammar, content types, readability, scannability, fluff, style guide compliance, and tone.
---

# CQA P13-P14, Q1-Q5, Q18, Q20: Editorial Quality

## Parameters

| # | Parameter | Level |
|---|-----------|-------|
| P13 | Grammatically correct American English | Required |
| P14 | Correct content type matches actual content | Required |
| Q1 | Scannable: sentences <= 22 words avg, paragraphs 2-3 sentences | Required |
| Q2 | Clearly written and understandable | Important |
| Q3 | Simple words (no "utilize", "leverage", "in order to") | Important |
| Q4 | Readability score (11-12th grade level) | Important |
| Q5 | No fluff ("This section describes...", "as mentioned") | Important |
| Q18 | Content follows Red Hat style guide | Required |
| Q20 | Appropriate conversational tone (2nd person, professional) | Important |

## Checks

### P13: Grammar

- American English spelling
- No grammatical errors in titles or body text
- Correct article usage ("a" vs "an")

### P14: Content type

- `:_mod-docs-content-type:` matches actual content
- Filename prefix matches content type
- Procedures have `.Procedure` with ordered list
- Concepts have explanatory content, no `.Procedure`

### Q1: Scannability

#### Thresholds

| Metric | Target | Hard limit |
|--------|--------|------------|
| Sentence length | <= 22 words average | Flag sentences > 30 words |
| Paragraph length | 2-3 sentences | Flag paragraphs > 4 sentences |
| Lists | Use bulleted lists for 3+ items | Flag inline enumerations |

#### What counts as prose (check these)

Only check actual prose text in `topics/` and `assemblies/` files:

- Abstract paragraphs (text after `[role="_abstract"]`)
- Body paragraphs between structural markers
- Prose within admonition blocks (NOTE, WARNING, IMPORTANT, TIP, CAUTION)
- Prose portion of bullet/ordered list items (the text after `* ` or `. `)
- Introductory text in assemblies (before first `include::`)

#### What to skip (not prose)

- Code/literal/passthrough blocks (between `----`, `....`, `++++` delimiters)
- AsciiDoc metadata, attributes (`:attr:` lines), directives (`include::`, `image::`, `ifdef::`)
- Block titles (`.Example`, `.Procedure`, `.Prerequisites`)
- Table content (lines starting with `|`, `|===` delimiters)
- Definition list terms (`term::` entries) — the term itself is not a sentence
- Block attribute annotations (`[source,yaml]`, `[role="_abstract"]`, `[id="..."]`)
- Comments (`// ...`)
- Headings (`= `, `== `)
- List continuation markers (`+` on its own line)
- URL-only list items (`* xref:...[]`, `* link:...[]`)

#### Word counting

- AsciiDoc attributes (`{prod-short}`, `{orch-name}`) count as the number of words they resolve to
- Backtick literals (`\`command\``) count as 1 word regardless of content
- Link macros: count only the link text, not the URL
- Leading list markers (`* `, `. `, `.. `) are not words

#### How to assess sentence length

Read each prose paragraph in every file in `topics/` and `assemblies/`. For each paragraph:

1. Split into sentences (split on `. ` followed by uppercase, `? `, `! `)
2. Count words per sentence (using word counting rules above)
3. Flag any sentence over 30 words
4. Note the file's average sentence length

Each list item is an independent unit — do not concatenate consecutive list items into a single "paragraph."

#### Sentence splitting patterns

When a sentence exceeds 30 words, split it using these patterns:

| Pattern | Split point | Example |
|---------|-------------|---------|
| "..., so that..." | Split at ", so that" | "Configure X. This allows Y." |
| "..., as..." (causal) | Split at ", as" | "X happens. The reason is Y." |
| "..., which..." (non-restrictive) | Split at ", which" | "X does Y. It also does Z." |
| "... to ... to ..." (chained infinitives) | Split after first purpose | "Do X. This enables Y." |
| "..., or ..." (alternative actions) | Split at ", or" | "Do X. Alternatively, do Y." |
| Inline enumeration | Convert to bulleted list | `The supported values are:\n* X\n* Y\n* Z` |
| Abstract with WHAT + WHY | Split into two sentences | "Do X to achieve Y." → "Do X. This achieves Y." |

#### How to assess paragraph length

A paragraph is a block of consecutive prose lines separated by blank lines. For each paragraph:

1. Count the number of sentences
2. Flag paragraphs with more than 4 sentences
3. Split long paragraphs at logical topic shifts by inserting a blank line

#### False positives to ignore

These patterns look like long sentences/paragraphs but are structured content:

| Pattern | Why it's not a scannability issue |
|---------|-----------------------------------|
| Definition list entries (`term:: description`) | Renders as formatted key-value pairs, not prose |
| Consecutive bullet items (`* item1\n* item2\n* item3`) | Each item is independent; they are not one paragraph |
| Procedure sub-steps (`.. step1\n.. step2`) | Ordered sub-steps render as a nested list |
| CSV-like metric tables | Renders as structured data |
| Code block annotations with backtick-heavy content | Technical identifiers inflate word count |
| Link-heavy sentences (URLs inside `link:...[text]`) | URLs inflate raw character/word count |

#### Bulleted lists

Check for inline enumerations that would be more scannable as lists:

- Flag: "The supported values are X, Y, and Z." (3+ items in a sentence)
- Fix: Convert to a bulleted list with a lead-in sentence
- Exception: Short enumerations of 2 items are fine inline ("supports X and Y")

#### Graphics and diagrams

Verify that complex procedures and architectural concepts have supporting diagrams:

- Architecture overviews should have component diagrams
- Monitoring/metrics topics should have dashboard screenshots
- Multi-step workflows with branching logic should have flow diagrams
- Simple configuration procedures do not need diagrams

#### Scoring

| Score | Criteria |
|-------|----------|
| **4** | 0 prose sentences > 30 words, overall avg <= 22 words/sentence, no paragraphs > 4 sentences, lists used for enumerations, graphics where needed |
| **3** | 1-5 sentences > 30 words (borderline cases like 31-33 words), avg <= 22, minor paragraph length issues |
| **2** | Multiple sentences > 30 words, avg > 22 in several files, long paragraphs common |
| **1** | Scannability not assessed or widespread issues |

### Q2: Clearly written

#### Core criteria

- Content is understandable on first read
- Technical terms are defined or linked at first use
- Cause-and-effect relationships are explicit
- Transitions between topics are logical

#### Minimalism principles

Minimalism focuses documentation on readers' needs through five principles:

1. **Customer focus and action orientation** — Know what users do and why. Minimize content between the user and real work. Separate conceptual information from procedural tasks.
2. **Findability** — Content is findable through search and scannable (short paragraphs, sentences, bulleted lists). See Q1 for detailed scannability checks.
3. **Titles and headings** — Clear titles with familiar keywords. Keep titles between 3-11 words. Too short = lacks clarity. Too long = poor search visibility. See title length checks below.
4. **Elimination of fluff** — No long introductions or unnecessary context. See Q5 for detailed fluff checks.
5. **Error recovery, verification, troubleshooting** — Procedures include verification steps and troubleshooting where appropriate.

#### Title length checks

For each file in `topics/` and `assemblies/`, check the main title (`= `) and subsection headings (`== `):

| Metric | Target | Flag |
|--------|--------|------|
| Word count | 3-11 words (resolved) | Flag titles under 3 words or over 11 words |
| Character count | 50-80 characters (resolved) | Titles under 50 chars acceptable if clear. Flag titles over 80 chars |

**Attribute resolution for word counting:**
- `{prod-short}` = 3 words, `{prod}` = 5 words, `{ocp}` = 3 words
- `{orch-name}` = 1 word, `{devworkspace}` = 2 words
- Backtick-quoted strings = 1 word each

**When fixing long titles:** Use shorter attribute forms (`{prod-short}` instead of `{prod}`, `{orch-name}` instead of `{ocp}`) to reduce word/character count while preserving meaning.

**Acceptable exceptions:** Single Kubernetes resource names as subsection headings in reference/concept files (e.g., `== DevWorkspaceTemplate`) are acceptable when the parent section provides context. Two-word titles like "Server components" or "Creating workspaces" are acceptable if clear and descriptive.

#### User pronoun rules

- **Animate users** (persons, human accounts): use "who" — "users who want to configure..."
- **Inanimate users** (system accounts, Linux users, SELinux users): use "that" — "a Linux user that has restricted access..."
- **Ambiguous relative clauses**: When "that" could refer to either an inanimate object or an animate user, restructure the sentence to eliminate ambiguity

#### Verification section coverage

- Procedures with observable outcomes should have `.Verification` sections
- Simple procedures with self-evident outcomes (UI navigation, single configuration edits) can omit verification
- No procedure should have verification steps as the last step inside `.Procedure` — use a separate `.Verification` section

#### Scoring

| Score | Criteria |
|-------|----------|
| **4** | Content understandable on first read, all minimalism principles applied, titles 3-11 words, correct pronoun usage, verification sections where meaningful |
| **3** | Minor clarity issues (1-3 ambiguous sentences), a few titles outside range, minor pronoun issues |
| **2** | Multiple clarity issues, minimalism principles not consistently applied, many short/long titles |
| **1** | Content frequently unclear, minimalism not applied |

### Q3: Simple words

Flag and replace:

- "utilize" -> "use"
- "leverage" -> "use"
- "in order to" -> "to"
- "prior to" -> "before"
- "subsequent to" -> "after"
- "commence" -> "start" or "begin"
- "terminate" -> "stop" or "end"
- "facilitate" -> "help" or "enable"
- "aforementioned" -> name the thing directly
- "in the event that" -> "if"

### Q4: Readability

- Target 11-12th grade reading level
- Measured by average words per sentence and syllables per word
- Technical terms (product names, Kubernetes resources) are acceptable complexity
- Assess overall flow, not individual jargon-heavy sentences

### Q5: Fluff

Flag and rewrite:

- "This section describes..."
- "This section provides..."
- "This topic covers..."
- "as mentioned above/below"
- "Learn how to..."
- "In this chapter..."
- "The following describes..."
- "It is important to note that..." -> state the fact directly
- "Please note that..." -> remove entirely or state directly

### Q18: Style guide compliance

- Follows Red Hat supplementary style guide
- Follows IBM Style guide (primary authority)
- See CLAUDE.md for specific style rules

### Q20: Tone

- Professional, 2nd person ("you") for procedures
- No first person in body text ("I", "we")
- No contractions ("can't" -> "cannot", "isn't" -> "is not")
- No informal language ("basically", "just", "simply")

## Scoring

See [scoring-guide.md](../../references/scoring-guide.md).
