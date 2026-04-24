# /wiki-review

Surface a wiki page for review. Supports three modes: random (default), spaced-repetition (least recently updated), or flashcard (generate questions from a page).

**Argument (optional):**
- None → random page
- `--stale` → page with the oldest `updated` date (spaced repetition)
- `--flash` → pick a page and generate flashcard questions from it
- `[[page-name]]` → review a specific page

---

## Constants

- Vault: `/Users/tom.savard/Library/CloudStorage/GoogleDrive-Tom.savard@orange.fr/Mon Drive/obsidian_vault/obsidian_vault`
- Wiki dir: `{vault}/wiki/`
- Index: `{vault}/wiki/index.md`

---

## Step 1 — Select page

**No argument (random):**
- Read `{vault}/wiki/index.md` to get the list of all pages
- Exclude `index.md` and `vault-principles.md`
- Pick one at random

**`--stale`:**
- Read all wiki pages, extract their `updated` frontmatter date
- Pick the page with the oldest date

**`--flash`:**
- Pick a page at random (same as no argument)
- Proceed to flashcard mode in Step 3

**`[[page-name]]`:**
- Use the named page directly

---

## Step 2 — Present the page for review

Display:
- The page title and one-paragraph summary
- Key claims (bullet list, extracted from the page body)
- Links to related pages (`## See also` section)
- Metadata: created date, updated date, source count

Then ask: *"Anything to update, expand, or connect to something new you've learned?"*

If the user wants to edit: make the changes surgically, update the `updated` date in frontmatter, update index if needed, then ask for approval before saving.

---

## Step 3 — Flashcard mode (only if `--flash`)

Generate 3–5 questions from the page content. Format:

```
Q1: [question]
→ (reveal) [answer drawn from the page]

Q2: ...
```

Questions should test understanding, not recall of exact wording. Prefer questions like:
- *"What is the key difference between X and Y?"*
- *"Why does [claim] matter?"*
- *"What would you expect to happen if [condition]?"*

After presenting questions, ask: *"Want to go through them one by one, or see all answers?"*
