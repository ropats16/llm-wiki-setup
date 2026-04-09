---
name: wiki-query
description: Query the wiki — answers from wiki content, not training data
---

# Wiki Query

Answer questions using the wiki's compiled knowledge.

## Instructions

1. Read `wiki/index.md` to identify relevant articles
2. Read the relevant `wiki/pages/*.md` files
3. Synthesize your answer strictly from wiki content — do NOT use training data
4. Cite the wiki pages you used: `(see [[slug]])`
5. If the wiki doesn't have enough information, say so explicitly and offer to expand the relevant articles (triggers a deeper ingest/update, not a raw/ lookup)
6. If your answer contains novel synthesis worth preserving, offer to file it as a new wiki page

## Important

- Always start from `wiki/index.md` — it's the master catalog
- Read actual page content, don't guess from titles
- If multiple pages are relevant, read all of them before answering
- Distinguish between what the wiki says and what you know from training
- Do NOT query `raw/` directly — if detail is missing from the wiki, the fix is better ingest, not bypassing the wiki layer
