# LLM Wiki — Schema & Conventions

This document defines how the LLM should read, write, and maintain this wiki. The wiki is treated as a persistent knowledge base that compounds in value with every source added and every query answered.

## Core Philosophy

- The **wiki** is the long-term memory. Answers and analyses should be filed back as new pages, not lost in chat history.
- The **user** is the editor-in-chief. The **LLM** is the writer. The **raw sources** are the source of truth.
- Bookkeeping (cross-references, index updates, link maintenance) is the LLM's job — not the user's.

## Source Provenance

- Every summary page must cite the original source with a URL, date, and file reference if stored in `sources/`.
- If a raw source file changes, flag derived wiki pages as stale.

## File Naming Conventions

- Use `kebab-case.md` for all filenames.
- Entities (people, orgs, countries): `entries/entities/<name>.md`
- Concepts (ideas, frameworks): `entries/concepts/<name>.md`
- Source summaries: `entries/summaries/<source-name>.md`
- Analysis/comparisons: `entries/analysis/<topic>.md`

## Required Links

Every page must have:
- An `index` header listing which pages reference it (maintained on updates).
- Outbound `[[wikilinks]]` to related entity, concept, or analysis pages.
- A `source` section at the bottom with original reference(s).

## Ingest Workflow

When a new source is added:
1. Read the source and discuss key takeaways with the user.
2. Classify document type (report, article, transcript, letter, podcast, video, etc.)
3. Write/update the summary page in `entries/summaries/`.
4. Update relevant entity and concept pages (a single source may touch 10-15 pages).
5. Update `index.md` with new entries.
6. Append an entry to `log.md` with format: `## [YYYY-MM-DD] ingest | Source Title`

## Query & Synthesis

- Search the wiki first (use `grep` or `rg` for BM25 search).
- Answer from existing knowledge before reading the full source.
- Every interaction should produce: (1) the answer, and (2) a wiki update filing the insight.

## Lint Schedule

Periodically audit the wiki for:
- Contradictions between pages.
- Orphan pages with no inbound links.
- Data gaps requiring external research.
- Stale claims superseded by newer sources.

## Token Budgets (Progressive Disclosure)

- **L0 — Context**: ~200 tokens (project identity, purpose) — always loaded.
- **L1 — Index**: ~1-2k tokens — read this to navigate.
- **L2 — Search results**: ~2-5k tokens — specific pages returned by query.
- **L3 — Full articles**: Only when deep detail is needed.
