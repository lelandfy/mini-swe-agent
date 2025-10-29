# Getting Started with Dev-Docs

Welcome to the mini-swe-agent internal development documentation!

## What's This?

The `dev-docs/` folder contains internal documentation for developers working on mini-swe-agent:
- Product requirements
- Architecture decisions
- Design documents
- Code comprehension guides
- Research findings
- Meeting notes
- RFCs
- Onboarding materials

This is separate from the public-facing `docs/` folder.

## Quick Navigation

### 🆕 New to the Project?

Start here:
1. **[Onboarding](./08-onboarding/)** - Setup and code walkthrough
2. **[Codebase Overview](./04-code-comprehension/codebase-overview.md)** - Architecture map
3. **[Key ADRs](./02-adr/)** - Why things are the way they are

### 🔨 Building a Feature?

Follow this path:
1. **[Requirements](./01-requirements/)** - Write or review PRD
2. **[Design](./03-design/)** - Create technical design
3. **[ADRs](./02-adr/)** - Document key decisions
4. **[RFCs](./07-rfcs/)** - If you need team consensus

### 🔍 Understanding Existing Code?

Look here:
1. **[Code Comprehension](./04-code-comprehension/)** - Module guides
2. **[Design Docs](./03-design/)** - How features are built
3. **[ADRs](./02-adr/)** - Past architectural decisions

### 🔬 Researching Something?

Check:
1. **[Research](./05-research/)** - Past investigations
2. **[Meeting Notes](./06-meeting-notes/)** - Discussion history

## Directory Structure

```
dev-docs/
├── README.md                    # Navigation guide (start here)
├── GETTING_STARTED.md          # This file
│
├── 01-requirements/            # Product Requirements
│   ├── README.md
│   ├── prd-template.md
│   └── prd-001-interactive-mode.md  # Example
│
├── 02-adr/                     # Architecture Decision Records
│   ├── README.md
│   ├── adr-template.md
│   ├── 0001-record-architecture-decisions.md
│   └── 0002-use-environment-abstraction.md
│
├── 03-design/                  # Detailed Design Docs
│   ├── README.md
│   ├── design-template.md
│   └── agent-lifecycle/
│       ├── overview.md
│       └── error-handling.md
│
├── 04-code-comprehension/      # Code Understanding
│   ├── README.md
│   ├── codebase-overview.md
│   ├── modules/
│   │   └── agents.md
│   ├── flows/
│   └── analysis/
│
├── 05-research/                # Research & Investigations
│   └── README.md
│
├── 06-meeting-notes/           # Meeting Records
│   ├── README.md
│   ├── 2025/
│   └── templates/
│       └── meeting-notes-template.md
│
├── 07-rfcs/                    # Request for Comments
│   ├── README.md
│   └── rfc-template.md
│
└── 08-onboarding/              # New Developer Guides
    ├── README.md
    └── code-walkthrough.md
```

## Document Types Explained

### PRD (Product Requirements Document)
- **What:** Feature description and requirements
- **When:** Before starting feature development
- **Template:** [01-requirements/prd-template.md](./01-requirements/prd-template.md)

### ADR (Architecture Decision Record)
- **What:** Record of an architectural decision
- **When:** After making significant architectural choice
- **Template:** [02-adr/adr-template.md](./02-adr/adr-template.md)

### Design Doc
- **What:** Technical implementation details
- **When:** Planning complex features
- **Template:** [03-design/design-template.md](./03-design/design-template.md)

### RFC (Request for Comments)
- **What:** Proposal seeking team feedback
- **When:** Before making major changes
- **Template:** [07-rfcs/rfc-template.md](./07-rfcs/rfc-template.md)

## Typical Workflow

### For New Features

```
1. Research (05-research/)
   ↓
2. RFC if needed (07-rfcs/)
   ↓
3. PRD (01-requirements/)
   ↓
4. Design Doc (03-design/)
   ↓
5. ADR for decisions (02-adr/)
   ↓
6. Implement
   ↓
7. Code Comprehension (04-code-comprehension/)
```

### For Understanding Code

```
1. Start: Codebase Overview (04-code-comprehension/)
   ↓
2. Module Deep Dive (04-code-comprehension/modules/)
   ↓
3. Check Design Docs (03-design/)
   ↓
4. Read Related ADRs (02-adr/)
```

## Best Practices

### When Writing Docs

- **Use templates:** They ensure consistency
- **Link liberally:** Connect related documents
- **Be specific:** Reference actual code with `file.py:line`
- **Keep updated:** Review when code changes
- **Date your work:** Helps track relevance

### When Reading Docs

- **Check dates:** Older docs may be outdated
- **Verify code:** Docs can drift from implementation
- **Update if wrong:** Help the next person
- **Ask questions:** Better than staying confused

## Contributing

### Adding New Documents

1. Choose the right directory
2. Use the template if available
3. Follow naming conventions
4. Update the section's README if needed
5. Link from related documents

### Updating Existing Documents

1. Make your changes
2. Update the "Last Updated" date
3. Consider if status needs updating
4. Notify others if it's significant

## Need Help?

- **Can't find something?** Check [main README](./README.md)
- **Not sure which doc type?** See templates and examples
- **Something unclear?** Update the docs to help others
- **Big questions?** Bring to team meeting

## Next Steps

Choose your path:

- 🆕 **New dev?** → [08-onboarding/](./08-onboarding/)
- 🔨 **Building feature?** → [01-requirements/](./01-requirements/)
- 🔍 **Understanding code?** → [04-code-comprehension/](./04-code-comprehension/)
- 📚 **Learning architecture?** → [02-adr/](./02-adr/)

Happy documenting! 📝
