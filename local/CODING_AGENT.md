# Coding Agent Evaluation

## Objective

Select and validate a local coding-agent environment that can operate against real GitHub repositories, follow repository instructions, edit code safely, run tests, review diffs, and work effectively with local inference.

The environment must be suitable for both this development-process repository and the actual AI Legal Platform / edge repositories. The objective is repository-scale implementation that preserves established architecture and project goals, not generic code generation or broad repository rewriting.

## Requirements

The candidate workflow should support:

- Windows 11;
- GitHub repositories;
- `chatgpt` working branch;
- GitHub Issues as task input;
- repository-local instruction files;
- test execution;
- Git diff inspection;
- controlled commits/pushes;
- no mandatory paid hosted service;
- local Ollama compatibility where practical.

## Qualification Unit

Evaluate the complete configuration:

```text
Agent Framework + Local Model + Repository Rules
+ Tool Permissions + Repository/Worktree Boundary
+ Independent Validation
```

Framework qualification precedes model optimization. Do not select a model based only on isolated coding output or raw runtime performance.
## Evaluation Criteria

Evaluate by documented behavior rather than popularity:

1. Repository inspection quality.
2. Instruction adherence.
3. Multi-file implementation reliability.
4. Test execution/reporting.
5. Diff quality.
6. Ability to recover from failed tests.
7. Context handling across a repository.
8. Local-model compatibility.
9. Windows operational fit.
10. Permission/safety controls.
11. Preservation of existing architecture and working behavior.
12. Resistance to broad unsolicited rewrites.
13. Ability to consume durable project rules and handoff artifacts.
14. Compatibility with an independent rules/compliance agent.
15. Reusability across the development-process repository and actual edge repositories.
16. Structured tool execution and reliable tool-result handling.
17. Adherence to configured write, command, and repository permissions.
18. Preservation of exact unrelated file content and encoding.
19. Accurate reporting of actual final Git/filesystem state.
20. Compatibility with independent repository validation and compliance review.

No agent is selected by this document.

The evaluation should use a small, low-risk real repository task after the ADO process is understood.
