# Learning Vault

A personal, Obsidian-compatible Markdown knowledge base. Conversations with an LLM get distilled into durable notes that link together over time.

## How it works

1. **Have a conversation** with Claude Code about something you want to learn.
2. **Run `/learn`** — Claude reads the vault's rules, synthesizes the conversation into a learning sheet, files it in the right topic folder, and wires up wiki-links to related notes.
3. **Open the vault in Obsidian** to browse, search, and see the knowledge graph.

The rules that govern note structure, naming, folder placement, and linking all live in [`CLAUDE.md`](./CLAUDE.md) — that file is the single source of truth, so the workflow stays consistent across sessions and tools.

## Layout

```
learning-vault/
├── CLAUDE.md            # vault rules (read first if editing manually)
├── _templates/
│   └── learning-sheet.md
├── _index/              # reserved for future index pages
└── <topic>/             # topic folders, created as you learn
    └── note.md
```

Topic folders are created lazily — the vault grows organically as you save notes.

## Open in Obsidian

1. Open Obsidian → *Open folder as vault*.
2. Point it at this directory.
3. Frontmatter, `[[wiki-links]]`, tags, and graph view all work out of the box.

## Useful conventions (full details in `CLAUDE.md`)

- **Filenames**: lowercase `kebab-case.md`, no dates (date is in frontmatter).
- **Folders**: lowercase `kebab-case`, max 2 levels deep.
- **Wiki-links**: bidirectional — when note A links to note B, note B should also link back to A.
- **`_`-prefixed folders** (`_templates`, `_index`) are infrastructure, not notes.

## The `/learn` skill

Defined at [`.claude/skills/learn/SKILL.md`](.claude/skills/learn/SKILL.md). Invoke it in Claude Code with `/learn` at the end of a conversation. It reads `CLAUDE.md`, picks the filename and folder, finds related existing notes, writes the new sheet, and back-links the related notes.
