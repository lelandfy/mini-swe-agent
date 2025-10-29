# Architecture Decision Records (ADRs)

This directory contains Architecture Decision Records documenting significant architectural decisions made in the mini-swe-agent project.

## What is an ADR?

An ADR is a document that captures an important architectural decision along with its context and consequences. ADRs help us:
- **Remember** why we made certain decisions
- **Onboard** new team members faster
- **Prevent** revisiting already-decided questions
- **Learn** from past decisions

## When to Write an ADR

Write an ADR when making decisions about:
- System architecture or structure
- Technology choices (languages, frameworks, libraries)
- Design patterns or coding standards
- Data models or storage strategies
- API design or integration approaches
- Security or performance trade-offs

## ADR Format

We follow the format popularized by Michael Nygard:
1. **Title** - Short noun phrase
2. **Status** - Proposed, Accepted, Deprecated, Superseded
3. **Context** - What forces are at play?
4. **Decision** - What we decided
5. **Consequences** - What becomes easier/harder?

Use [adr-template.md](./adr-template.md) to create new ADRs.

## Naming Convention

```
NNNN-short-title-in-kebab-case.md
```

ADRs are numbered sequentially starting from 0001.

## ADR Lifecycle

```
Proposed → Accepted → [Deprecated/Superseded]
```

- **Proposed:** Under discussion
- **Accepted:** Approved and being followed
- **Deprecated:** No longer recommended but not replaced
- **Superseded by ADR-XXXX:** Replaced by a newer decision

## Existing ADRs

| Number | Title | Status | Date |
|--------|-------|--------|------|
| [0001](./0001-record-architecture-decisions.md) | Record Architecture Decisions | Accepted | 2025-01 |
| [0002](./0002-use-environment-abstraction.md) | Use Environment Abstraction | Accepted | 2025-01 |

## Tips for Writing ADRs

- **Be specific:** Avoid vague language
- **Explain context:** Future readers need to understand the situation
- **List alternatives:** Show what else was considered
- **Document consequences:** Both positive and negative
- **Keep it short:** 1-2 pages max
- **Don't revise:** If a decision changes, write a new ADR that supersedes the old one
