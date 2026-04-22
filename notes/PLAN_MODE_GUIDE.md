# Plan Mode Guide

## When to Use

- New feature implementation
- Multiple valid approaches
- Architectural decisions
- Multi-file changes
- Unclear requirements

## Setup

### Model Selection
- **Opus** for planning (better reasoning)
- **Sonnet** for coding (faster, cheaper)

### Extended Thinking
```
/config → config → Thinking Mode → Enter
```

### Effort Level
```
/effort → Low | Medium | High | Max
```
Higher = more tokens for analysis

### Ultraplan
```
/ultraplan <prompt>
```
Or use word "ultraplan" in prompt

## Workflow

1. Enter Plan Mode: `/plan` or `EnterPlanMode`
2. Explore codebase (Glob, Grep, Read)
3. Design approach
4. Present plan for approval
5. Exit with `ExitPlanMode`

## Tips

- Use `AskUserQuestion` for clarifying requirements
- Keep plans concrete with file paths
- Mark trade-offs explicitly
- Update plan if requirements change
