# Code Comprehension

This directory contains documentation to help understand the existing codebase.

## Purpose

Code comprehension docs help developers:
- **Understand** how the codebase works
- **Onboard** to unfamiliar modules quickly
- **Debug** issues by understanding context
- **Plan** refactoring with full knowledge
- **Review** code with better context

## When to Create Comprehension Docs

Create or update these docs when:
- Learning a new module or subsystem
- Debugging complex issues
- Planning refactoring work
- Onboarding new team members
- Code structure changes significantly

## Structure

```
04-code-comprehension/
├── codebase-overview.md    # High-level architecture map
├── modules/                # Deep dives into each module
│   ├── agents.md
│   ├── models.md
│   └── environments.md
├── flows/                  # Process and flow documentation
│   ├── agent-execution-flow.md
│   └── message-lifecycle.md
└── analysis/               # Code analysis artifacts
    ├── dependency-graph.md
    └── api-surface.md
```

## What Goes in Each Section

### codebase-overview.md
- Project structure
- Key abstractions
- Module relationships
- Entry points

### modules/
- Module purpose and responsibility
- Key classes and functions
- Public APIs
- Internal structure
- Common patterns

### flows/
- End-to-end processes
- Data flow diagrams
- Sequence diagrams
- State machines

### analysis/
- Dependency analysis
- Code metrics
- Technical debt inventory
- API surface documentation

## Format Tips

### For Module Docs
```markdown
# Module Name

## Purpose
What this module does

## Key Components
- Component 1: Brief description (link to file:line)
- Component 2: Brief description (link to file:line)

## Public API
What's exported and intended for use

## Internal Structure
How it's organized internally

## Common Patterns
Recurring patterns in this module

## Gotchas
Things that surprised you or might confuse others
```

### For Flow Docs
Use diagrams! ASCII art, Mermaid, or images.

```
User Action
    ↓
Component A (file.py:123)
    ↓
Component B (file.py:456)
    ↓
Result
```

## Existing Comprehension Docs

- [Codebase Overview](./codebase-overview.md)
- [Agents Module](./modules/agents.md)
- [Agent Execution Flow](./flows/agent-execution-flow.md)

## Tips

- **Link to code:** Use `file.py:line` references
- **Keep it updated:** Review when code changes
- **Be specific:** Point to actual implementations
- **Show examples:** Include code snippets
- **Note surprises:** Document non-obvious behaviors
