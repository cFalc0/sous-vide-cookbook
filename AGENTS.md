# AGENTS.md - sous_vide_wiki

## Wiki Structure (Karpathy Pattern)

This is a personal knowledge base using the 3-layer wiki pattern:
- `raw/` - immutable source documents (articles, notes, transcripts)
- `wiki/` - LLM-compiled markdown (LLM owns this, never hand-edit)
- `CLAUDE.md` / `AGENTS.md` - schema with conventions

## Directory Layout

```
sous_vide_wiki/
├── raw/                    # source documents (never edited by LLM)
│   ├── articles/
│   ├── notes/
│   └── transcripts/
├── wiki/                   # LLM-owned compiled knowledge
│   ├── index.md            # content catalog
│   ├── log.md              # chronological record
│   ├── concepts/           # one file per concept
│   ├── entities/           # people, products, places
│   └── sources/            # one summary per ingested source
├── .obsidian/              # Obsidian vault config (parent dir)
└── AGENTS.md               # this file
```

## Core Rules

1. **Never hand-edit wiki/ files** - the next ingest will silently overwrite your changes
2. **To fix something, fix the raw source or add instruction to AGENTS.md**
3. **raw/ is append-only** - add new sources, never modify existing ones
4. **One concept = one file** in wiki/concepts/ or wiki/entities/

## Page Format

Every wiki page starts with:
```markdown
# Title

2-sentence summary covering what this page is about.

## Tags
- tag1
- tag2

## Summary
Key information distilled from sources...

## Details
Full body with cross-references...

## Sources
- [[source-name]]
```

## Wikilinks

Use `[[page-name]]` for cross-references. The LLM must resolve every link to an existing file.

## Index Maintenance

- `wiki/index.md` lists every page with a one-line summary
- **Update index.md on EVERY ingest or page creation**
- Keep alphabetical within categories

## Log Format

`wiki/log.md` is append-only, newest first:
```markdown
## [2026-05-04] ingest | Source Title

- Created wiki/concepts/new-concept.md
- Updated wiki/concepts/existing-concept.md
- Added to index.md
```

## Ingest Workflow

1. Add new source file to `raw/`
2. LLM reads source end-to-end
3. Identifies key concepts, entities, claims
4. Creates/updates relevant wiki pages
5. Adds cross-references
6. Updates index.md
7. Appends to log.md

## Query Workflow

1. Read `wiki/index.md` first to find relevant pages
2. Open those pages
3. Synthesize answer from compiled wiki (not raw/)
4. Cite sources in answer

## Lint Checklist

Before finishing any session, verify:
- [ ] Every page in wiki/ appears in index.md
- [ ] Every [[wikilink]] resolves to a real file
- [ ] No duplicate concepts across pages
- [ ] Conflicting claims are flagged, not silently overwritten