# LLM Wiki — Schema

This is the schema file for this wiki. It tells you (the LLM) how this wiki is structured, what the conventions are, and what workflows to follow. You own the wiki layer entirely. The human curates sources and asks questions; you do the writing, cross-referencing, and maintenance.

## Directory layout

```
llm-wiki/
├── CLAUDE.md          # this file — the schema
├── index.md           # content catalog — update on every ingest
├── wiki/              # all LLM-generated pages (you write here)
│   ├── log.md         # append-only chronological log
│   ├── overview.md    # high-level synthesis of everything in the wiki
│   ├── entities/      # pages for specific people, projects, companies, models
│   ├── concepts/      # pages for ideas, techniques, patterns, terminology
│   └── summaries/     # one summary page per ingested source
└── sources/           # raw, immutable source documents (you read, never write)
    ├── llm/           # articles and notes about LLMs
    ├── books/         # book content
    ├── personal/      # personal notes (career, resume, etc.)
    └── images/        # images and attachments referenced by source documents
```

If a subdirectory doesn't exist yet under `wiki/`, create it as needed.

## Page format

All wiki pages are markdown. Use `[[wikilink]]` syntax for internal links (Obsidian-compatible). Add YAML frontmatter to every page:

```yaml
---
title: Page Title
type: concept | entity | source | overview | index | log
tags: [tag1, tag2]
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: 0   # number of source documents this page draws from
---
```

Use `##` for top-level sections within a page. Keep pages focused — one concept or entity per page. If a page grows beyond ~500 lines, consider splitting it.

## index.md format

The index is a catalog of every wiki page. Organize by category. Each entry: `- [[PageName]] — one-line summary`. Update it on every ingest and whenever pages are created or removed.

## log.md format

Append-only. Each entry starts with `## [YYYY-MM-DD] <type> | <title>` where type is one of: `ingest`, `query`, `lint`. This makes entries greppable. Example:

```
## [2026-04-07] ingest | LLM Wiki Pattern
- Added summary page for llm-wiki source document
- Created concept pages: wiki-as-artifact, rag-vs-wiki
- Updated overview.md with core pattern
```

## Workflows

### Ingest

When given a new source to process:

1. Read the source document in full.
2. Discuss key takeaways with the human if they want to be involved, or proceed directly if they ask for batch processing.
3. Write a summary page to `wiki/sources/<slug>.md`.
4. Create or update entity pages (`wiki/entities/`) for any significant people, projects, models, or organizations mentioned.
5. Create or update concept pages (`wiki/concepts/`) for key ideas and techniques.
6. Update `wiki/overview.md` if the source shifts or strengthens the overall synthesis.
7. Update `wiki/index.md` with any new pages.
8. Append an entry to `wiki/log.md`.

A single source will typically touch 5-15 wiki pages. That's expected and correct.

### Query

When asked a question:

1. Read `wiki/index.md` to identify relevant pages.
2. Read those pages.
3. Synthesize an answer with citations to wiki pages and, where relevant, source documents.
4. If the answer is substantive (a comparison, an analysis, a new synthesis), offer to file it as a new wiki page. Good answers compound in the wiki just like ingested sources do.
5. If answering the question revealed a gap (missing page, missing cross-reference, stale claim), note it and offer to fix it.

### Lint

When asked to health-check the wiki:

1. Read `wiki/index.md` and a sample of wiki pages.
2. Look for: contradictions between pages, stale claims superseded by newer sources, orphan pages with no inbound links, important concepts mentioned but lacking their own page, missing cross-references, data gaps worth investigating.
3. Report findings and offer to fix them.
4. Suggest new sources to look for or new questions to investigate.
5. Append a lint entry to `wiki/log.md`.

## Conventions

- **Internal links:** Use `[[PageName]]` syntax throughout. When you mention a concept or entity that has (or should have) its own page, link it.
- **Contradiction handling:** When a new source contradicts an existing claim, update the relevant pages to note both positions and the tension between them. Don't silently overwrite — flag it.
- **Source attribution:** On entity and concept pages, note which sources support each claim. You don't need inline citations for every sentence, but a `## Sources` section at the bottom of each page listing contributing source summary pages is good practice.
- **Immutable sources:** Never modify files under `sources/`. They are the ground truth. If a source is wrong or outdated, note that in the wiki page about it, not in the source itself.
- **Dates:** Use ISO 8601 (YYYY-MM-DD) for all dates.
- **Slug format:** Use kebab-case for filenames. Example: `wiki/concepts/rag-vs-wiki.md`.

## Domain

This wiki covers LLMs, AI tooling, and related topics. The primary focus areas will emerge from the sources ingested. When in doubt about whether something deserves its own page, err on the side of creating it — orphan pages are easier to merge than missing context is to reconstruct.

## Writing Style

**Never use em-dashes (—).** Use these alternatives instead:
- Use a comma for a soft aside
- Use a colon to introduce or expand on a point
- Use parentheses for a true aside
- Use a period to split into two sentences when the thought is independent

**Never use em-dashes, examples**
- "She arrived late, exhausted from the trip." (not "She arrived late—exhausted from the trip.")
- "One thing matters: consistency." (not "One thing matters—consistency.")

** never use "rule of thumb" or expressions like it
- because they are racist
- the rule of thumb was literally about how much value was in one slave's thumb
instead use: "many people say" or "a common pattern" and the like