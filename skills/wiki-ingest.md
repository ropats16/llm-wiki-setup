---
name: wiki-ingest
description: Ingest a source into the wiki — full pipeline from raw to compiled wiki articles
---

# Wiki Ingest

Full ingest pipeline: source → raw → wiki articles → indices → log. Single command.

## Instructions

1. **Read SCHEMA.md** to load conventions (categories, frontmatter format, backlink rules)

2. **Determine input type:**
   - URL → detect source type (tweet, gist, article, repo, paper)
   - Raw text → treat as manual paste (ask user for title/author/date if not obvious)
   - Local file already in `raw/` → user points to it

3. **Fetch content:**
   - Articles/blogs: WebFetch
   - GitHub gists/READMEs: WebFetch or `gh` CLI
   - Pasted text: save directly as markdown
   - If fetching fails, ask user to paste the content manually

4. **Generate slug** from content (lowercase, hyphenated, descriptive)

5. **Save to `raw/`:**
   - Text content → `raw/<slug>.md`
   - Binary files (images, PDFs) → `raw/<slug>.<ext>`
   - Raw files have NO frontmatter requirement — raw means raw

6. **Update `raw/index.md`** — add row to source table:
   ```
   | slug | Title | Author | Date | Type | URL |
   ```

7. **Read `wiki/index.md`** to understand existing article landscape

8. **Compile source into wiki:**
   - Create new `wiki/pages/*.md` articles for novel topics
   - Update existing articles that this source enriches
   - Add proper frontmatter (title, created/updated dates, category, sources list)
   - Each article should synthesize, not just summarize — connect to existing knowledge

9. **Maintain `[[backlinks]]` bidirectionally** across all affected pages

10. **Update `wiki/index.md`** — add new entries, organized by category

11. **Append operation to `wiki/log.md`**

## Important

- A single source may create multiple wiki articles and update several existing ones
- Always check for existing articles that should be updated, not just create new ones
- Backlinks must be bidirectional — if A links B, add B linking A too
- Assign articles to existing categories when they fit, or create new categories as needed — categories emerge organically from ingested content
- If the source is low quality or not tier-1, flag it to the user before ingesting
