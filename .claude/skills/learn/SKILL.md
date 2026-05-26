---
name: learn
description: Synthesize the current conversation into a learning sheet, file it in the right topic folder following the vault's rules, and update wiki-link back-references. Use when the user invokes /learn or says "create a learning sheet" / "save this as a note" / equivalent.
---

# /learn — Create a learning sheet from this conversation

The full rules live in `CLAUDE.md` at the vault root. **Read it first** — it defines folder structure, naming, linking discipline, and tone. This skill is the execution procedure.

## Procedure

### 1. Load the rules

- Read `CLAUDE.md` (vault root) — folder/naming/linking/tone rules.
- Read `_templates/learning-sheet.md` — structural skeleton.

### 2. Determine subject, title, filename

- Identify the dominant subject of the conversation. If multiple distinct subjects came up, ask the user which one(s) to capture — one sheet per subject.
- Title: human-readable (Title Case OK).
- Filename: lowercase `kebab-case.md`, derived from the title.

### 3. Decide where to file it

Follow the decision procedure in `CLAUDE.md` ("Where to file a new note"):

1. `find learning-vault -type d -not -path '*/\.*' -not -path '*/_*'` → list existing topic folders.
2. Match the subject to an existing folder if possible.
3. Otherwise pick a durable, broad new top-level folder.
4. Never ask the user for path — decide.

### 4. Find related notes for wiki-links

- `grep -ril "<key-term>" learning-vault/` for each key concept in the conversation (skip `_templates/`, `_index/`, `.claude/`, `.git/`).
- Collect a list of related note filenames (without `.md`).

### 5. Write the new sheet

Use the template structure. Fill every section with synthesized content (not chat transcript). Frontmatter:

- `title`: from step 2
- `created`: today's date in `YYYY-MM-DD`
- `tags`: 2–5 lowercase hyphenated tags
- `source`: `conversation with Claude` (override if the user cited specific external sources)
- `related`: wiki-links from step 4

Mirror `related` frontmatter into the `## Related notes` body section as visible `[[wiki-links]]`.

### 6. Back-link bidirectionally

For each note linked in step 4:

- Read that note's frontmatter and `## Related notes` section.
- Add the new note as a `[[wiki-link]]` in both places (idempotently — don't duplicate).

### 7. Report

Tell the user:

- The new file's path
- Which existing notes were back-linked
- Any subjects from the conversation that were **not** captured (so they can ask for a separate sheet)

## What this skill does NOT do

- Does not commit to git (user controls when to commit).
- Does not generate MOCs / index pages.
- Does not ask the user for filename or folder — decide based on rules.
- Does not add Obsidian callouts or Dataview blocks.

## Edge cases

- **Empty vault (no existing notes)**: skip steps 4 and 6; `related: []` is fine.
- **Conversation covered multiple subjects**: ask the user which to save, then create one sheet per chosen subject.
- **Subject perfectly matches an existing note**: ask the user whether to *update* the existing note or create a new one. Default to updating if the existing note is short; create new if the existing one is already substantial.
- **No clear subject** (e.g. small talk, debugging session): say so and don't create a sheet.
