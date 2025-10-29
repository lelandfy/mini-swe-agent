# PRD-001: Interactive Agent Mode

**Status:** Implemented
**Owner:** mini-swe-agent team
**Created:** 2024-08-01
**Last Updated:** 2024-08-21

## Summary

Enable users to interact with the agent in an interactive REPL-style interface where they can see each command before it executes and approve/modify/reject actions.

## Problem Statement

### Current Situation
The default agent runs autonomously without user oversight, which can be risky when making code changes or executing commands.

### Pain Points
- Users can't intervene if the agent is about to execute a problematic command
- No visibility into agent reasoning before actions execute
- Risk of unintended changes to codebase
- Difficult to guide the agent during execution

### Impact
- Users hesitant to use the agent on production code
- Learning curve for understanding agent behavior
- Need for post-execution rollback mechanisms

## Goals

### Primary Goals
1. Allow users to approve/reject each agent action before execution
2. Provide visibility into agent's reasoning process
3. Enable users to modify proposed commands
4. Support both interactive and autonomous modes

### Non-Goals
- Building a full IDE or code editor
- Supporting multi-agent coordination
- Implementing undo/redo functionality

## User Stories

### User Persona
**Who:** Developer working on their local codebase
**Needs:** To use AI assistance safely
**So that:** They can make changes without risking their code

### User Journeys

**Scenario 1:** Safe code modification
1. User runs `mini -t "fix the bug in parser.py"`
2. Agent analyzes and proposes: `sed -i 's/old/new/' parser.py`
3. User sees the command and reasoning
4. User approves → command executes
5. User sees output and continues

**Scenario 2:** Rejecting dangerous command
1. Agent proposes: `rm -rf tests/`
2. User sees command
3. User rejects with reason: "Don't delete tests"
4. Agent adjusts approach

## Requirements

### Functional Requirements
- **FR1:** Display agent's thought process before each action
- **FR2:** Show proposed command in syntax-highlighted format
- **FR3:** Provide options: Approve (y), Reject (n), Edit (e), Auto-approve rest (a)
- **FR4:** Allow command editing before execution
- **FR5:** Support both interactive and YOLO (no confirmation) modes via flag

### Non-Functional Requirements
- **Performance:** Command approval UI must respond instantly (<100ms)
- **Usability:** Clear, intuitive prompts and controls
- **Reliability:** Must handle Ctrl+C gracefully

### Constraints
- Must work in standard terminal environments
- Should support both simple TTY and rich terminal UIs

## Success Metrics

### Key Metrics
- **Adoption:** 70%+ of users use interactive mode over YOLO mode
- **Intervention Rate:** Users modify/reject commands in 10-20% of cases
- **User Feedback:** Positive sentiment about control and safety

### Acceptance Criteria
- [x] Agent shows reasoning before each command
- [x] User can approve/reject commands
- [x] User can edit commands before execution
- [x] `-y` flag enables autonomous mode
- [x] Ctrl+C exits gracefully
- [x] Works in both plain and rich terminal modes

## Dependencies

- **Depends on:** DefaultAgent base implementation
- **Blocks:** Visual mode (PRD-002)
- **Related to:** ADR-0003 (Agent message format)

## Timeline

- **Draft:** 2024-08-01
- **Approved:** 2024-08-05
- **Implemented:** 2024-08-21
- **Launched:** 2024-08-21

## References

- Implementation: `src/minisweagent/agents/interactive.py`
- User docs: `docs/usage/mini.md`
- Related ADR: `dev-docs/02-adr/0003-interactive-mode-design.md`
