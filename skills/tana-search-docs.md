---
name: Search and read Tana documentation
description: Programmatically discover, search, and read Tana and Tana Outliner documentation as clean markdown via the public Documentation API.
api: openapi/tana-docs-api-openapi.yml
operations:
  - getDocsIndex
  - searchDocs
  - readDoc
---

# Search and read Tana documentation

The Tana **Documentation API** is public and unauthenticated — ideal for grounding an agent
in accurate, current Tana docs instead of stale training data.

## Base
`https://tana.inc` (product `tana`) or `https://outliner.tana.inc` (product `outliner`).

## Steps
1. Call `getDocsIndex` — `GET /api/docs/index` — to load the feature taxonomy and section
   outlines as markdown.
2. Call `searchDocs` — `GET /api/docs/search?q=<pattern>` (min 2 chars). Optionally scope with
   `features=` (comma-separated feature keys), `product=tana|outliner`, and `limit=`. Each
   result includes a `path` in the form `product/type/slug`.
3. Call `readDoc` — `GET /api/docs/read?path=<path>` — to read a full page as clean markdown.
   Pass `section=<headingId>` for a single section (`intro` for pre-heading content), or
   `format=raw` for original MDX.

## Notes
- Tana Inc. ships two products that share the brand: **Tana** (agentic meeting platform,
  `tana.inc`) and **Tana Outliner** (note-taking outliner, `outliner.tana.inc`). Use the
  `product` param or the matching host to avoid conflating them.
- An `llms.txt` index is available at `https://tana.inc/llms.txt`.
