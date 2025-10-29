# Agents Module Comprehension

**File:** src/minisweagent/agents/
**Last Updated:** 2025-01-29

## Purpose

The agents module implements the core agent loop that orchestrates LLM queries, action parsing, command execution, and observation handling.

## Module Structure

```
agents/
├── default.py              # Core DefaultAgent base class
├── interactive.py          # InteractiveAgent (user confirmation)
└── interactive_textual.py  # TextualAgent (visual TUI)
```

## Key Components

### DefaultAgent (default.py:56-130)

The base agent implementation. All other agents extend this.

**Responsibilities:**
- Manage message history
- Query LLM and handle responses
- Parse actions from LLM output
- Execute actions via environment
- Handle errors and termination

**Key Attributes:**
```python
self.config: AgentConfig       # Configuration
self.messages: list[dict]      # Message history
self.model: Model              # LLM interface
self.env: Environment          # Execution environment
self.extra_template_vars: dict # Additional template variables
```

**Key Methods:**

#### run(task: str) → tuple[str, str]
Main entry point. Runs until completion.

**Flow:**
1. Initialize messages with system + user prompts
2. Loop: call step() until TerminatingException
3. Catch NonTerminatingException → add to messages, continue
4. Catch TerminatingException → return (status, result)

**Location:** default.py:71-84

#### step() → dict
Single iteration: query → parse → execute → observe

**Location:** default.py:86-88

#### query() → dict
Query LLM and add response to messages.
Checks step/cost limits before querying.

**Location:** default.py:90-96

#### parse_action(response: dict) → dict
Extract bash command from LLM response.

**Regex:** `r"```bash\n(.*?)\n```"`
**Returns:** `{"action": "command", **response}`
**Raises:** `FormatError` if not exactly one code block

**Location:** default.py:105-110

#### execute_action(action: dict) → dict
Execute command via environment.

**Handles:**
- Timeout errors → raise ExecutionTimeoutError
- Completion check → raise Submitted if done

**Location:** default.py:112-123

#### has_finished(output: dict)
Check if output indicates completion.

**Completion markers:**
- `MINI_SWE_AGENT_FINAL_OUTPUT`
- `COMPLETE_TASK_AND_SUBMIT_FINAL_OUTPUT`

If found, raises `Submitted` with remaining output.

**Location:** default.py:125-129

### Exception Classes (default.py:32-54)

#### NonTerminatingException
Base for recoverable errors that allow retry.

**Subclasses:**
- `FormatError` - LLM output parsing failed
- `ExecutionTimeoutError` - Command timed out

#### TerminatingException
Base for terminal conditions that end execution.

**Subclasses:**
- `Submitted` - Task completed successfully
- `LimitsExceeded` - Reached step or cost limit

### AgentConfig (default.py:13-29)

Configuration dataclass for agent behavior.

**Key Fields:**
```python
system_template: str          # System prompt
instance_template: str        # Task prompt template
timeout_template: str         # Timeout error message
format_error_template: str    # Format error message
action_observation_template: str  # Observation formatting
step_limit: int = 0          # Max LLM calls (0 = unlimited)
cost_limit: float = 3.0      # Max cost in dollars
```

## InteractiveAgent (interactive.py)

Extends DefaultAgent to add user confirmation before each action.

**Additional Methods:**

#### parse_action(response: dict) → dict
Overrides parent to show action and get user approval.

**Options:**
- `y` - Yes, execute
- `n` - No, skip and explain
- `e` - Edit command before executing
- `a` - Auto-approve all remaining actions

**Modes:**
- `confirm` mode: Always ask
- `yolo` mode: Auto-approve all
- `whitelist` mode: Auto-approve whitelisted commands

## TextualAgent (interactive_textual.py)

Visual TUI version using the Textual framework.

**Features:**
- Split-pane interface
- Scrollable output
- Syntax highlighting
- Better visual formatting

## Usage Patterns

### Basic Usage
```python
from minisweagent import DefaultAgent, Model, Environment

agent = DefaultAgent(
    model=Model(...),
    env=Environment(...),
    system_template="...",
    step_limit=100,
    cost_limit=5.0
)

exit_status, result = agent.run("Fix the bug in parser.py")
```

### Interactive Usage
```python
from minisweagent.agents.interactive import InteractiveAgent

agent = InteractiveAgent(
    model=model,
    env=env,
    mode="confirm",  # or "yolo"
    **config
)

exit_status, result = agent.run(task)
```

### Custom Agent
```python
class CustomAgent(DefaultAgent):
    def parse_action(self, response: dict) -> dict:
        # Custom parsing logic
        action = super().parse_action(response)
        # Add custom processing
        return action

    def execute_action(self, action: dict) -> dict:
        # Custom execution logic
        print(f"Executing: {action['action']}")
        return super().execute_action(action)
```

## Common Gotchas

### Message History Growth
Messages accumulate throughout execution. Long-running agents can hit context limits.

**Mitigation:** Implement message pruning or summarization in custom agents.

### Template Variables
Templates have access to:
- Agent config vars (via `asdict(self.config)`)
- Environment vars (via `env.get_template_vars()`)
- Model vars (via `model.get_template_vars()`)
- Extra vars (via `self.extra_template_vars`)

### Exception Handling
Don't catch `TerminatingException` in custom agents - let them propagate to `run()`.

### Timeout Handling
`TimeoutError` and `subprocess.TimeoutExpired` are both caught and converted to `ExecutionTimeoutError`.

## Testing

**Test file:** tests/agents/test_default.py

**Test patterns:**
- Mock Model and Environment
- Use DeterministicModel for predictable responses
- Test exception handling with controlled failures

## Related Documentation

- [Agent Lifecycle Design](../../03-design/agent-lifecycle/overview.md)
- [Error Handling Design](../../03-design/agent-lifecycle/error-handling.md)
- [Agent Execution Flow](../flows/agent-execution-flow.md)
- [ADR-0002: Environment Abstraction](../../02-adr/0002-use-environment-abstraction.md)
