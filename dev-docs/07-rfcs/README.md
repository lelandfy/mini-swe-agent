# Request for Comments (RFCs)

This directory contains RFCs for major changes that need team input and consensus.

## What is an RFC?

An RFC (Request for Comments) is a document proposing a significant change that:
- Affects multiple areas of the codebase
- Requires input from multiple stakeholders
- Needs consensus before implementation
- Has significant trade-offs to discuss

## RFC vs. ADR vs. Design Doc

- **RFC:** Proposal seeking feedback (before decision)
- **ADR:** Record of decision made (after decision)
- **Design Doc:** Technical implementation plan (after approval)

**Typical flow:** RFC → Discussion → ADR → Design Doc → Implementation

## When to Write an RFC

Write an RFC for:
- Major architectural changes
- Breaking changes to public APIs
- New features with significant complexity
- Changes affecting multiple teams
- Controversial proposals needing consensus

Don't write an RFC for:
- Small, localized changes
- Bug fixes
- Documentation updates
- Obvious improvements

## RFC Process

1. **Draft:** Author writes RFC using template
2. **Review:** Team reviews and provides feedback (async or in meeting)
3. **Discussion:** Address concerns, iterate on proposal
4. **Decision:** Accept, reject, or request more information
5. **Document:** If accepted, create ADR and design doc
6. **Implement:** Build it!

## Naming Convention

```
NNN-short-title.md
```

RFCs are numbered sequentially starting from 001.

Examples:
- `001-plugin-system.md`
- `002-multi-agent-coordination.md`
- `003-web-ui.md`

## RFC Status

- **Draft:** Still being written
- **Review:** Ready for feedback
- **Accepted:** Approved, ready for implementation
- **Rejected:** Not moving forward (document why)
- **Withdrawn:** Author decided not to pursue
- **Implemented:** Done! (link to ADR and implementation)

## Template

See [rfc-template.md](./rfc-template.md)

## Tips

- **Be clear:** What exactly are you proposing?
- **Explain why:** What problem does this solve?
- **Show alternatives:** What else did you consider?
- **Invite feedback:** Ask specific questions
- **Iterate:** RFCs can go through multiple revisions
- **Time-box:** Set a deadline for feedback
- **Document outcome:** Update status and link to ADR
