# Code Walkthrough

This guide provides a tour of the mini-swe-agent codebase for new developers.

## Quick Overview

mini-swe-agent is an autonomous coding agent that uses LLMs to solve programming tasks by executing shell commands.

**Core loop:**
```
LLM generates command → Parse → Execute in environment → Observe result → Repeat
```

## Directory Tour

### src/minisweagent/

The main package. Everything lives here.

```
src/minisweagent/
├── __init__.py              # Package exports
├── agents/                  # ⭐ Start here - the agent loop
├── models/                  # LLM integrations
├── environments/            # Command execution
├── run/                     # Entry point scripts
└── config.py                # Configuration utilities
```

### Key Files to Understand

#### 1. `agents/default.py` (150 lines)
**The heart of the system.** Read this first!

**Key parts:**
- Lines 13-29: `AgentConfig` - configuration options
- Lines 32-54: Exception classes for control flow
- Lines 71-84: `run()` method - the main loop
- Lines 86-88: `step()` - single iteration
- Lines 105-110: `parse_action()` - extract bash commands
- Lines 112-123: `execute_action()` - run commands
- Lines 125-129: `has_finished()` - check completion

**Read this to understand:** How the agent works end-to-end.

#### 2. `agents/interactive.py` (~200 lines)
Extends DefaultAgent to add user confirmation.

**Key method:**
- `parse_action()` - overridden to show command and get approval

**Read this to understand:** How user interaction is added.

#### 3. `models/litellm.py` (~100 lines)
LLM API integration using LiteLLM.

**Key methods:**
- `query()` - send messages, get response
- Cost tracking logic

**Read this to understand:** How we call LLM APIs.

#### 4. `environments/local.py` (~100 lines)
Execute commands on the host machine.

**Key method:**
- `execute()` - run bash command, return output

**Read this to understand:** How commands are executed.

#### 5. `run/mini.py` (~100 lines)
The interactive CLI entry point.

**Read this to understand:** How all pieces connect.

## Trace a Complete Execution

Let's trace what happens when you run: `mini -t "list files"`

### 1. Entry Point (run/mini.py:91)
```python
exit_status, result = agent.run(task)
```

### 2. Agent Initialization (agents/default.py:73-76)
```python
self.messages = []
self.add_message("system", system_prompt)
self.add_message("user", "Your task: list files...")
```

Messages now:
```python
[
  {"role": "system", "content": "You are a helpful assistant..."},
  {"role": "user", "content": "Your task: list files..."}
]
```

### 3. Main Loop Starts (agents/default.py:77-84)
```python
while True:
    try:
        self.step()
    except NonTerminatingException as e:
        self.add_message("user", str(e))
    except TerminatingException as e:
        return type(e).__name__, str(e)
```

### 4. Step 1: Query LLM (agents/default.py:86-88, 90-96)
```python
def step(self):
    return self.get_observation(self.query())
```

Query calls model:
```python
response = self.model.query(self.messages)
# response = {"content": "THOUGHT: ...\n```bash\nls -la\n```"}
self.add_message("assistant", **response)
```

Messages now have assistant response.

### 5. Step 2: Parse Action (agents/default.py:105-110)
```python
actions = re.findall(r"```bash\n(.*?)\n```", response["content"])
# actions = ["ls -la"]
return {"action": "ls -la", **response}
```

### 6. Step 3: Execute (agents/default.py:112-123)
```python
output = self.env.execute("ls -la")
# output = {"returncode": 0, "output": "total 48\ndrwxr-xr-x..."}
```

Environment calls subprocess:
```python
# In environments/local.py
result = subprocess.run(
    ["bash", "-c", "ls -la"],
    capture_output=True,
    timeout=60
)
```

### 7. Step 4: Check Completion (agents/default.py:122)
```python
self.has_finished(output)
# Not finished yet, continues
```

### 8. Step 5: Add Observation (agents/default.py:101-102)
```python
observation = "Observation: <returncode>0</returncode>\n<output>total 48...</output>"
self.add_message("user", observation)
```

### 9. Loop Back to Step 1
Agent queries LLM again with updated history, and loop continues...

### 10. Eventually: Completion
Agent runs command that outputs `COMPLETE_TASK_AND_SUBMIT_FINAL_OUTPUT`, triggering:
```python
raise Submitted("final output here")
```

This bubbles up to main loop, which returns:
```python
return "Submitted", "final output here"
```

## Data Structures

### Message Format
```python
{
    "role": "system" | "user" | "assistant",
    "content": str,
    # Optional fields depend on role and model
}
```

### Environment Output
```python
{
    "returncode": int,
    "output": str,  # Combined stdout + stderr
}
```

### Agent Config
```python
@dataclass
class AgentConfig:
    system_template: str       # System prompt
    instance_template: str     # Task prompt
    step_limit: int = 0       # Max LLM calls
    cost_limit: float = 3.0   # Max cost
    # ... more template strings
```

## Common Patterns

### 1. Jinja2 Templating
Templates everywhere:
```python
Template(template_str).render(**variables)
```

Variables come from config, environment, model.

### 2. Dataclass Configs
Every component has a Config dataclass:
```python
@dataclass
class ComponentConfig:
    field1: str = "default"
    field2: int = 0
```

### 3. Exception-Based Control Flow
Errors and termination via exceptions:
```python
raise NonTerminatingException  # Recoverable
raise TerminatingException     # Exit
```

### 4. Dependency Injection
Agent receives dependencies:
```python
agent = DefaultAgent(model, env, **config)
```

## Testing

Tests mirror the source structure:
```
tests/
├── agents/           # Test agent behavior
├── models/           # Test model integration
├── environments/     # Test environments
└── run/              # Test entry points
```

**Key test file:** `tests/agents/test_default.py`

Uses mocks and DeterministicModel for predictable testing.

## Next Steps

Now that you've seen the big picture:

1. **Run the code:** Set up environment and run `mini`
2. **Read key files:** Start with `agents/default.py`
3. **Make a change:** Try adding a log message
4. **Run tests:** `pytest tests/`
5. **Read more docs:** Check `../04-code-comprehension/`

## Questions?

- What's still unclear?
- What surprised you?
- What would help the next person?

Update these docs with what you learn!
