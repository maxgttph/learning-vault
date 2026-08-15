# Learning Vault — Operating Instructions

This repo is a personal **learning vault** — an Obsidian-compatible Markdown knowledge base. These instructions apply to any LLM (Claude Code, other tools) editing notes here.

The primary workflow:

1. The user holds a conversation about a topic.
2. The user invokes `/learn` (or says "create a learning sheet" / equivalent).
3. The LLM synthesizes the conversation into a new note using the rules below.

## Languages

The vault is **bilingual**. Supported languages: **`en`** (English) and **`fr`** (French).

- **One file per supported language, per note.** A note is not finished until every supported language exists.
- **English is canonical for naming**: the note folder and the file stem always use the English slug, whatever the language of the file.
- **Write the English sheet first**, then the French one as a translation of the *same synthesis* — not a second, independently written note. The two must stay equivalent in content and structure.
- **Section headings stay in English in both languages** (`## TL;DR`, `## Key concepts`, `## Deep dive`, …). Only the prose, the `title`, the `tags`, the `aliases` and the free text of `source` are translated.
- The conversation's own language does **not** decide the sheet's language: both are always produced.

## Folder structure

```
learning-vault/
├── CLAUDE.md                    # this file
├── README.md                    # human-facing intro
├── _templates/
│   ├── learning-sheet.en.md
│   └── learning-sheet.fr.md
├── _index/                      # reserved for future index/MOC files
└── <topic>/                     # topic folders, created lazily
    └── <sub-topic>/             # optional, only if a topic accumulates >~5 notes
        └── <slug>/              # one folder per note, named with the English slug
            ├── <slug>.en.md
            └── <slug>.fr.md
```

- Topic folders are **created lazily** — never pre-seed empty categories.
- Folders prefixed with `_` (e.g. `_templates`, `_index`) are vault infrastructure, not knowledge notes. Never file a learning sheet inside them.

## Naming conventions

- **Folders**: lowercase `kebab-case` (e.g. `cognitive-science/`, `ancient-history/`)
- **Note folder**: lowercase `kebab-case`, derived from the **English** title (e.g. `solar-eclipses/`, `ship-of-theseus/`)
- **Files**: `<slug>.<lang>.md`, repeating the note folder's slug (e.g. `solar-eclipses/solar-eclipses.en.md`). Never `en.md` alone — Obsidian could not resolve short wiki-links between dozens of identically named files.
- **No dates in filenames** — the date lives in frontmatter (`created:`)
- **Nesting**: max 2 levels of *topic* folders above the note folder (`topic/sub-topic/<slug>/<slug>.en.md`). If you'd need a 3rd topic level, reconsider the taxonomy first.

## Where to file a new note

Decision procedure (do this in order, don't ask the user):

1. **Search first.** Run `find learning-vault -type d` to list existing topic folders. Also `grep -ri` for the subject across existing notes.
2. **Reuse if it fits.** If an existing folder is a clear match for the topic, file the note there.
3. **Create a sub-folder** only when the parent topic already holds several notes that cluster naturally (e.g. `philosophy/` has 6 notes, 3 about ethics → create `philosophy/ethics/`).
4. **Create a new top-level folder** only when the topic genuinely doesn't belong anywhere existing. Prefer broad, durable names (`science`, not `quantum-stuff`).
5. **Create the note folder** inside it, named with the English slug, and write one file per supported language in it.
6. **Never** ask the user for the folder or filename. Decide based on the rules and tell them where you filed it.

## Writing the note

Use `_templates/learning-sheet.<lang>.md` as the structural skeleton. Fill it in by **synthesizing** the conversation — don't transcribe it. Specifically:

- **Frontmatter**:
  - `title`: human-readable title, in the file's own language (Title Case OK here)
  - `tags`: free-form, lowercase, hyphenated, in the file's own language (e.g. `tags: [philosophy, ethics, thought-experiment]`)
  - `created`: today's ISO date (`YYYY-MM-DD`) — the **same** date in every language of the note
  - `source`: `conversation with Claude` by default; override if the conversation cited specific books, papers, etc. — list them
  - `lang`: `en` or `fr`, matching the file's suffix
  - `translations`: list of `[[wiki-links]]` to the same note in the other supported languages
  - `related`: list of `[[wiki-links]]` to other notes in the vault, **in the same language as this file**
- **TL;DR**: 1–2 sentences. Should stand on its own.
- **Key concepts**: bullets, each a term and a one-line definition.
- **Deep dive**: prose, organized by sub-heading. Keep it durable — written for the user reading it in six months, not as a chat recap.
- **Examples & analogies**: concrete grounding. At least one if possible.
- **Open questions**: things worth a future sheet or further reading.
- **Related notes**: mirror the frontmatter `related` list as visible `[[wiki-links]]`.

## Math & formulas

**These rules apply ONLY to Obsidian files (vault notes).** They do NOT apply to chat replies — see the last bullet.

**In Obsidian files, always write mathematical formulas in LaTeX** — Obsidian renders MathJax natively. Never put equations in code blocks (``` ```) or inline backticks: those stay as unrendered monospace text and are hard to read.

- **Inline math**: single dollars — `$d_{\text{model}} \times d_k$`, `$O(n^2)$`, `$x + f(x)$`.
- **Block math** (centered, own line): double dollars — `$$\text{Attention}(Q,K,V) = \text{softmax}\!\left(\frac{Q K^\top}{\sqrt{d_k}}\right) V$$`.
- This covers **any** mathematical notation, including subscripts/superscripts (`$d_{\text{model}}$`, `$10^{13}$`), complexity (`$O(n^2)$`), Greek letters (`$\eta$`, `$\theta$`), operators (`$\cdot$`, `$\times$`, `$\odot$`, `$\top$`), and single variables in a math context (`$Q$`, `$W_Q$`, `$E^\top$`).
- Use `\text{...}` for function names and labels inside math (`$\text{softmax}$`, `$\text{ReLU}$`).
- **Keep in code blocks only what is NOT a formula**: ASCII flow diagrams (pipelines, layer anatomy), filenames (`.safetensors`), and code identifiers. Monospace is correct there.
- **LaTeX is for notes only, not for chat.** Use LaTeX (`$...$` / `$$...$$`) **only when writing a vault note**. In normal conversation with the user, write math in plain readable notation instead (e.g. `d_model × d_k`, `O(n²)`, `η`) — unrendered LaTeX source is hard to read in the terminal.

## Linking discipline

Wiki-links build the knowledge graph over time. Before saving a new note:

1. `grep -r "topic-keyword" learning-vault/` for related concepts.
2. For every existing note that's genuinely related, add a `[[wiki-link]]` in the new note's `related` section **and** frontmatter.
3. **Back-link**: in each referenced existing note, append the new note to its `related:` frontmatter list and its `## Related notes` section. Bidirectional linking matters — without it the graph stays sparse.
4. Don't fabricate links. If no existing note is related, leave `related: []`.

Wiki-link format: `[[<slug>.<lang>]]` — the filename without its `.md` extension (Obsidian's default short-form), e.g. `[[solar-eclipses.en]]`.

**Links stay inside their own language.** An `.en.md` file links only to `.en` targets, an `.fr.md` file only to `.fr` targets. The single exception is the `translations:` frontmatter key, which is the only bridge between languages. Consequence: back-linking must be done **in every language** — adding a link in the English sheet means adding the mirrored link in the French one too, and updating both languages of every referenced note.

## Tone & content rules

- **Synthesize, don't transcribe.** A learning sheet is the durable extract of a conversation, not its transcript.
- **No chat artifacts.** No "as we discussed", "you asked about", "I explained". Write impersonally.
- **Clarity over comprehensiveness.** Better to nail the core idea than dump every tangent.
- **No filler.** Skip empty sections — if there are no good examples, delete the section rather than write "TBD".

## What not to do

- Don't ask the user for the filename or folder — decide.
- Don't leave a note in a single language — every supported language gets its file.
- Don't link across languages (outside `translations:`), and don't name a language file `en.md` / `fr.md`.
- Don't auto-generate MOCs, dashboards, or index pages (out of current scope).
- Don't add Obsidian callouts (`> [!note]`) — out of scope.
- Don't add Dataview queries — out of scope.
- Don't write formulas in code blocks or backticks — use LaTeX (`$...$` / `$$...$$`). See "Math & formulas".
- Don't commit changes unless the user asks.
- Don't write notes inside `_templates/` or `_index/`.
