# Domain Docs (LLM Wiki)

How the engineering skills should consume this repo's domain documentation when exploring the codebase.

## Before exploring, read these

- **`knowledge-base/wiki/index.md`** — read the wiki index to understand what concepts, entities, and sources exist.
- **`knowledge-base/instructions.md`** — read the instructions on how this wiki is maintained and structured.
- **`knowledge-base/wiki/*.md`** — read any wiki page relevant to the area/topic you're exploring or working in.
- **`docs/adr/`** — read ADRs that touch the area you're about to work in.

If the `knowledge-base/` folder or files don't exist, **proceed silently**. Don't flag their absence; don't suggest creating them upfront. The repository needs to be initialized first (e.g., using the `init-wiki` skill), or concepts should be created lazily.

## File structure

```
/
├── knowledge-base/
│   ├── raw/                           ← immutable source documents
│   ├── wiki/                          ← LLM-maintained markdown files
│   │   ├── index.md                   ← content catalog
│   │   ├── log.md                     ← chronological log of events
│   │   └── Concept.md                 ← specific wiki page/glossary entry
│   └── instructions.md                ← wiki schema and LLM instructions
└── docs/adr/
    ├── 0001-event-sourced-orders.md
    └── 0002-postgres-for-write-model.md
```

## Use the LLM Wiki's vocabulary

When your output names a domain concept (in an issue title, a refactor proposal, a hypothesis, a test name), use the terms as defined in the wiki pages under `knowledge-base/wiki/`. Don't drift to synonyms the wiki explicitly avoids.

If the concept you need isn't in the wiki yet, that's a signal — either you're inventing language the project doesn't use (reconsider) or there's a real gap (ingest/add it to the wiki using `/grill-with-docs`).

## Flag ADR conflicts

If your output contradicts an existing ADR, surface it explicitly rather than silently overriding:

> _Contradicts ADR-0007 (event-sourced orders) — but worth reopening because…_
