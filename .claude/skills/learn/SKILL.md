---
name: learn
description: Synthesize the current conversation into a learning sheet, file it in the right topic folder following the vault's rules, and update wiki-link back-references. Use when the user invokes /learn or says "create a learning sheet" / "save this as a note" / equivalent.
---

# /learn — Create a learning sheet from this conversation

The full rules live in `CLAUDE.md` at the vault root. **Read it first** — it defines folder structure, naming, linking discipline, and tone. This skill is the execution procedure.

## Procedure

### 1. Load the rules

- Read `CLAUDE.md` (vault root) — languages/folder/naming/linking/tone rules.
- Read `_templates/learning-sheet.en.md` and `_templates/learning-sheet.fr.md` — structural skeletons.

### 2. Determine subject, title, slug

- Identify the dominant subject of the conversation. If multiple distinct subjects came up, ask the user which one(s) to capture — one sheet per subject.
- Title: human-readable (Title Case OK), one per supported language.
- Slug: lowercase `kebab-case`, derived from the **English** title. It names both the note folder and the file stems: `<slug>/<slug>.en.md`, `<slug>/<slug>.fr.md`.

### 3. Decide where to file it

Follow the decision procedure in `CLAUDE.md` ("Where to file a new note"):

1. `find learning-vault -type d -not -path '*/\.*' -not -path '*/_*'` → list existing topic folders.
2. Match the subject to an existing folder if possible.
3. Otherwise pick a durable, broad new top-level folder.
4. Create the note folder `<topic>/<slug>/` inside it.
5. Never ask the user for path — decide.

### 4. Find related notes for wiki-links

- `grep -ril "<key-term>" learning-vault/` for each key concept in the conversation (skip `_templates/`, `_index/`, `.claude/`, `.git/`).
- Collect the list of related note **slugs**. Each one exists in every supported language: link `<slug>.en` from the English sheet, `<slug>.fr` from the French one.

### 5. Write the new sheets — one per supported language

Write the **English** sheet first (`<slug>/<slug>.en.md`), then the **French** one (`<slug>/<slug>.fr.md`) as a translation of that same synthesis — not a separately written note. Same structure, same sections, same figures, same LaTeX.

Use the matching template. Fill every section with synthesized content (not chat transcript). Frontmatter:

- `title`: from step 2, in the file's own language
- `created`: today's date in `YYYY-MM-DD`, identical across languages
- `tags`: 2–5 lowercase hyphenated tags, in the file's own language
- `source`: `conversation with Claude` (override if the user cited specific external sources)
- `lang`: `en` or `fr`, matching the filename suffix
- `translations`: `[[<slug>.<other-lang>]]` for every other supported language
- `related`: wiki-links from step 4, in this file's language

Mirror `related` frontmatter into the `## Related notes` body section as visible `[[wiki-links]]`. Section headings stay in English in both languages.

**Formulas**: write all math in LaTeX (`$...$` inline, `$$...$$` block) — never in code blocks or backticks. See "Math & formulas" in `CLAUDE.md`. Code blocks are reserved for ASCII diagrams, filenames, and code.

### 6. Back-link bidirectionally, in every language

For each note linked in step 4, and for **each of its language files**:

- Read that file's frontmatter and `## Related notes` section.
- Add the new note as a `[[wiki-link]]` in both places, using the **same language** as the file being edited (idempotently — don't duplicate).

### 7. Report

Tell the user:

- The new files' paths (one per language)
- Which existing notes were back-linked
- Any subjects from the conversation that were **not** captured (so they can ask for a separate sheet)

## What this skill does NOT do

- Does not commit to git (user controls when to commit).
- Does not generate MOCs / index pages.
- Does not ask the user for filename or folder — decide based on rules.
- Does not add Obsidian callouts or Dataview blocks.
- Does not put formulas in code blocks/backticks — math goes in LaTeX.

## Edge cases

- **Empty vault (no existing notes)**: skip steps 4 and 6; `related: []` is fine.
- **Conversation held in French (or any other language)**: irrelevant to the output — both sheets are written regardless. The conversation's language never restricts which files are produced.
- **Conversation covered multiple subjects**: ask the user which to save, then create one sheet per chosen subject.
- **Subject perfectly matches an existing note**: ask the user whether to *update* the existing note or create a new one. Default to updating if the existing note is short; create new if the existing one is already substantial.
- **No clear subject** (e.g. small talk, debugging session): say so and don't create a sheet.
