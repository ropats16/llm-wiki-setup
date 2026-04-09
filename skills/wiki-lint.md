---
name: wiki-lint
description: Health check the wiki — find broken links, missing backlinks, orphan pages, and content gaps
---

# Wiki Lint

Comprehensive health check over the wiki's structure and content.

## Instructions

1. **Read SCHEMA.md** to load conventions

2. **Read `wiki/index.md`** and enumerate all referenced pages

3. **Read all `wiki/pages/*.md` files**

4. **Run structural checks:**
   - **Broken `[[backlinks]]`** — links to non-existent pages
   - **Missing bidirectional links** — A→B exists but B→A doesn't
   - **Orphan pages** — files in `wiki/pages/` not listed in `wiki/index.md`
   - **Ghost entries** — listed in `wiki/index.md` but file doesn't exist
   - **Frontmatter validation** — missing/invalid title, category, created, updated, sources
   - **`raw/index.md` consistency** — raw files not in index, or index entries without files

5. **Run content checks:**
   - **Stale info** — contradictions between articles
   - **Coverage gaps** — topics frequently `[[linked]]` but without dedicated articles
   - **Thin articles** — very short pages that could be expanded

6. **Output report:**
   - **Errors** (must fix): broken links, missing backlinks, orphans, ghost entries, frontmatter issues
   - **Warnings** (should fix): stale info, gaps, thin articles

7. **Offer to auto-fix** structural issues (add missing backlinks, update index entries, fix frontmatter)

8. **Append lint operation to `wiki/log.md`**

## Important

- Read every wiki page — don't sample or skip
- Structural errors are auto-fixable; content issues are flagged for human judgment
- When fixing backlinks, always maintain bidirectionality
- A "thin" article is one with fewer than ~5 substantive sentences
- Coverage gaps = slugs that appear in 3+ `[[backlinks]]` across the wiki but have no dedicated page
