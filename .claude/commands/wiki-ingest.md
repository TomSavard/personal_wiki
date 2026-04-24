# /wiki-ingest

Process a source from `raw/` into the wiki. Creates a source-summary page, updates relevant concept and entity pages, and updates the index. The user reviews and approves before anything is considered done.

**Argument:** filename or path relative to `raw/` (e.g. `wiki-ingest Transformer.md` or `wiki-ingest tech/LoRA.md`)

---

## Constants

- Vault: `/Users/tom.savard/Library/CloudStorage/GoogleDrive-Tom.savard@orange.fr/Mon Drive/obsidian_vault/obsidian_vault`
- Raw dir: `{vault}/raw/`
- Wiki dir: `{vault}/wiki/`
- Index: `{vault}/wiki/index.md`
- Principles: `{vault}/wiki/vault-principles.md`

---

## Step 1 — Resolve source file

- If argument is provided, resolve it relative to `{vault}/raw/`
- If the file does not exist, stop and output: `✗ File not found: {resolved path}`
- Read the file

---

## Step 2 — Discuss with user before writing

Read the source. Then present to the user:
- **3–5 key takeaways** from the source in bullet form
- **Proposed wiki pages to create or update** (source-summary + any concept/entity pages)
- **Proposed tags** (check existing tags in wiki pages before proposing new ones)

Ask: *"Does this look right? Any emphasis to change before I write?"*

Do not proceed to Step 3 until the user confirms.

---

## Step 3 — Write wiki pages

Apply the page format from `CLAUDE.md`:

```yaml
---
title: <title>
type: concept | entity | source-summary | synthesis
tags: []
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: [<raw filename>]
language: en | fr
---
```

Follow the vault principles from `{vault}/wiki/vault-principles.md`:
- One idea per page
- Write in your own words — no copy-paste from the source
- Titles as full phrases readable in 2 years without context
- Link to related wiki pages with `[[page-name]]`
- End every page with a `## See also` section

Pages to write/update:
1. **Source-summary page** — `{vault}/wiki/{slug}.md` — summary of the source, key claims, your synthesis
2. **Concept/entity pages** — one per major concept or entity introduced; update if page already exists (surgical edits only, flag contradictions with `> ⚠️ Contradiction with [[other-page]]:`)

---

## Step 4 — Update index

Add new pages to the appropriate section in `{vault}/wiki/index.md`:
- One line per page: `- [[page-name]] — one-sentence description`
- Update `_Total pages_` count
- Update `_Last updated_` date

---

## Step 5 — Show diff summary and wait for approval

Output a list of every file created or modified:
```
Created:
  wiki/transformer.md
  wiki/attention-mechanism.md
Modified:
  wiki/index.md
```

Then ask: *"Ready to commit? Any changes before we do?"*

Do not commit until the user explicitly says yes.

---

## Step 6 — Commit

Commit message format: `ingest: <source title>`

Use conventional commit via the `doctolib-base:commit` skill if available, otherwise:
```
git -C {vault} add wiki/ && git -C {vault} commit -m "ingest: <title>"
```

Note: the vault is not a git repo by default — skip the commit step and note it if git is not initialized.
