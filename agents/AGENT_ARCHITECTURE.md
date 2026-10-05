# Local Multi-Agent Architecture

## Purpose

The local AI environment is a reusable development capability for the AI Legal Platform.

It must support both:
- the `ai-legal-platform-development` control-plane repository; and
- the actual AI Legal Platform / edge service repositories.

The purpose is not to replace the project's architecture with whatever design a local model happens to prefer. The purpose is to provide local, repeatable implementation and independent verification while preserving the project's established goals, rules, and working behavior.

## Cost Boundary

The base environment must operate at **$0 incremental AI cost**.

- Ollama provides local model inference.
- Local agents use local models.
- ChatGPT Free may be used interactively for architecture and difficult reasoning.
- Local agents do not depend on the ChatGPT Free service as an API backend.
- The workflow does not require paid OpenAI API usage.
- The workflow does not require a paid hosted coding-agent subscription.

## Agent Configuration Boundary

The implementation path is:

```text
Human / ChatGPT
      |
      v
Task / Issue
      |
      v
Qualified Agent Framework
      |
      +--> repository rules
      +--> scoped tools
      +--> permission boundary
      +--> repository/worktree boundary
      |
      v
Local Ollama Model
      |
      v
Repository changes
      |
      v
Independent validation
```

The framework and model are replaceable components. The model is not the sole enforcement mechanism for repository or project rules.
## Roles

### Human

The human is the final authority for project goals, substantive architecture, operational environment, deployment confirmation, and final release/promotion decisions.

### ChatGPT

ChatGPT is the primary human-facing architecture and reasoning resource. Responsibilities include architecture, requirements, cross-service reasoning, difficult debugging, acceptance criteria, verification planning, review of agent-produced work, documentation/recovery planning, and resolving or escalating architectural questions.

ChatGPT Free is not an API dependency for the local environment.

### Coding Agent

The coding agent performs repository-scale implementation: read authoritative instructions, inspect actual source and branch state, implement the approved issue, run practical local tests, inspect the diff, keep changes scoped, report limitations, and commit/push only through the approved development workflow.

The coding agent must preserve existing working behavior and architecture unless the issue explicitly changes them. It must not redesign unrelated services, change evidence semantics or temporal authority without approval, introduce new cross-service contracts without escalation, or treat stylistic preference as justification for broad rewrites.

### Rules / Compliance Agent

The rules/compliance agent independently checks the coding agent's work.

It should inspect repository-local instructions, `agents/AGENTS.md`, `chatrules.md` and `projectrules.md` where applicable, issue scope and acceptance criteria, architecture and service boundaries, evidence/provenance rules, temporal authority, branch/workflow rules, documentation ownership, tests and verification expectations, and unexpected broad changes.

The compliance agent does not redesign the implementation merely to impose a preferred style. A compliance failure returns the work for correction or escalation. The coding agent cannot self-approve its own violation.

### Testing / Review Agents

Testing/review agents may independently execute or analyze tests, inspect diffs, compare behavior with acceptance criteria, identify regressions and unverified behavior, and evaluate generated test evidence.

These agents provide verification evidence; they do not independently change architecture or authorize release.

## Handoffs

The durable handoff is a repository artifact, normally a GitHub Issue plus the resulting branch/diff. A normal implementation handoff is:

```
ChatGPT / Human
      |
      v
GitHub Issue
      |
      v
Coding Agent
      |
      +----> implementation + tests + diff
      |
      v
Rules / Compliance Agent
      |
      +----> PASS --> Review / PR
      |
      +----> FAIL --> correction / escalation
```

The handoff must carry enough durable context that an agent does not need hidden conversational memory to understand the task.

## Authority and Escalation

Agent output is evidence, not authority.

Escalate rather than guess when a change appears to require a new cross-service contract, a change to evidence ownership or immutability, a change to temporal authority, a controller boundary change, a database architecture change, a deployment dependency, a branch/workflow rule change, or reinterpretation of the overall AI Legal Platform goal.

## Model Strategy

Model selection is role-specific. The coding role requires strong repository-scale coding and instruction-following capability. Compliance and review roles may use smaller models if they can reliably inspect the required context.

Model selection must be validated empirically on the real repositories. A model is not accepted solely because it fits within 12 GB of VRAM.

## Platform Reuse

The same local environment must eventually be usable against `edge-controller`, `edge-time`, `edge-gps`, `edge-video`, `edge-audio`, and future AI Legal Platform repositories.

The environment owns the development process. Each platform repository remains authoritative for its own implementation and service-specific rules.

## Required Validation

Before the local environment becomes the normal development path:

1. Validate the Ollama installation and local model inventory.
2. Qualify an established repository-oriented agent framework.
3. Qualify repository, tool, permission, and write boundaries.
4. Execute a minimal controlled repository task.
5. Validate representative Python implementation.
6. Validate multi-file and cross-repository work.
7. Run independent compliance and testing/review.
8. Review actual Git/filesystem state for unwanted scope expansion.
9. Verify tests, encoding, limitations, and unresolved behavior.
10. Commit only through the approved `chatgpt` workflow.
11. Confirm GitHub -> ADO automation remains unchanged and functional.
12. Repeat against an actual edge repository.
13. Document the operating procedure and model/resource policy.