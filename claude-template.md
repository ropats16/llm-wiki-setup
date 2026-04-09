# {{WIKI_NAME}}

An LLM-maintained wiki. Based on Karpathy's LLM Knowledge Base pattern.

## Rules

- Read SCHEMA.md before any wiki operation
- NEVER modify files in `raw/` — they are immutable source documents
- After ANY wiki change: update `wiki/index.md` AND append to `wiki/log.md`
- Maintain `[[backlinks]]` bidirectionally — if A links B, B must link A
- Cite sources using the raw source slug from `raw/index.md`
- When querying: read `wiki/index.md` first, navigate to relevant pages, answer from wiki content
- When ingesting: save source to `raw/`, then compile into `wiki/pages/`
- Only ingest tier-1 sources: original authors, high-trust community figures, or reputable outlets
- When scouting: never auto-ingest, only present candidates for user review

## Source Fetching

- **URLs** (articles, blogs, gists): WebFetch or `gh` CLI
- **Pasted text**: User pastes content directly — save as markdown to `raw/`
- **Local files**: User drops files into `raw/` directly (images, PDFs, docs)
- **Media in wiki articles**: Reference raw files as `![alt](../raw/filename.jpg)`. Never download videos — link or embed instead.
