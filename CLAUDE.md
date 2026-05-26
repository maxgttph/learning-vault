# Learning Vault — Operating Instructions

This repo is a personal **learning vault** — an Obsidian-compatible Markdown knowledge base. These instructions apply to any LLM (Claude Code, other tools) editing notes here.

The primary workflow:

1. The user holds a conversation about a topic.
2. The user invokes `/learn` (or says "create a learning sheet" / equivalent).
3. The LLM synthesizes the conversation into a new note using the rules below.

## Folder structure

```
learning-vault/
├── CLAUDE.md            # this file
├── README.md            # human-facing intro
├── _templates/
│   └── learning-sheet.md
├── _index/              # reserved for future index/MOC files
└── <topic>/             # topic folders, created lazily
    └── <sub-topic>/     # optional, only if a topic accumulates >~5 notes
        └── note.md
```

- Topic folders are **created lazily** — never pre-seed empty categories.
- Folders prefixed with `_` (e.g. `_templates`, `_index`) are vault infrastructure, not knowledge notes. Never file a learning sheet inside them.

## Naming conventions

- **Folders**: lowercase `kebab-case` (e.g. `cognitive-science/`, `ancient-history/`)
- **Files**: lowercase `kebab-case.md` (e.g. `fermi-paradox.md`, `ship-of-theseus.md`)
- **No dates in filenames** — the date lives in frontmatter (`created:`)
- **Nesting**: max 2 levels deep (`topic/sub-topic/note.md`). If you'd need a 3rd level, reconsider the taxonomy first.

## Where to file a new note

Decision procedure (do this in order, don't ask the user):

1. **Search first.** Run `find learning-vault -type d` to list existing topic folders. Also `grep -ri` for the subject across existing notes.
2. **Reuse if it fits.** If an existing folder is a clear match for the topic, file the note there.
3. **Create a sub-folder** only when the parent topic already holds several notes that cluster naturally (e.g. `philosophy/` has 6 notes, 3 about ethics → create `philosophy/ethics/`).
4. **Create a new top-level folder** only when the topic genuinely doesn't belong anywhere existing. Prefer broad, durable names (`science`, not `quantum-stuff`).
5. **Never** ask the user for the folder or filename. Decide based on the rules and tell them where you filed it.

## Writing the note

Use `_templates/learning-sheet.md` as the structural skeleton. Fill it in by **synthesizing** the conversation — don't transcribe it. Specifically:

- **Frontmatter**:
  - `title`: human-readable title (Title Case OK here)
  - `tags`: free-form, lowercase, hyphenated (e.g. `tags: [philosophy, ethics, thought-experiment]`)
  - `created`: today's ISO date (`YYYY-MM-DD`)
  - `source`: `conversation with Claude` by default; override if the conversation cited specific books, papers, etc. — list them
  - `related`: list of `[[wiki-links]]` to other notes in the vault
- **TL;DR**: 1–2 sentences. Should stand on its own.
- **Key concepts**: bullets, each a term and a one-line definition.
- **Deep dive**: prose, organized by sub-heading. Keep it durable — written for the user reading it in six months, not as a chat recap.
- **Examples & analogies**: concrete grounding. At least one if possible.
- **Open questions**: things worth a future sheet or further reading.
- **Related notes**: mirror the frontmatter `related` list as visible `[[wiki-links]]`.

## Linking discipline

Wiki-links build the knowledge graph over time. Before saving a new note:

1. `grep -r "topic-keyword" learning-vault/` for related concepts.
2. For every existing note that's genuinely related, add a `[[wiki-link]]` in the new note's `related` section **and** frontmatter.
3. **Back-link**: in each referenced existing note, append the new note to its `related:` frontmatter list and its `## Related notes` section. Bidirectional linking matters — without it the graph stays sparse.
4. Don't fabricate links. If no existing note is related, leave `related: []`.

Wiki-link format: `[[filename-without-extension]]` (Obsidian's default short-form).

## Tone & content rules

- **Synthesize, don't transcribe.** A learning sheet is the durable extract of a conversation, not its transcript.
- **No chat artifacts.** No "as we discussed", "you asked about", "I explained". Write impersonally.
- **Clarity over comprehensiveness.** Better to nail the core idea than dump every tangent.
- **No filler.** Skip empty sections — if there are no good examples, delete the section rather than write "TBD".

## What not to do

- Don't ask the user for the filename or folder — decide.
- Don't auto-generate MOCs, dashboards, or index pages (out of current scope).
- Don't add Obsidian callouts (`> [!note]`) — out of scope.
- Don't add Dataview queries — out of scope.
- Don't commit changes unless the user asks.
- Don't write notes inside `_templates/` or `_index/`.
