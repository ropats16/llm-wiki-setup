# {{WIKI_NAME}} Schema

This document defines all conventions for the {{WIKI_NAME}} wiki. The LLM agent must read this file before any wiki operation.

## Architecture

Two layers, following Karpathy's LLM Knowledge Base pattern:

1. **Raw Sources** (`raw/`) — Immutable source material. Never modified after creation. Any format: markdown, PDFs, images, data files. The LLM reads from these but never changes them. This is your source of truth.
2. **The Wiki** (`wiki/`) — LLM-maintained compiled knowledge. Interlinked markdown articles synthesized from raw sources.

All metadata about raw sources lives in `raw/index.md`, not in the files themselves. Raw means raw.

## File Conventions

### Slug Naming
All wiki page filenames use lowercase, hyphenated slugs:
- `three-layer-architecture.md` (not `ThreeLayerArchitecture.md`)
- `andrej-karpathy.md` (not `Andrej_Karpathy.md`)

### Article Frontmatter
Every wiki page in `wiki/pages/` must have this YAML frontmatter:

```yaml
---
title: Human-Readable Title
created: YYYY-MM-DD
updated: YYYY-MM-DD
category: category-slug
sources: [raw-source-slug-1, raw-source-slug-2]
---
```

### Categories

Categories are not predefined — they emerge as sources are ingested. When creating a new article, assign it to an existing category or create a new one if nothing fits. Each category needs a slug and one-line description.

Categories are tracked as headings in `wiki/index.md`. The first article in a new category creates the heading.

## Backlinks

Use wiki-style double-bracket syntax: `[[slug]]`

Example: "The [[three-layer-architecture]] separates raw sources from compiled knowledge."

**Rules:**
- Backlinks must be bidirectional. If page A links to page B, page B should link back to page A.
- Use the slug (filename without `.md`), not the title.

## Index Files

### wiki/index.md
Master catalog of all wiki articles. One entry per article:
```
- [Title](pages/slug.md) — One-line summary `[category]`
```
Organized under category headings. Updated after every article creation or deletion.

### raw/index.md
Catalog of all raw sources as a markdown table:
```
| Slug | Title | Author | Date | Type | URL |
```
This is the sole metadata layer for raw sources. Updated after every source ingestion.

### wiki/log.md
Append-only operation log:
```
| Date | Operation | Files Touched | Notes |
```
Every wiki operation (ingest, query-filed, lint-fix, update) gets a log entry.

## Operations

### Ingest
When adding a new source:
1. Determine source type and fetch content (URL → WebFetch; pasted text → save directly; local file → already in place)
2. Save to `raw/` — any format is fine, raw means raw
3. Update `raw/index.md` with metadata row
4. Create or update wiki articles in `wiki/pages/` with new knowledge
5. Add bidirectional `[[backlinks]]` to related articles
6. Update `wiki/index.md`
7. Append to `wiki/log.md`

### Query
When answering questions from the wiki:
1. Read `wiki/index.md` to find relevant articles
2. Read the relevant `wiki/pages/*.md` files
3. Synthesize answer from wiki content, not training data
4. Cite wiki pages in the answer
5. Optionally file the answer as a new wiki page (if novel insight)

### Scout
When discovering new sources to potentially ingest:
1. Read `wiki/index.md` and `raw/index.md` to understand current state
2. Identify search focus (user-specified topic, gaps, or URL evaluation)
3. WebSearch for candidate tier-1 sources
4. Evaluate and rank candidates (tier-1 check, relevance, overlap)
5. Present candidates to user — never auto-ingest
6. Append scout operation to `wiki/log.md`

### Lint
Health checks over the wiki:
- Broken `[[backlinks]]` — links to non-existent pages
- Orphan pages — pages not listed in `wiki/index.md`
- Missing backlinks — A links to B but B doesn't link to A
- Stale information — contradictions between articles
- Knowledge gaps — topics mentioned but without dedicated articles

## Source Fetching

- **URLs** (articles, blogs, gists): WebFetch or `gh` CLI
- **Pasted text**: User pastes content directly (tweets, excerpts, notes) — save as markdown to `raw/`
- **Local files**: User drops files into `raw/` directly (images, PDFs, docs)
- **Media in wiki articles**: Reference raw files as `![alt](../raw/filename.jpg)`. Never download videos — link or embed instead.

## Article Template

```markdown
---
title: Article Title
created: YYYY-MM-DD
updated: YYYY-MM-DD
category: category-name
sources: [source-slug-1]
---

# Article Title

One-paragraph summary of the topic.

## Section Heading

Article content with [[backlinks]] to related articles.

## Related Articles

- [[related-slug-1]]
- [[related-slug-2]]
```
