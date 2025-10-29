# ADR-0001: Record Architecture Decisions

**Status:** Accepted
**Date:** 2025-01-29
**Deciders:** mini-swe-agent team
**Related:** dev-docs structure

## Context

As the mini-swe-agent project grows, we need a way to document architectural decisions and their rationale. Team members need to understand:
- Why certain approaches were chosen over alternatives
- What trade-offs were made
- Historical context for current architecture

Without documentation, we risk:
- Re-litigating already-decided questions
- Making inconsistent decisions
- Slower onboarding for new contributors
- Loss of institutional knowledge

## Decision

We will use Architecture Decision Records (ADRs) to document significant architectural decisions.

An ADR:
- Is a numbered markdown document (0001, 0002, etc.)
- Captures context, decision, alternatives, and consequences
- Is immutable once accepted (new ADRs supersede old ones)
- Lives in `dev-docs/02-adr/`
- Follows the format in `adr-template.md`

We will write ADRs for:
- Architectural patterns and structure
- Technology and library choices
- API design decisions
- Security or performance trade-offs
- Significant refactoring approaches

## Alternatives Considered

### Option 1: Wiki or Confluence
- **Pros:** Rich formatting, easy editing, search
- **Cons:** Not in version control, can become stale, separate tool
- **Why not chosen:** Want decisions version-controlled with code

### Option 2: Code Comments
- **Pros:** Close to the code, no separate docs
- **Cons:** Hard to find, scattered, no structure
- **Why not chosen:** Need centralized, discoverable location

### Option 3: GitHub Issues/Discussions
- **Pros:** Already using GitHub, good for collaboration
- **Cons:** Not structured, hard to maintain, buried in noise
- **Why not chosen:** ADRs need permanent, structured home

## Consequences

### Positive Consequences
- ✅ Architectural decisions are documented and version-controlled
- ✅ New contributors can understand why things are the way they are
- ✅ Prevents rehashing old decisions
- ✅ Forces explicit decision-making process
- ✅ Creates institutional memory

### Negative Consequences
- ❌ Requires discipline to write ADRs consistently
- ❌ Adds overhead to decision-making process
- ❌ ADRs can become outdated if not maintained

### Risks
- **Risk:** ADRs not kept up to date
  - **Mitigation:** Include ADR review in PR process when architecture changes
- **Risk:** Over-documentation of trivial decisions
  - **Mitigation:** Clear guidelines on when ADRs are needed

## Implementation Notes

To implement this decision:
1. ✅ Create `dev-docs/02-adr/` directory
2. ✅ Add README.md explaining ADR process
3. ✅ Create adr-template.md
4. ✅ Write this ADR as first example
5. Document major existing architectural decisions retroactively

## References

- [Michael Nygard's ADR format](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions)
- [ADR GitHub organization](https://adr.github.io/)
- [ThoughtWorks Technology Radar on ADRs](https://www.thoughtworks.com/radar/techniques/lightweight-architecture-decision-records)
