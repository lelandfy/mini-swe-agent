# Mini-SWE-Agent Codebase Overview

**Last Updated:** 2025-01-29

## High-Level Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    Entry Points                         │
│  (CLI scripts in src/minisweagent/run/)                 │
│  - mini.py: Interactive local use                       │
│  - github_issue.py: GitHub issue resolution             │
│  - swebench.py: Benchmark evaluation                    │
└────────────────┬────────────────────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────────────────────────┐
│                   Agent Layer                           │
│  (src/minisweagent/agents/)                             │
│  - DefaultAgent: Core agent loop                        │
│  - InteractiveAgent: User confirmation                  │
│  - TextualAgent: Visual TUI                             │
└─────┬─────────────────────────────┬─────────────────────┘
      │                             │
      ▼                             ▼
┌────────────────┐         ┌──────────────────┐
│  Model Layer   │         │ Environment Layer│
│  (models/)     │         │  (environments/) │
│  - LitellmModel│         │  - LocalEnv      │
│  - Anthropic   │         │  - DockerEnv     │
│  - TestModels  │         │  - SwerexDocker  │
└────────────────┘         └──────────────────┘
```

## Directory Structure

```
src/minisweagent/
├── __init__.py              # Package exports
├── agents/                  # Agent implementations
│   ├── default.py          # Core agent loop
│   ├── interactive.py      # Interactive mode
│   └── interactive_textual.py  # Visual TUI
├── models/                  # LLM integrations
│   ├── litellm.py          # Multi-provider via LiteLLM
│   ├── anthropic.py        # Direct Anthropic API
│   └── test_models.py      # Mock models for testing
├── environments/            # Command execution
│   ├── local.py            # Local shell execution
│   ├── docker.py           # Docker container execution
│   └── swerex_docker.py    # Docker with GUI support
├── run/                     # Entry point scripts
│   ├── mini.py             # Interactive CLI
│   ├── github_issue.py     # GitHub integration
│   └── extra/              # SWE-bench and utilities
└── config.py                # Configuration utilities
```

## Core Abstractions

### 1. Agent
**Purpose:** Orchestrates the LLM query → parse → execute → observe loop

**Key Methods:**
- `run(task: str) -> tuple[str, str]` - Main entry point
- `step()` - Single iteration of the loop
- `query()` - Get LLM response
- `parse_action()` - Extract command from LLM output
- `execute_action()` - Run command in environment
- `has_finished()` - Check for completion

**File:** src/minisweagent/agents/default.py:56-130

### 2. Model
**Purpose:** Abstract LLM API interactions

**Key Methods:**
- `query(messages: list[dict]) -> dict` - Send messages, get response

**Implementations:**
- `LitellmModel` - Multi-provider support (OpenAI, Anthropic, etc.)
- `AnthropicModel` - Direct Anthropic API with extended thinking
- `DeterministicModel` - Replays pre-recorded responses for testing

**Files:** src/minisweagent/models/

### 3. Environment
**Purpose:** Abstract command execution context

**Key Methods:**
- `execute(action: str) -> dict` - Run command, return output

**Implementations:**
- `LocalEnvironment` - Executes on host machine
- `DockerEnvironment` - Executes in isolated container
- `SwerexDockerEnvironment` - Docker with X11 for GUI apps

**Files:** src/minisweagent/environments/

## Data Flow

### Typical Execution
```
1. User provides task
   ↓
2. Agent initializes messages with system + task prompts
   ↓
3. Loop:
   a. Query LLM with message history
   b. Parse bash command from response
   c. Execute command in environment
   d. Check if task complete
   e. Add observation to messages
   ↓
4. Exit on completion, limits, or error
   ↓
5. Save trajectory to JSON file
```

### Message Format
```python
messages = [
    {"role": "system", "content": "You are a helpful assistant..."},
    {"role": "user", "content": "Task: Fix the bug in parser.py"},
    {"role": "assistant", "content": "THOUGHT: ...\n```bash\ncat parser.py\n```"},
    {"role": "user", "content": "<returncode>0</returncode>\n<output>...</output>"},
    # ... more iterations ...
]
```

## Key Files by Use Case

### Want to understand agent execution?
- src/minisweagent/agents/default.py:71-84 (run loop)
- dev-docs/03-design/agent-lifecycle/overview.md
- dev-docs/04-code-comprehension/flows/agent-execution-flow.md

### Want to add a new LLM provider?
- src/minisweagent/models/litellm.py (example)
- dev-docs/04-code-comprehension/modules/models.md

### Want to add a new environment?
- src/minisweagent/environments/local.py (simplest example)
- dev-docs/02-adr/0002-use-environment-abstraction.md

### Want to create a new entry point?
- src/minisweagent/run/mini.py (interactive example)
- src/minisweagent/run/github_issue.py (automation example)

## Configuration System

Configuration uses YAML files with Jinja2 templating:
- Config files in `.minisweagent/configs/` or built-in configs
- Templates use `{{variable}}` syntax
- Variables come from agent, model, and environment

**Example:**
```yaml
agent:
  system_template: "You are {{agent_role}}..."
  step_limit: 250
model:
  model_name: "claude-3-5-sonnet-20241022"
env:
  timeout: 60
```

## Testing Structure

```
tests/
├── agents/           # Agent behavior tests
├── models/           # Model integration tests
├── environments/     # Environment tests
├── run/              # Entry point tests
└── test_data/        # Test fixtures
```

## Common Patterns

### Config Dataclasses
Each major component has a `Config` dataclass:
```python
@dataclass
class AgentConfig:
    system_template: str = "..."
    step_limit: int = 0
    # ...
```

### Template Rendering
Jinja2 templates used throughout:
```python
Template(template_str).render(**variables)
```

### Exception-Based Control Flow
Termination via exceptions (see error-handling.md):
- `NonTerminatingException` - Recoverable
- `TerminatingException` - Exit loop

## Dependencies

**Core:**
- `litellm` - Multi-provider LLM API
- `jinja2` - Template rendering
- `typer` - CLI framework

**Optional:**
- `docker` - Container execution
- `anthropic` - Direct Anthropic API
- `textual` - TUI framework
- `prompt_toolkit` - Interactive prompts

## Next Steps

For deeper understanding, see:
- [Agents Module](./modules/agents.md)
- [Agent Execution Flow](./flows/agent-execution-flow.md)
- [Message Lifecycle](./flows/message-lifecycle.md)
