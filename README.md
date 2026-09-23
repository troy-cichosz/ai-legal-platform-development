# AI Legal Platform Development

This repository is the control plane and durable development-process record for the AI Legal Platform.

It does **not** contain the platform's application source code. The platform repositories remain authoritative for their own implementation, service documentation, and verified service state.

## Purpose

This repository defines and tracks the development process that connects:

```
Human / ChatGPT architecture and review
        |
        v
GitHub Issues / branches / pull requests
        |
        v
Local coding agent + local AI inference
Windows workstation / Ollama
        |
        v
GitHub chatgpt
        |
        v
GitHub webhook
        |
        v
edge-platform-automation
        |
        v
ADO repository/chatgpt mirrors
        |
        +--> build_service: existing self-hosted ADO CI/CD
        |
        +--> sync_only: public-maintenance pipeline
        |
        v
Build / test / deploy / runtime verification
        |
        v
Verified release-candidate state
```

The intended process is local-first: repository implementation should be agent-assisted on the Windows workstation using local inference where practical, while the existing self-hosted ADO infrastructure performs reproducible build, deployment, and operational verification.

## Established Repository Rules

Across the platform repositories:

- GitHub is the source of truth for committed source and documentation.
- `chatgpt` is the primary working/development branch.
- `public` is the release-candidate / last-known-good branch.
- Changes are developed on `chatgpt`, not directly on `public`.
- ADO `chatgpt` branches are operational mirrors, not authoritative history.
- ADO remains the CI/CD and operational verification authority.
- A Git commit or successful build is not, by itself, proof of runtime completion.
- Verified operational state is eligible for controlled promotion to `public`.
- Existing architectural rules and service documentation remain authoritative for platform behavior.
- Substantive implementation and architectural changes are reviewed before commitment.
- Documentation is updated from verified results and according to document ownership.
- Legacy `chatgpt.md` handoff documents are not authoritative.

## Current Process Effort

**Phase:** 4 — GitHub → ADO Automation  
**Status:** IN PROGRESS

Increment B, using `edge-gps` as the pilot, demonstrated the end-to-end GitHub `chatgpt` → webhook → automation → ADO `chatgpt` → existing service CI/CD path.

Increment C now extends the automation registry to all seven current project repositories:

### Build/deploy services

- `edge-controller`
- `edge-time`
- `edge-gps`
- `edge-video`
- `edge-audio`

### Sync-only development/control repositories

- `ai-legal-platform-development`
- `edge-platform-automation`

The sync-only repositories receive ADO `chatgpt` mirrors and use lightweight public-maintenance pipelines rather than Docker build/deployment. The automation implementation is on `chatgpt`; ADO/runtime verification remains required before Increment C is considered complete.

See:

- `PROJECT_STATUS.md` — current process state and recovery point
- `DEVELOPMENT_MODEL.md` — normal development lifecycle and responsibilities
- `ARCHITECTURE.md` — process architecture and system boundaries
- `DECISIONS.md` — durable decisions
- `ROADMAP.md` — implementation phases
- `agents/AGENTS.md` — coding-agent operating contract
- `local/CODING_AGENT.md` — coding-agent evaluation
- `local/WINDOWS_WORKSTATION.md` — local workstation and Ollama baseline
- `ado/ADO_INTEGRATION.md` — ADO integration and automation contract

## Scope Boundary

This repository governs **how we develop the platform**.

The platform repositories govern **what the platform is and how each service works**.

The current repositories covered by this process are:

- `edge-controller`
- `edge-time`
- `edge-gps`
- `edge-video`
- `edge-audio`
- `ai-legal-platform-development`
- `edge-platform-automation`

Additional repositories can be brought under the process without changing the established branch model.

## Cost Constraint

The process is designed to use the existing GitHub, Windows workstation, Azure DevOps, self-hosted agents, Raspberry Pi infrastructure, and local GPU resources. No paid CI/CD platform or hosted AI service is required for the base workflow.

Local model inference is a planned part of the development workflow. Model selection and resource policy remain an implementation task rather than an assumption.
