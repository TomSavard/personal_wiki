# Personal Wiki — Operating Schema

This is the bookkeeper's contract. Follow it exactly. Deviate only when explicitly instructed by the user, and document the deviation.

The wiki is a compounding artifact. Its value comes from consistency and durability over time. When in doubt: do less, ask more.

---

## Roles

**User** — sources, direction, final review. Approves all changes before commit.
**LLM** — reads sources, writes and maintains wiki pages, updates index, proposes commits. Never commits autonomously.

---

## Directory structure

```
personal_wiki/
  raw/            # immutable source documents — never modified
    assets/       # images and attachments
  wiki/           # LLM-maintained knowledge pages
  index.md        # catalog of all wiki pages
  CLAUDE.md       # this file
  llm_wiki.md     # backbone idea reference
```

**Rule:** The LLM writes only to `wiki/` and `index.md`. Everything in `raw/` is read-only.

---

## Page format

Every wiki page starts with this frontmatter:

```yaml
---
title: <page title>
type: concept | entity | source-summary | synthesis
tags: []
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: []
language: en | fr
---
```

- `type` must be one of the four defined values — no new types without user approval
- `tags` should reuse existing tags; propose new ones explicitly before using them
- `sources` lists filenames from `raw/` that this page draws from
- `language` reflects the primary language of the page content

After the frontmatter, pages follow this structure:

```markdown
# Title

One-paragraph summary of what this page covers.

## [Sections as needed]

## See also

- [[linked-page]]
```

**Rule:** Never delete content from a page without surfacing the deletion to the user first. Flag contradictions with `> ⚠️ Contradiction with [[other-page]]:` rather than silently overwriting.

---

## Operations

### Ingest

Triggered when the user drops a source into `raw/` and says "ingest [filename]".

1. Read the source
2. Discuss key takeaways with the user — do not proceed to writing until the user confirms direction
3. Write or update a source-summary page in `wiki/`
4. Update or create relevant concept and entity pages
5. Update `index.md` (add new pages, update page summaries if changed)
6. Show the user a diff summary: list every file created or modified
7. Wait for user approval before anything is committed

Commit message format: `ingest: <source title>`

### Query

Triggered when the user asks a question.

1. Read `index.md` to find relevant pages
2. Read those pages
3. Synthesize an answer with citations (`[[page-name]]`)
4. If the answer is valuable enough to persist, propose filing it as a synthesis page — user decides

### Lint

Triggered when the user says "lint".

Check for:
- Pages with no inbound links (orphans)
- Contradictions between pages
- Concepts mentioned across multiple pages but lacking their own page
- Stale claims that newer sources may have superseded
- Missing `See also` links that should exist

Report findings as a list. Propose fixes. Do not apply them until the user approves.

---

## Git discipline

- One commit per operation (one ingest, one query filing, one lint pass)
- Never commit without user approval
- Commit messages: `ingest: <title>` | `query: <topic>` | `lint: <date>` | `chore: <description>`
- Git is the audit trail — do not duplicate it with a log file

---

## Non-deterioration rules

These protect the wiki from quality decay over time:

1. **Never rewrite a page from scratch** unless the user explicitly asks. Prefer surgical updates.
2. **Never remove cross-references** without replacing them or flagging the removal.
3. **Flag contradictions, don't resolve them silently.** Surface the conflict; let the user decide.
4. **Preserve the user's own writing verbatim** unless asked to edit it.
5. **When uncertain about where something belongs**, ask rather than guess.
