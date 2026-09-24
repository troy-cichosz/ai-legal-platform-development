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
Local multi-agent development environment
Windows workstation / Ollama
        |
        +--> coding agent
        +--> independent rules/compliance review
        +--> testing/review agents
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

The intended process is local-first: repository implementation should be performed by a controlled local multi-agent environment using local inference where practical, while the existing self-hosted ADO infrastructure performs reproducible build, deployment, and operational verification.

The local AI environment is intended to serve both this development-process repository and the actual AI Legal Platform / edge service repositories. It must therefore be designed as a reusable engineering environment, not as a one-off tool for this control-plane project.

The local coding model must be capable of repository-scale implementation suitable for this project while preserving established architecture and working behavior. Broad unsolicited rewrites are not acceptable. Independent rules/compliance review is a required control so correctness does not depend solely on the coding model following its own instructions.

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
**Status:** VERIFIED — Increment C complete

Increment B, using `edge-gps` as the pilot, demonstrated the end-to-end GitHub `chatgpt` → webhook → automation → ADO `chatgpt` → existing service CI/CD path.

Increment C expanded and hardened the automation registry across all seven current project repositories. The synchronization path has now been validated across the full registry, including exact-SHA synchronization, idempotence, complete-tree deletion propagation, non-`chatgpt` no-op handling, recursion-boundary behavior, and GitHub SHA → ADO synchronization SHA correlation.

A controlled `edge-audio` `chatgpt` change also verified the changed-source downstream path through ADO synchronization, existing CI/CD, and the resulting GitHub `public` maintenance.

The automation increment is complete. Future work moves to the later process phases for automated integration verification and controlled release-candidate promotion.

See:

- `PROJECT_STATUS.md` — current process state and recovery point
- `DEVELOPMENT_MODEL.md` — normal development lifecycle and responsibilities
- `ARCHITECTURE.md` — process architecture and system boundaries
- `DECISIONS.md` — durable decisions
- `ROADMAP.md` — implementation phases
- `agents/AGENTS.md` — coding-agent operating contract
- `agents/AGENT_ARCHITECTURE.md` — local multi-agent roles, boundaries, and handoffs
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

The process is designed to use the existing GitHub, Windows workstation, Azure DevOps, self-hosted agents, Raspberry Pi infrastructure, and local GPU resources. No paid CI/CD platform or hosted AI service is required for the base workflow. The base AI development environment must not depend on paid OpenAI API usage or a paid hosted coding-agent subscription.

ChatGPT Free remains the human-facing architecture/reasoning resource. It is not treated as a free API backend for local agents. Local autonomous development uses Ollama/local models.

Local model selection, model-to-role assignment, and resource policy are now the immediate implementation tasks. The selected local environment must remain reusable for the actual AI Legal Platform / edge repositories.
