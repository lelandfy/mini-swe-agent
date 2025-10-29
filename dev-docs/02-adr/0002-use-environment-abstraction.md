# ADR-0002: Use Environment Abstraction Layer

**Status:** Accepted
**Date:** 2024-08-01
**Deciders:** mini-swe-agent team
**Related:** src/minisweagent/environments/

## Context

The agent needs to execute shell commands in different contexts:
- Local development environment (user's machine)
- Docker containers (for isolation)
- Remote environments (cloud VMs, SSH targets)

Each environment has different:
- Setup requirements (Python, dependencies)
- Isolation guarantees (filesystem, network)
- Performance characteristics (latency, throughput)
- Security considerations (sandbox, permissions)

We need a way to support multiple execution environments without coupling the agent logic to environment details.

## Decision

We will create an `Environment` abstraction that encapsulates command execution details.

**Key components:**
1. **Base `Environment` class** with `execute()` method
2. **Concrete implementations:**
   - `LocalEnvironment` - executes on host machine
   - `DockerEnvironment` - executes in Docker container
   - `SwerexDockerEnvironment` - specialized Docker with X11 support
3. **Configuration dataclass** for each environment type
4. **Consistent interface** for all environments

**Interface:**
```python
class Environment:
    def execute(self, action: str) -> dict[str, str]:
        """Execute command and return output dict."""
        pass
```

**Agent usage:**
```python
agent = DefaultAgent(model, env, **config)
# Agent doesn't know or care what environment it's using
```

## Alternatives Considered

### Option 1: Hard-code Docker execution
- **Pros:** Simpler initially, one less abstraction
- **Cons:** Can't support local dev, harder to test, inflexible
- **Why not chosen:** Need to support multiple environments

### Option 2: Strategy pattern with runtime switching
- **Pros:** Can switch environments mid-execution
- **Cons:** More complex, unclear use case, state management issues
- **Why not chosen:** No requirement to switch environments during run

### Option 3: Plugin system with dynamic loading
- **Pros:** Very extensible, third-party environments
- **Cons:** Over-engineered for current needs, complexity
- **Why not chosen:** YAGNI - can add later if needed

## Consequences

### Positive Consequences
- ✅ Agent code independent of execution environment
- ✅ Easy to add new environment types (SSH, cloud VMs)
- ✅ Simple to test with mock environments
- ✅ Users can choose environment via config
- ✅ Clear separation of concerns

### Negative Consequences
- ❌ Extra abstraction layer to understand
- ❌ Some environment-specific features harder to expose
- ❌ Overhead of maintaining multiple implementations

### Risks
- **Risk:** Abstraction leaks environment details
  - **Mitigation:** Careful interface design, thorough testing
- **Risk:** Performance overhead from abstraction
  - **Mitigation:** Minimal wrapper code, benchmark critical paths

## Implementation Notes

Environment implementations location:
- `src/minisweagent/environments/local.py`
- `src/minisweagent/environments/docker.py`
- `src/minisweagent/environments/swerex_docker.py`

Each environment:
1. Extends base `Environment` class
2. Has a `Config` dataclass for configuration
3. Implements `execute(action: str)` method
4. Handles setup/teardown in `__init__`/`__del__` or context manager

## References

- Implementation: `src/minisweagent/environments/`
- Usage example: `src/minisweagent/run/mini.py:81`
- Related: ADR-0003 (Docker container selection)
