# 1. Record architecture decisions

- **Status:** Accepted
- **Date:** 2026-10-20

## Context

Decisions about architecture, tooling and conventions get made in chats and PR threads and
are then forgotten. New contributors (and future us) can't tell why things are the way they are.

## Decision

We record significant decisions as Architecture Decision Records (ADRs) in `docs/adr/`,
following [Michael Nygard's format](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions).

- One decision per file, named `NNNN-short-title.md` with a zero-padded, increasing number.
- An ADR is never edited after it's accepted, except to change its status. To reverse a
  decision, write a new ADR and mark the old one `Superseded by ADR-NNNN`.
- Write an ADR when a choice is hard to reverse, affects the whole codebase, or would
  surprise a newcomer.

## Consequences

Decisions and their trade-offs are discoverable next to the code and reviewed in PRs like code.
Writing one takes a few minutes per decision.

---

## Template

Copy this into `docs/adr/NNNN-short-title.md`:

```markdown
# NNNN. Title

- **Status:** Proposed | Accepted | Deprecated | Superseded by ADR-NNNN
- **Date:** YYYY-MM-DD

## Context

What is the problem, and what forces (constraints, requirements, trade-offs) are at play?

## Decision

What we decided to do, stated in full sentences ("We will ...").

## Consequences

What becomes easier or harder as a result, including the downsides we accept.
```
