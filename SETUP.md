# LLM Wiki Setup

You are setting up an LLM-maintained wiki for the user. Follow these steps exactly.

## Step 1: Ask the user two questions

Ask these in a single message:
1. **"What's your wiki about?"** — The topic shapes the wiki's overview and schema.
2. **"What do you want to call it?"** — The name goes in CLAUDE.md header and the wiki overview.

Wait for answers before proceeding.

## Step 2: Fetch and customize template files

Fetch these two files from the setup repo and customize them:

**Important:** Use `curl` (not WebFetch) to fetch template files — WebFetch summarizes content through a model, which mangles templates. These URLs return raw text.

### SCHEMA.md
1. `curl -s https://raw.githubusercontent.com/ropats16/llm-wiki-setup/main/schema-template.md`
2. Replace `{{WIKI_NAME}}` with the user's wiki name
3. Write as `SCHEMA.md` in the current directory

### CLAUDE.md
1. `curl -s https://raw.githubusercontent.com/ropats16/llm-wiki-setup/main/claude-template.md`
2. Replace `{{WIKI_NAME}}` with the user's wiki name
3. Write as `CLAUDE.md` in the current directory

## Step 3: Install skills

Fetch each skill file and install to `.claude/skills/`:

Use `curl -s` for each (same reason — need raw content, not summaries):

1. `curl -s https://raw.githubusercontent.com/ropats16/llm-wiki-setup/main/skills/wiki-ingest.md`
   → Write to `.claude/skills/wiki-ingest/SKILL.md`

2. `curl -s https://raw.githubusercontent.com/ropats16/llm-wiki-setup/main/skills/wiki-query.md`
   → Write to `.claude/skills/wiki-query/SKILL.md`

3. `curl -s https://raw.githubusercontent.com/ropats16/llm-wiki-setup/main/skills/wiki-scout.md`
   → Write to `.claude/skills/wiki-scout/SKILL.md`

4. `curl -s https://raw.githubusercontent.com/ropats16/llm-wiki-setup/main/skills/wiki-lint.md`
   → Write to `.claude/skills/wiki-lint/SKILL.md`

## Step 4: Create directory structure

```
raw/
  index.md
wiki/
  index.md
  log.md
  overview.md
  pages/           (empty directory — create with a .gitkeep)
```

### raw/index.md
```markdown
# Raw Sources Index

All source material ingested into the wiki. These files are immutable — never modified after creation.

| Slug | Title | Author | Date | Type | URL |
|------|-------|--------|------|------|-----|
```

### wiki/index.md
```markdown
# {Wiki Name} — Article Index

A catalog of all articles in the wiki. Categories emerge as sources are ingested.
```

### wiki/log.md
```markdown
# {Wiki Name} — Operation Log

Append-only record of all wiki operations.

| Date | Operation | Files Touched | Notes |
|------|-----------|---------------|-------|
| {today's date} | init | all | Wiki initialized with empty structure |
```

### wiki/overview.md
```markdown
# {Wiki Name}

{Wiki Name} is a new wiki about {topic}. It follows Karpathy's LLM Knowledge Base pattern — an LLM incrementally builds and maintains this wiki from raw sources.

## Getting Started

Start by ingesting your first source:
- `/wiki-ingest <url-or-pasted-text>` — Add a source and compile it into wiki articles
- `/wiki-query <question>` — Ask questions answered from wiki content
- `/wiki-scout <topic>` — Discover new sources to ingest
- `/wiki-lint` — Health check the wiki's structure and content

## Architecture

- `raw/` — Immutable source material (any format). Metadata in `raw/index.md`.
- `wiki/pages/` — LLM-maintained articles synthesized from raw sources.
- `SCHEMA.md` — Conventions and rules the LLM follows.
```

## Step 5: Print getting-started message

After everything is created, print:

```
{Wiki Name} is ready!

Your wiki has 4 skills installed:
- /wiki-ingest <url-or-text>  — Add and compile a source
- /wiki-query <question>      — Ask your wiki
- /wiki-scout <topic>         — Find new sources
- /wiki-lint                   — Health check

Tutorial: try ingesting Karpathy's original idea file:
  /wiki-ingest https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f

For the full walkthrough, see:
  https://github.com/ropats16/llm-wiki-setup#tutorial-your-first-wiki-operations
```

## What NOT to do

- Do NOT create `.claude/settings.local.json` — user manages permissions locally
- Do NOT auto-ingest any sources — the wiki starts empty
- Do NOT create an `assets/` directory — all source material goes in `raw/`
