# /wiki-query

Answer a question using the wiki as the knowledge source. Always cite wiki pages. Offer to file valuable answers as synthesis pages.

**Argument:** the question to answer (e.g. `wiki-query What do I know about transformers?`)

---

## Constants

- Vault: `/Users/tom.savard/Library/CloudStorage/GoogleDrive-Tom.savard@orange.fr/Mon Drive/obsidian_vault/obsidian_vault`
- Wiki dir: `{vault}/wiki/`
- Index: `{vault}/wiki/index.md`

---

## Step 1 — Read the index

Read `{vault}/wiki/index.md` to find all pages relevant to the question. List the candidates mentally before reading any of them.

---

## Step 2 — Read relevant pages

Read the relevant wiki pages identified in Step 1. If a page links to other pages that seem relevant, read those too (max 2 levels deep).

---

## Step 3 — Synthesize and answer

Write the answer:
- Ground every claim in a wiki page — cite with `[[page-name]]`
- If the wiki doesn't have enough information to answer well, say so explicitly: *"The wiki doesn't cover this yet. Sources that might help: ..."*
- Do not fill gaps with general LLM knowledge unless clearly labeled as such: *"(not in wiki — general knowledge)"*

---

## Step 4 — Offer to file the answer

If the answer is non-trivial (synthesizes multiple pages, reveals a new connection, or would be useful to revisit), ask:

*"This synthesis seems worth keeping. Should I file it as a wiki page?"*

If the user says yes, create `{vault}/wiki/{slug}.md` with `type: synthesis`, update the index, and ask for approval before committing.
