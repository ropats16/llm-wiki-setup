# LLM Wiki Setup

Create your own LLM-maintained knowledge base. Based on [Karpathy's LLM Knowledge Base pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

## What is this?

An LLM wiki is a personal knowledge base where an LLM does the maintenance work — cross-references, synthesis, indexing — that makes humans abandon wikis. You curate sources and ask questions. The LLM does the bookkeeping.

Read [Karpathy's idea file](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) for the full motivation.

## Quick Start

1. Open Claude Code in your desired folder
2. Paste this prompt:
   > Set up an LLM wiki here. Follow https://raw.githubusercontent.com/ropats16/llm-wiki-setup/main/SETUP.md
3. Answer Claude's questions about your wiki topic and name

That's it. Claude creates the directory structure, installs 4 skills, and seeds the wiki.

## Tutorial: Your First Wiki Operations

### Ingest a source

```
/wiki-ingest https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f
```

This fetches the source, saves it to `raw/`, creates wiki articles, updates indices, and logs the operation.

### Query your wiki

```
/wiki-query What are LLM knowledge bases?
```

Answers strictly from wiki content, not training data. Cites the pages it used.

### Check wiki health

```
/wiki-lint
```

Finds broken links, missing backlinks, orphan pages, and content gaps. Offers to auto-fix structural issues.

### Discover new sources

```
/wiki-scout <your-topic>
```

Searches for tier-1 sources, evaluates them, and presents candidates. Never auto-ingests.

## Viewing Your Wiki

The wiki is plain markdown with `[[backlinks]]` — which is [Obsidian](https://obsidian.md)'s native syntax. Open your wiki directory in Obsidian to get backlink navigation, graph view, and search for free.

Or just use `/wiki-query` — the LLM is the primary interface.

## How It Works

**Two layers:**

- **`raw/`** — Your source material. Any format (text, images, PDFs). Immutable — the LLM reads from these but never changes them. Metadata lives in `raw/index.md`.
- **`wiki/`** — LLM-maintained articles. Interlinked markdown pages synthesized from raw sources. The LLM owns this layer — creates pages, maintains cross-references, keeps everything consistent.

**Plus a schema** (`SCHEMA.md`) that tells the LLM how the wiki is structured and what conventions to follow. You and the LLM co-evolve this over time.

## What's Included

| File | Purpose |
|------|---------|
| `SETUP.md` | Claude's entry point — full setup instructions |
| `schema-template.md` | SCHEMA.md template with placeholders |
| `claude-template.md` | CLAUDE.md template with placeholders |
| `skills/wiki-ingest.md` | Full ingest pipeline: source → raw → wiki |
| `skills/wiki-query.md` | Query wiki content with citations |
| `skills/wiki-scout.md` | Discover and evaluate potential sources |
| `skills/wiki-lint.md` | Structural and content health checks |
