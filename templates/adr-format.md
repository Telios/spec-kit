# ADR Format

ADRs record load-bearing decisions. They are authored by the **plan** command, never by the
WHAT phases (specify/clarify). Keep them lean — the value is in recording *that* a decision
was made and *why*, not in filling out sections. If the project constitution defines a
decision-documentation principle, follow it; this format is the sensible default.

## Location and numbering

- Single-context repo → `docs/adr/`
- Multi-context repo → the context's own ADR directory (e.g. `<area>/docs/adr/`)

Sequential numbering within each directory: `0001-slug.md`, `0002-slug.md`, … Scan the
target directory for the highest existing number and increment by one. Create the directory
lazily only when the first ADR is needed.

## Required elements

Every ADR captures four things. Each can be a sentence or two:

```md
# {Short title of the decision}

**Context**: {What forced the decision — the constraint, requirement, or problem.}

**Decision**: {What we chose.}

**Alternatives considered**: {What else we evaluated and why we rejected it.}

**Consequences**: {What this commits us to — the downstream effects, good and bad.}
```

Add `**Status**` frontmatter (`proposed | accepted | deprecated | superseded by ADR-NNNN`)
when a decision is revisited. When a later decision overturns an earlier one, supersede the
old ADR rather than deleting it.

## When a decision is ADR-worthy

All three must be true:

1. **Hard to reverse** — changing your mind later carries meaningful cost.
2. **Surprising without context** — a future reader will look at the code and wonder "why on
   earth did they do it this way?"
3. **The result of a real trade-off** — there were genuine alternatives and you picked one
   for specific reasons.

If a decision is easy to reverse, skip it. If it's not surprising, nobody will wonder. If
there was no real alternative, there's nothing to record beyond "we did the obvious thing."

### What qualifies

- **Architectural shape** — "the write model is event-sourced, the read model is projected."
- **Integration patterns between contexts** — "Ordering and Billing talk via events, not HTTP."
- **Technology choices that carry lock-in** — database, message bus, deployment target.
- **Boundary and scope decisions** — "Customer data is owned by the Customer context; others
  reference it by ID." The explicit *no*s are as valuable as the *yes*es.
- **Deliberate deviations from the obvious path** — "manual SQL instead of an ORM because X."
- **Constraints not visible in the code** — "response times must be under 200ms because of a
  partner API contract."

## Relationship to research.md

The **plan** command produces `research.md` as its working analysis (Decision / Rationale /
Alternatives). The ADR is the durable record of the load-bearing subset. Link the ADR back
to `research.md` rather than duplicating the analysis.
