# Development Documentation

This directory contains internal development documentation for mini-swe-agent. Unlike the public documentation in `docs/`, these files are for developers working on the project.

## 📁 Directory Structure

### [01-requirements/](./01-requirements/)
Product Requirements Documents (PRDs) that define features, user stories, and acceptance criteria.

**When to use:** Before starting any new feature development.

### [02-adr/](./02-adr/)
Architecture Decision Records (ADRs) documenting significant architectural decisions and their rationale.

**When to use:** When making decisions that affect the system's structure, dependencies, or technical direction.

### [03-design/](./03-design/)
Detailed design documents for complex features, including algorithms, data structures, and implementation plans.

**When to use:** For complex features that need technical design before implementation.

### [04-code-comprehension/](./04-code-comprehension/)
Documentation about existing code: module overviews, flow diagrams, and analysis of how things work.

**When to use:** When onboarding, debugging, or planning refactoring work.

### [05-research/](./05-research/)
Research spikes, proof-of-concepts, benchmark results, and investigation notes.

**When to use:** For exploratory work, technology evaluations, or competitive analysis.

### [06-meeting-notes/](./06-meeting-notes/)
Records of team meetings, design reviews, and important discussions.

**When to use:** After any meeting where decisions are made or context is shared.

### [07-rfcs/](./07-rfcs/)
Request for Comments (RFCs) for major changes that need team input and consensus.

**When to use:** For significant changes that affect multiple areas or need broader discussion.

### [08-onboarding/](./08-onboarding/)
Guides for new developers: setup instructions, code walkthroughs, and contribution workflows.

**When to use:** To help new team members get up to speed quickly.

## 📝 Document Lifecycle

```
Idea → Research (05) → RFC (07) → PRD (01) → ADR (02) → Design (03) → Implementation → Comprehension (04)
```

## 🎯 Quick Start

**New to the codebase?** Start here:
1. Read [08-onboarding/code-walkthrough.md](./08-onboarding/code-walkthrough.md)
2. Review [04-code-comprehension/codebase-overview.md](./04-code-comprehension/codebase-overview.md)
3. Check key ADRs in [02-adr/](./02-adr/)

**Building a new feature?** Follow this:
1. Write or review PRD in [01-requirements/](./01-requirements/)
2. Create design doc in [03-design/](./03-design/)
3. Document key decisions as ADRs in [02-adr/](./02-adr/)

**Understanding existing code?** Look at:
1. Module documentation in [04-code-comprehension/modules/](./04-code-comprehension/modules/)
2. Flow diagrams in [04-code-comprehension/flows/](./04-code-comprehension/flows/)

## ✍️ Templates

Each directory contains templates to help you create consistent documentation:
- `01-requirements/prd-template.md`
- `02-adr/adr-template.md`
- `03-design/design-template.md`
- `06-meeting-notes/templates/meeting-notes-template.md`
- `07-rfcs/rfc-template.md`

## 🔗 Related Documentation

- **Public Docs:** See `docs/` for user-facing documentation
- **API Reference:** See `docs/reference/` for auto-generated API docs
- **Contributing:** See `docs/contributing.md` for contribution guidelines

## 📌 Conventions

- Use **kebab-case** for file names (e.g., `agent-lifecycle-design.md`)
- Include **dates** in time-sensitive documents (e.g., `2025-01-15-planning.md`)
- Number ADRs and RFCs sequentially (e.g., `0001-record-decisions.md`)
- Keep documents **focused** - split large documents into smaller ones
- Link between documents liberally for cross-referencing
