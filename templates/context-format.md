# CONTEXT.md Format

`CONTEXT.md` is the controlled vocabulary for a context. It is a glossary and nothing else —
no implementation details, no spec content, no decisions.

## Structure

```md
# {Context Name}

{One or two sentences: what this context is and why it exists.}

## Language

**Order**:
A concise description of the term.
_Avoid_: Purchase, transaction

**Customer**:
A person or organization that places orders.
_Avoid_: Client, buyer, account

## Relationships

- An **Order** produces one or more **Invoices**
- An **Invoice** belongs to exactly one **Customer**

## Example dialogue

> **Dev:** "When a **Customer** places an **Order**, do we create the **Invoice** immediately?"
> **Domain expert:** "No — an **Invoice** is only generated once a **Fulfillment** is confirmed."

## Flagged ambiguities

- "account" was used to mean both **Customer** and **User** — resolved: these are distinct concepts.
```

## Rules

- **Be opinionated.** When multiple words exist for one concept, pick the best and list the
  rest under `_Avoid_` as aliases.
- **Flag conflicts explicitly.** If a term was used ambiguously, record it under "Flagged
  ambiguities" with a clear resolution.
- **Keep definitions tight.** One sentence. Define what it *is*, not what it does.
- **Show relationships.** Bold the term names; express cardinality where it's obvious.
- **Only project-specific terms.** General programming concepts (timeouts, retries, error
  types) don't belong, even if the project uses them heavily. Ask: is this unique to this
  context, or a general concept? Only the former belongs.
- **Group under subheadings** when natural clusters emerge; a flat list is fine otherwise.

## Single vs multiple contexts

**Single context (most repos):** one `CONTEXT.md` at the repo root.

**Multiple contexts:** a root `CONTEXT-MAP.md` lists the contexts, where they live, and how
they relate:

```md
# Context Map

## Contexts

- [Ordering](./src/ordering/CONTEXT.md) — receives and tracks customer orders
- [Billing](./src/billing/CONTEXT.md) — generates invoices and processes payments

## Relationships

- **Ordering → Billing**: Ordering emits `OrderPlaced` events; Billing consumes them
- Shared types: `CustomerId`, `Money`
```

Create `CONTEXT-MAP.md` lazily the first time you author cross-context vocabulary. When a
term clearly belongs to one context, write it into that context's `CONTEXT.md`.
