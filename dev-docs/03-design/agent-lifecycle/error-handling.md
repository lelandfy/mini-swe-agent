# Agent Error Handling Design

**Status:** Implemented
**Author:** mini-swe-agent team
**Created:** 2024-08-01
**Last Updated:** 2025-01-29
**Related:** src/minisweagent/agents/default.py:32-54

## Overview

How the agent handles errors during execution, distinguishing between recoverable and terminal failures.

## Goals

- **Primary goal:** Graceful error handling with recovery
- **Secondary goals:** Clear error messages, proper cleanup
- **Non-goals:** Automatic bug fixing, infinite retry logic

## Exception Hierarchy

```
Exception
│
├── NonTerminatingException     # Recoverable errors
│   ├── FormatError            # LLM output parsing failed
│   └── ExecutionTimeoutError  # Command timed out
│
└── TerminatingException       # Terminal conditions
    ├── Submitted              # Agent completed successfully
    └── LimitsExceeded         # Reached step/cost limits
```

## Design Details

### Recoverable Errors (NonTerminatingException)

**Philosophy:** The agent should recover from temporary issues and try again.

#### FormatError
**Trigger:** LLM output doesn't contain exactly one bash code block
**Recovery:**
1. Raise `FormatError` with helpful message
2. Main loop catches it and adds to messages
3. LLM sees error and reformats next response

**Example:**
```
LLM: "Let me check the file and then edit it"
      (no code block)

Agent: FormatError("Please always provide EXACTLY ONE action...")

LLM: "```bash\ncat file.txt\n```"
```

#### ExecutionTimeoutError
**Trigger:** Command exceeds timeout limit
**Recovery:**
1. Kill the process
2. Capture partial output (if any)
3. Raise `ExecutionTimeoutError` with context
4. LLM sees error and tries different approach

**Example:**
```
Agent: Executing "sleep 300"
       (timeout after 60s)

Agent: ExecutionTimeoutError("Command timed out...")

LLM: "Let me try a different approach..."
```

### Terminal Conditions (TerminatingException)

**Philosophy:** These should stop execution immediately.

#### Submitted
**Trigger:** Command output starts with completion marker
**Behavior:**
1. Parse remaining output as final result
2. Raise `Submitted` with the result
3. Agent run loop exits
4. Return ("Submitted", result)

**Example:**
```python
def has_finished(self, output: dict[str, str]):
    lines = output.get("output", "").lstrip().splitlines(keepends=True)
    if lines and lines[0].strip() == "COMPLETE_TASK_AND_SUBMIT_FINAL_OUTPUT":
        raise Submitted("".join(lines[1:]))  # Rest is the result
```

#### LimitsExceeded
**Trigger:** Step or cost limit reached
**Check location:** Before each LLM query
**Behavior:**
1. Raise `LimitsExceeded`
2. Agent run loop exits
3. Return ("LimitsExceeded", "")

**Example:**
```python
def query(self):
    if 0 < self.config.step_limit <= self.model.n_calls:
        raise LimitsExceeded()
    if 0 < self.config.cost_limit <= self.model.cost:
        raise LimitsExceeded()
    # ... continue with query
```

## Error Message Design

### Format Error Template
```
Please always provide EXACTLY ONE action in triple backticks, found {{actions|length}} actions.

Please format your action in triple backticks as shown in <response_example>.

<response_example>
Here are some thoughts about why you want to perform the action.

```bash
<action>
```
</response_example>
```

**Key features:**
- Shows what went wrong (found N actions)
- Provides clear example of correct format
- Gives LLM template to follow

### Timeout Error Template
```
The last command <command>{{action['action']}}</command> timed out and has been killed.
The output of the command was:
 <output>
{{output}}
</output>
Please try another command and make sure to avoid those requiring interactive input.
```

**Key features:**
- Shows what command timed out
- Provides partial output if available
- Gives guidance (avoid interactive commands)

## Trade-offs

### Why Not Automatic Retry?
**Decision:** Errors are added to message history, letting LLM decide next step
**Reason:** LLM has context and can make better recovery decisions
**Trade-off:** Uses more tokens, but more flexible

### Why Two Exception Types?
**Decision:** Separate recoverable from terminal conditions
**Reason:** Different handling logic (continue vs. exit)
**Trade-off:** More classes, but clearer semantics

## Testing

Error handling tests in `tests/agents/test_default.py`:
- Mock LLM returns invalid format → FormatError
- Mock environment times out → ExecutionTimeoutError
- Mock output with completion marker → Submitted
- Exceed step/cost limits → LimitsExceeded

## References

- Implementation: src/minisweagent/agents/default.py:32-54, 115-123
- Related: [Agent lifecycle overview](./overview.md)
