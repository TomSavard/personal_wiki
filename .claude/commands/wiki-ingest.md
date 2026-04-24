# /wiki-ingest

Clean and integrate a **user-written draft from `raw/`** into the wiki. The draft is Tom's own synthesis. The bookkeeper does not generate knowledge — only structures, links to existing pages, and proposes (never adds) potential enrichments. After the commit, the raw note is deleted since it's now represented in `wiki/`.

**Argument:** filename or path relative to `raw/` (e.g. `Transformer.md` or `art/peinture/Kupka.md`).

---

## Constants

- Vault: `/Users/tom.savard/Library/CloudStorage/GoogleDrive-Tom.savard@orange.fr/Mon Drive/obsidian_vault/obsidian_vault`
- Raw dir: `{vault}/raw/`
- Wiki dir: `{vault}/wiki/`
- Index: `{vault}/wiki/index.md`
- Principles: `{vault}/wiki/vault-principles.md`

---

## Philosophy (read before acting)

1. The user's draft is the authoritative content. Do not rewrite its claims.
2. Do not add ideas from general knowledge.
3. Structure, templating, and links to existing pages are in scope.
4. Any enrichment (implicit idea, missing link, related entity) must be proposed as a discrete suggestion awaiting the user's yes/no.

---

## Step 1 — Read the draft

Resolve the argument relative to `{vault}/raw/`. Read the draft. If the file does not exist, stop and output `✗ File not found: {path}`.

The draft is Tom's own notes — treat it as authoritative. Do not second-guess its claims.

---

## Step 2 — Identify target wiki page(s)

- Is the draft about one concept/entity → one wiki page
- Does it cover multiple distinct ideas → propose splitting, ask the user

Check `{vault}/wiki/index.md`: does a page already exist? If yes, we update; if no, we create.

---

## Step 3 — Apply the template

Wrap the draft with frontmatter from `CLAUDE.md`:

```yaml
---
title: <title>
type: concept | entity | source-summary | synthesis
status: stub | draft | complete
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources:
  - <path to raw source, if any>
language: en | fr
---
```

Normalize the filename (kebab-case, lowercase, no accents).

---

## Step 4 — Wrap explicit matches with links

Scan the draft for words/phrases that match existing wiki page titles. Wrap them in `[[...]]`.

This is mechanical — only exact or near-exact matches. No interpretation.

---

## Step 5 — Propose enrichments as a numbered list

Present suggestions for the user to validate. Each item:
- Numbered
- One sentence explaining the suggestion
- Explicit Y/N expected

Examples:

```
1. The draft mentions "Cézanne" — link to existing [[paul-cezanne]]? (Y/N)
2. The draft talks about "impression of light" — this echoes [[impressionnisme]]'s stated focus. Add cross-link? (Y/N)
3. "Louise" is mentioned as having visited an exhibition — create an entity page [[louise]]? (Y/N)
4. The draft describes a painting style similar to [[art-abstrait]]. Add `See also` link? (Y/N)
```

**Wait for the user's responses.** Apply only what they approve.

---

## Step 6 — Update the index

Add or update the relevant lines in `{vault}/wiki/index.md`. Update the `_Last updated_` and `_Total pages_` metadata.

---

## Step 7 — Show diff summary

```
Created:
  wiki/<new page>.md
Modified:
  wiki/index.md
  wiki/<existing page>.md
```

Ask: *"Ready to commit? Any last changes?"*

---

## Step 8 — Commit and delete raw note

Only after explicit user approval:
1. Commit with message `ingest: <page title>` (vault git repo if initialized; otherwise skip and inform)
2. Delete the source file from `{vault}/raw/` — it is now represented in `wiki/` and the raw inbox should shrink as ingestion progresses
3. Confirm with the user: *"Ingested and deleted from raw/. Raw inbox: N remaining."*
