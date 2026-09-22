# AI Legal Platform Development

This repository is the control plane and durable development-process record for the AI Legal Platform.

It does **not** contain the platform's application source code. The platform repositories remain authoritative for their own implementation, service documentation, and verified service state.

## Purpose

This repository defines and tracks the development process that connects:

```
ChatGPT architecture / review
        |
        v
GitHub Issues / branches / pull requests
        |
        v
Coding agent / local implementation
        |
        v
Azure DevOps CI/CD
        |
        v
Existing self-hosted build/deployment environment
        |
        v
Build / test / deploy / runtime verification
        |
        v
Known-good GitHub release-candidate state
```

## Established Repository Rules

Across the platform repositories:

- GitHub is the source of truth for committed source and documentation.
- `chatgpt` is the primary working/development branch.
- `public` is the release-candidate / last-known-good ADO-mirrored branch.
- Changes are developed on `chatgpt), not directly on `public`.
- ADO remains the CI/CD and operational verification authority.
- A Git commit or successful build is not, by itself, proof of runtime completion.
- Verified operational state is mirrored back to `public`.
- Existing architectural rules and service documentation remain authoritative for platform behavior.
- Substantive implementation and architectural changes are reviewed before commitment.
- Documentation is updated from verified results and according to document ownership.
- Legacy `chatgpt.md` handoff documents are not authoritative.

## Current Process Effort

**Phase:** 0 — Development Workflow Design  
**Status:** IN PROGRESS

The immediate objective is to establish a repeatable, agent-assisted development process without disrupting the existing edge platform.

See:

- `PROJECT_STATUS.md` — current process state and recovery point
- `DEVELOPMENT_MODEL.md` — normal development lifecycle
- `ARCHITECTURE.md` — process architecture and system boundaries
- `DECISIONS.md` — durable decisions
- `ROADMAP.md` — implementation phases
- `agents/AGENTS.md` — coding-agent operating contract

## Scope Boundary

This repository governs **how we develop the platform**.

The platform repositories govern **what the platform is and how each service works**.

The first repositories covered by this process are:

- `edge-controller`
- `edge-time`
- `edge-gps`
- `edge-video`
- `edge-audio`

Additional repositories can be brought under the process without changing the established branch model.

## Cost Constraint

The process is designed to use the existing GitHub, Windows workstation, Azure DevOps, self-hosted agents, Raspberry Pi infrastructure, and local GPU resources. No paid CI/CD platform or hosted AI service is required for the base workflow.

Local model inference may be used where practical; model/service usage limits are documented separately rather than assumed away.
