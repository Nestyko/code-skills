---
name: init-wiki
description: Initialize a persistent LLM Wiki knowledge base inside the current repository. Creates the 'knowledge-base/' directory with 'raw/' and 'wiki/' folders, index.md, log.md, and detailed instructions. Use when the user wants to initialize a wiki, set up a knowledge base, or start using Karpathy's LLM Wiki pattern.
---

# Initialize LLM Wiki

Initialize a persistent, compounding LLM Wiki knowledge base at the root of this repository.

## Workflow

### 1. Scaffold Directory Structure

Create the `knowledge-base/` folder at the repo root containing:
- `raw/` - directory for immutable source documents (place a `.gitkeep` here)
- `wiki/` - directory for LLM-maintained markdown pages (place a `.gitkeep` here)

### 2. Write instructions.md

Create `knowledge-base/instructions.md` with the following markdown content:

```markdown
# LLM Wiki Instructions

This directory is a persistent, compounding knowledge base structured as an LLM Wiki. It serves as the single source of truth for domain concepts, entities, and system-wide decisions.

## Architecture

1. **Raw sources (`knowledge-base/raw/`)**: Curated collection of source documents (articles, specs, transcripts, etc.). These are immutable.
2. **The wiki (`knowledge-base/wiki/`)**: Directory of LLM-generated markdown files. Includes entity pages, concept pages, summaries, comparisons, and syntheses.
3. **The schema (`knowledge-base/instructions.md`)**: This configuration file, outlining wiki structure, conventions, and workflows.

## Core Operations

### 1. Ingest
When a new source document is added to `knowledge-base/raw/`:
- Read the source document.
- Propose/discuss key takeaways with the user.
- Create a summary page in `knowledge-base/wiki/` for the new source (e.g., `knowledge-base/wiki/source-title.md`).
- Update the content index (`knowledge-base/wiki/index.md`) to categorize and link the new page.
- Update relevant entity and concept pages across the wiki (e.g., adding or updating glossary definitions, noting contradictions or updates).
- Append a log entry to `knowledge-base/wiki/log.md` with format: `## [YYYY-MM-DD] ingest | Source Title`.

### 2. Query
When answering questions or exploring:
- Read `knowledge-base/wiki/index.md` first to identify relevant pages.
- Drill down into those pages and synthesize a response with citations.
- Propose filing valuable syntheses or comparisons back into the wiki as new pages.

### 3. Lint
Periodically health-check the wiki:
- Find contradictions between pages.
- Locate stale claims superseded by newer sources.
- Identify orphan pages (no inbound links).
- Identify important concepts mentioned but lacking their own page.
- Suggest missing cross-references or gaps to fill.

## Navigation & Tracking Files

- **`knowledge-base/wiki/index.md`**: Content-oriented catalog of all pages in the wiki, grouped by category (Concepts, Entities, Sources).
- **`knowledge-base/wiki/log.md`**: Chronological, append-only record of all wiki events (ingests, queries, lints).
```

### 3. Initialize Index and Log Files

Create `knowledge-base/wiki/index.md`:
```markdown
# Wiki Index

Catalog of all pages in the knowledge base, updated on every ingest.

## Concepts
- *No concepts registered yet.*

## Entities
- *No entities registered yet.*

## Sources
- *No sources registered yet.*
```

Create `knowledge-base/wiki/log.md`:
```markdown
# Wiki Log

Chronological append-only record of all wiki events.

## [2026-06-04] setup | Initialized knowledge base
- Scaffolded directory structure under `knowledge-base/`
- Set up initial `index.md`, `log.md`, and `instructions.md`
```

Create `.gitkeep` files in `knowledge-base/raw/` and `knowledge-base/wiki/` if they are empty to ensure they are tracked by git.
