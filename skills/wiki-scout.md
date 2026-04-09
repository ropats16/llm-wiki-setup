---
name: wiki-scout
description: Discover potential sources for the wiki — finds candidates, never auto-ingests
---

# Wiki Scout

Find potential tier-1 sources to expand the wiki's knowledge base.

## Instructions

1. Read `wiki/index.md` to understand current article landscape
2. Read `raw/index.md` to know what's already been ingested
3. Determine search focus:
   - If user specified a topic/area → search around that
   - If user said "gaps" → identify thin areas from the index
   - If user gave a URL → evaluate that specific source
4. Use WebSearch to find candidate sources
5. For each candidate, evaluate:
   - **Tier-1 check**: Is this an original author, high-trust figure, or reputable outlet?
   - **Relevance**: Which existing wiki articles would this enrich?
   - **Overlap**: Does this duplicate already-ingested sources?
   - **Type**: tweet | article | gist | paper | repo
6. Present candidates as a ranked list with the above assessments
7. Do NOT ingest anything — wait for user to select candidates
8. Append scout operation to `wiki/log.md`

## Output Format

For each candidate:
- **Title** — Source title
- **Author** — Who wrote it
- **URL** — Link
- **Type** — tweet/article/gist/paper/repo
- **Tier-1?** — Yes/No with reasoning
- **Relevance** — Which wiki articles this connects to
- **Overlap** — Any overlap with existing raw sources

## Important

- Never auto-ingest. This is discovery only.
- Prefer quality over quantity — 3-5 strong candidates > 15 weak ones
- Flag non-tier-1 sources honestly, even if they seem relevant
- If a topic area has no good sources available, say so
