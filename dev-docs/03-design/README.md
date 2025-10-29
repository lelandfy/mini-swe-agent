# Design Documents

This directory contains detailed technical design documents for complex features and systems.

## Purpose

Design docs help us:
- **Plan** implementation before writing code
- **Communicate** technical approaches to the team
- **Review** designs before investing in implementation
- **Document** design rationale for future reference
- **Identify** edge cases and issues early

## When to Write a Design Doc

Write a design doc when:
- Building a complex feature that spans multiple modules
- Making significant changes to existing architecture
- Implementing algorithms or data structures
- Designing APIs or interfaces
- Planning refactoring work

## Design Doc vs. ADR vs. PRD

- **PRD:** What and why (product perspective)
- **ADR:** Architectural decisions and trade-offs
- **Design Doc:** How to implement (technical details)

## Structure

Design docs are organized by topic:

```
03-design/
├── agent-lifecycle/         # Agent execution and control flow
├── model-integration/       # LLM model integration
├── environment-abstraction/ # Command execution environments
└── [other topics]/
```

Use [design-template.md](./design-template.md) for new designs.

## Naming Convention

```
topic-area/descriptive-name.md
```

Examples:
- `agent-lifecycle/error-handling.md`
- `model-integration/cost-tracking-design.md`
- `environment-abstraction/docker-setup.md`

## Design Doc Lifecycle

```
Draft → Review → Approved → Implemented
```

Mark status in the document header.

## Tips for Writing Design Docs

- **Start with the problem:** Why are we doing this?
- **Show alternatives:** What else was considered?
- **Include diagrams:** Flow charts, sequence diagrams, architecture diagrams
- **List open questions:** What's still undecided?
- **Link to context:** PRDs, ADRs, related designs
- **Keep it current:** Update as implementation reveals new insights
- **Be specific:** Include code snippets, API signatures, data structures

## Existing Design Docs

### Agent Lifecycle
- [Agent execution flow](./agent-lifecycle/overview.md)
- [Error handling](./agent-lifecycle/error-handling.md)

### Model Integration
- [LiteLLM integration](./model-integration/litellm-design.md)

### Environment Abstraction
- [Local environment](./environment-abstraction/local-env-design.md)
