# Agent Lifecycle Design

**Status:** Implemented
**Author:** mini-swe-agent team
**Created:** 2024-08-01
**Last Updated:** 2025-01-29
**Related:** src/minisweagent/agents/default.py, ADR-0002

## Overview

This document describes the execution lifecycle of the DefaultAgent, from initialization through task completion.

## Goals

- **Primary goal:** Define clear agent execution flow
- **Secondary goals:** Handle errors gracefully, support interruption
- **Non-goals:** Multi-agent coordination, agent persistence

## Background

### Current State
The agent follows a simple query-execute-observe loop until termination.

### Problem
Need to understand:
- When and how does the agent stop?
- How are errors handled?
- What's the message flow?

## Design Overview

### Agent State Machine

```
┌──────────┐
│   Init   │
└────┬─────┘
     │
     ▼
┌──────────────────────────────────────┐
│          Main Loop (run)             │
│                                      │
│  ┌────────────────────────────────┐  │
│  │  Query LLM                     │  │
│  │    ↓                           │  │
│  │  Parse Action                  │  │
│  │    ↓                           │  │
│  │  Execute Command               │  │
│  │    ↓                           │  │
│  │  Check Completion              │  │
│  │    ↓                           │  │
│  │  Add Observation               │  │
│  └────────────────────────────────┘  │
│         │                            │
│    ┌────┴────────┬─────────────┐     │
│    ▼             ▼             ▼     │
│  Error      Submitted    Limit       │
│  Handle    (Complete)   Exceeded     │
└────┬─────────────┬─────────────┬──────┘
     │             │             │
     └─────────────┴─────────────┘
                   │
                   ▼
            ┌──────────┐
            │   Exit   │
            └──────────┘
```

### Key Components

#### DefaultAgent
- **Responsibility:** Orchestrates LLM queries and command execution
- **Interface:** `run(task: str) -> tuple[str, str]`
- **Dependencies:** Model, Environment

#### Exception Hierarchy
- **NonTerminatingException:** Recoverable errors (format errors, timeouts)
- **TerminatingException:** End conditions (submission, limits exceeded)

## Detailed Design

### Main Loop (run method)

```python
def run(self, task: str) -> tuple[str, str]:
    # Initialize
    self.messages = []
    self.add_message("system", system_prompt)
    self.add_message("user", task_prompt)

    # Loop until termination
    while True:
        try:
            self.step()  # Query + Execute + Observe
        except NonTerminatingException as e:
            # Recoverable error - add to messages and continue
            self.add_message("user", str(e))
        except TerminatingException as e:
            # Terminal condition - return status and result
            self.add_message("user", str(e))
            return type(e).__name__, str(e)
```

### Step Execution

Each step consists of:
1. **Query:** Get LLM response (check limits first)
2. **Parse:** Extract bash command from response
3. **Execute:** Run command in environment
4. **Observe:** Format output and add to messages

### Termination Conditions

| Condition | Trigger | Exception | Behavior |
|-----------|---------|-----------|----------|
| Explicit completion | Command output starts with `COMPLETE_TASK_AND_SUBMIT_FINAL_OUTPUT` | `Submitted` | Return success with diff |
| Step limit | `model.n_calls >= config.step_limit` | `LimitsExceeded` | Return failure |
| Cost limit | `model.cost >= config.cost_limit` | `LimitsExceeded` | Return failure |

### Error Handling

| Error | Exception | Recovery |
|-------|-----------|----------|
| Malformed LLM output | `FormatError` | Add error message, retry |
| Command timeout | `ExecutionTimeoutError` | Add error message, retry |
| Other errors | Caught at run-script level | Return error status |

## Implementation Notes

### File Structure
- `src/minisweagent/agents/default.py:56-130` - Core agent logic
- `src/minisweagent/agents/interactive.py` - Interactive extension
- `src/minisweagent/agents/interactive_textual.py` - Visual UI extension

### Message Flow
1. System message (instructions)
2. User message (task description)
3. Loop:
   - Assistant message (LLM response)
   - User message (observation or error)
4. Final user message (termination reason)

## Testing Strategy

- Unit tests for each step method
- Integration tests for full run cycles
- Edge cases: format errors, timeouts, limits
- Mock environments for deterministic testing

## References

- Implementation: src/minisweagent/agents/default.py:71-84
- Related design: [Error handling](./error-handling.md)
- Related ADR: ADR-0002 (Environment abstraction)
