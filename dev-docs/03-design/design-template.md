# [Feature/System Name] Design

**Status:** Draft | Review | Approved | Implemented
**Author:** [Your Name]
**Created:** YYYY-MM-DD
**Last Updated:** YYYY-MM-DD
**Related:** [Links to PRD, ADRs]

## Overview

Brief description of what you're designing and why.

## Goals

- **Primary goal:** What's the main objective?
- **Secondary goals:** What else should this accomplish?
- **Non-goals:** What's explicitly out of scope?

## Background

### Current State
How things work today (if applicable).

### Problem
What problem are we solving? What's the motivation?

### Requirements
Key requirements from the PRD or use cases:
1. Functional requirement 1
2. Functional requirement 2
3. Non-functional requirement (performance, security, etc.)

## Design Overview

High-level approach and architecture.

### Architecture Diagram
```
[Insert diagram - use ASCII art, mermaid, or link to image]

┌─────────────┐      ┌─────────────┐
│  Component  │─────▶│  Component  │
│      A      │      │      B      │
└─────────────┘      └─────────────┘
       │
       ▼
┌─────────────┐
│  Component  │
│      C      │
└─────────────┘
```

### Key Components

#### Component 1: [Name]
- **Responsibility:** What does it do?
- **Interface:** What's its API?
- **Dependencies:** What does it depend on?

#### Component 2: [Name]
- **Responsibility:** What does it do?
- **Interface:** What's its API?
- **Dependencies:** What does it depend on?

## Detailed Design

### Data Structures

```python
@dataclass
class ExampleData:
    """Description of this data structure."""
    field1: str
    field2: int
    field3: Optional[dict] = None
```

### APIs / Interfaces

```python
class ExampleComponent:
    def main_method(self, param: Type) -> ReturnType:
        """
        What does this method do?

        Args:
            param: Description

        Returns:
            Description of return value

        Raises:
            ExceptionType: When does this happen?
        """
        pass
```

### Algorithms

**Algorithm name:**
1. Step 1: Description
2. Step 2: Description
3. Step 3: Description

**Time complexity:** O(?)
**Space complexity:** O(?)

### Flow Diagrams

```
User Action → Component A → Component B → Result
     ↓
   Error? → Error Handler → Recovery
```

### State Management

How is state tracked? What's mutable? What's immutable?

### Error Handling

- What can go wrong?
- How do we handle errors?
- What's the recovery strategy?

## Alternatives Considered

### Alternative 1: [Name]
- **Description:** How would this work?
- **Pros:** What are advantages?
- **Cons:** What are disadvantages?
- **Decision:** Why not chosen?

### Alternative 2: [Name]
- **Description:** How would this work?
- **Pros:** What are advantages?
- **Cons:** What are disadvantages?
- **Decision:** Why not chosen?

## Trade-offs and Decisions

### Decision 1: [Topic]
**Choice:** What we decided
**Reason:** Why we chose this
**Trade-off:** What we're giving up

### Decision 2: [Topic]
**Choice:** What we decided
**Reason:** Why we chose this
**Trade-off:** What we're giving up

## Implementation Plan

### Phase 1: [Name]
- [ ] Task 1
- [ ] Task 2
- [ ] Task 3

### Phase 2: [Name]
- [ ] Task 1
- [ ] Task 2

### Testing Strategy
- Unit tests for components X, Y, Z
- Integration tests for flow A, B
- Edge cases to cover: ...

### Migration Plan
If changing existing systems:
1. Step 1
2. Step 2
3. Step 3

## Security Considerations

- What are the security implications?
- How do we protect sensitive data?
- What attack vectors exist?

## Performance Considerations

- Expected load/scale?
- Performance requirements?
- Bottlenecks to watch?
- Optimization strategies?

## Monitoring and Observability

- What metrics should we track?
- What logs are needed?
- What alerts should we set up?

## Open Questions

1. Question that needs resolution?
2. Trade-off that needs input?
3. Dependency that's unclear?

## Future Work

What's deferred to later:
- Enhancement 1
- Enhancement 2

## References

- [Link to PRD]
- [Link to ADR]
- [Link to prototype]
- [External documentation]
