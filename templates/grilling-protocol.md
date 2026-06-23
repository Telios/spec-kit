# Grilling Protocol

The shared interview protocol for Spec Kit's interactive phases. The **specify** and
**clarify** commands load this to grill the **WHAT/WHY**; the **plan** command loads it to
grill the **HOW**. The goal is a genuine shared understanding between the agent and the
engineer before anything is committed — not a fixed number of questions.

## How to interview

- **One question at a time.** Ask, wait for the answer, then ask the next. Never dump a
  batch of questions.
- **Always recommend an answer.** For every question, state your recommended answer and a
  one-or-two-sentence reason before the engineer responds. The engineer can accept it
  ("yes" / "recommended") or override.
- **Explore before you ask.** If a question can be answered by reading the codebase, the
  spec, `CONTEXT.md`, or existing ADRs — answer it yourself and confirm, rather than
  asking. Only ask what the code cannot tell you.
- **Walk the design tree.** Resolve decisions in dependency order: settle the parent
  decision before the ones that hang off it. Follow each branch to its leaves before
  moving to the next.
- **No numeric cap.** Keep grilling until there are no open branches left **and** the
  engineer signals completion ("done", "good", "no more", "proceed"). Depth is the point —
  do not stop just because you have asked some number of questions.

## Sharpen the language

- **Challenge against the glossary.** When a term conflicts with `CONTEXT.md`, call it out:
  "Your glossary defines 'cancellation' as X, but you seem to mean Y — which is it?"
- **Resolve fuzzy terms.** When a word is vague or overloaded, propose a precise canonical
  term: "You're saying 'account' — do you mean the Customer or the User?"
- **Stress-test with scenarios.** When domain relationships are in play, invent concrete
  edge-case scenarios that force precise boundaries between concepts.
- **Cross-reference the code.** When the engineer states how something works, check whether
  the code agrees. Surface contradictions: "Your code cancels entire Orders, but you said
  partial cancellation is possible — which is right?"

## What each phase emits (altitude rule)

Each phase runs this same protocol but writes only the document at its altitude. This is how
the flow keeps decisions out of the spec without losing them.

| Phase | Grills about | Emits |
|-------|--------------|-------|
| specify, clarify | WHAT / WHY — domain language, scope, requirements | `spec.md` + `CONTEXT.md` glossary |
| plan | HOW — architecture, technology, trade-offs | ADRs under `docs/adr/` |

### Update CONTEXT.md inline (specify / clarify)

When a domain term is resolved, update `CONTEXT.md` right then — don't batch. Use the format
in [context-format.md](./context-format.md). `CONTEXT.md` is a glossary and nothing else: no
implementation details, no spec content, no decisions.

### Park ADRs, never write them (specify / clarify)

A decision is **ADR-worthy** when all three are true: hard to reverse, surprising without
context, and the result of a real trade-off (see [adr-format.md](./adr-format.md)). These
are HOW decisions and belong to the **plan** command, not to the WHAT phases.

When the grilling surfaces an ADR-worthy decision during specify or clarify, **do not write
an ADR.** Instead append the open topic to the spec's `## Decisions for Planning` section,
phrased as a question or topic (not a resolved choice), so the spec stays free of
implementation detail. The **plan** command reads that section and authors the ADR there.

### Author ADRs (plan)

The **plan** command reads `## Decisions for Planning`, grills each open topic to a
resolution, and records every load-bearing decision as an ADR per
[adr-format.md](./adr-format.md). The ADR is the durable record; `research.md` is the
working analysis — link from one to the other, don't duplicate the content.

## Single vs multiple contexts

A "context" is a bounded area of the domain with its own vocabulary. Most repos have one:

- **Single context (default):** one `CONTEXT.md` at the repo root; ADRs in `docs/adr/`.
- **Multiple contexts:** a `CONTEXT-MAP.md` at the repo root indexes the contexts, where each
  `CONTEXT.md` lives, and how they relate — so the same word can mean different things in
  different areas. Each context keeps its own `CONTEXT.md` and ADR directory (e.g.
  `<area>/docs/adr/`).

Decide which context the current feature belongs to and write `CONTEXT.md` / ADRs into that
context. If a `CONTEXT-MAP.md` exists, read it first to find the contexts. If neither
`CONTEXT.md` nor `CONTEXT-MAP.md` exists yet, create a root `CONTEXT.md` lazily when the
first term is resolved. When the target context is unclear, ask.
