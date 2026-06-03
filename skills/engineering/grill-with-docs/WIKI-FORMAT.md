# Wiki Page Format

Each concept, entity, or topic in the LLM Wiki (`knowledge-base/wiki/`) has its own markdown file.

## Template for a Concept / Entity Page

```markdown
# {Concept Name}

{A tight one or two sentence description of the concept or entity. Define what it IS, not what it does.}

## Details & Invariants
- {Invariant or rule 1}
- {Invariant or rule 2}

## Avoid
{Synonyms or conflicting terms that this concept replaces}
- Avoid: {Conflicting term 1}, {Conflicting term 2}
```

## Rules for Wiki Pages

- **Keep pages focused**: Each page should represent a single distinct concept or entity.
- **Maintain links**: Cross-link between concept pages using standard relative markdown links (e.g. `[Order](Order.md)`).
- **Update index**: Every time a page is created or updated, update the Content Index (`knowledge-base/wiki/index.md`) to include its link and one-line summary.
- **No implementation details**: The wiki should focus on domain language and concepts, not specific classes, functions, or database schemas.
