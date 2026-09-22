# Project Status

**Repository:** `ai-legal-platform-development`  
**Current phase:** Phase 0 — Development Workflow Design  
**Status:** IN PROGRESS  
**Last reviewed:** September 2026

## Authoritative Sources

- **GitHub:** source of truth for committed source, documentation, issues, branches, and pull requests.
- **ADO:** CI/CD and operational verification authority.
- **Platform repositories:** authoritative for service implementation and service/project architecture.
- **Runtime verification:** authoritative for whether implemented behavior actually works.

## Branch Model

| Branch | Role |
|---|---|
| `chatgpt` | Primary working/development branch |
| `public` | Release candidate / last-known-good ADO-mirrored state |

Changes should be developed on `chatgpt`. `public` is not a development workspace.

## Existing Platform Baseline Reviewed

The current edge repositories are:

- `edge-controller`
- `edge-time`
- `edge-gps`
- `edge-video`
- `edge-audio`

All five currently expose both `chatgpt` and `public` branches.

The current platform documentation establishes:

- `edge-controller` as the generic management/policy plane.
- Local `edge-time` as the evidence-facing temporal service; it is not brokered through the controller.
- Host-addressed HTTP for cross-service communication; Docker service/container names are not used as cross-host endpoints.
- Local evidence ownership and preservation by evidence-producing services.
- Immutable finalized source evidence.
- Provenance, uncertainty, integrity, attestation, and temporal context as first-class evidence concerns.
- Implementation must be distinguished from verified runtime behavior.
- Documentation must be maintained according to ownership rather than copied indiscriminately.

## Current Verified Platform State

The reviewed `public` baseline shows:

- `edge-controller`: generic management foundation is at maintenance-level status.
- `edge-time`: authoritative time foundation and Capture Time Context are operational.
- `edge-gps`: functional GNSS foundation exists, but context-provider integration is incomplete and development is paused.
- `edge-video`: operational MVP with local `edge-time` temporal integration complete and verified.
- `edge-audio`: working MVP with local `edge-time` integration complete and verified; calibration refinement remains active.

The current platform direction is common evidence-model alignment across `edge-video` and `edge-audio`.

## Process Design State

Established:

- GitHub source-of-truth model.
- `chatgpt` working branch / `public` release-candidate model.
- ADO remains CI/CD authority.
- Existing ADO self-hosted infrastructure should be reused.
- Windows 11 workstation with RTX 3060 12 GB is the primary local development/agent host.
- Local Ollama is an available candidate for free local inference.
- Docker Desktop is not a prerequisite for the process.
- New development-process repository established as this control plane.

Not yet established:

- Existing ADO project/pipeline/agent-pool inventory.
- Exact GitHub → ADO trigger mechanism currently used by each pipeline.
- Standard ADO pipeline template for agent-created PR validation.
- Coding-agent selection and operating procedure.
- Local Ollama model selection and resource policy.
- Automated deployment/integration-test contract across the existing edge environment.
- Repository-wide GitHub Issue/PR templates and labels for agent work.

## Current Blocker

ADO environment inventory is required before changing CI/CD behavior.

The next implementation work is to document the existing ADO projects, pipelines, self-hosted agent pools, service connections, deployment targets, triggers, and verification steps.

## Recovery Rule

At the beginning of a new development increment:

1. Read this file.
2. Review the relevant platform repository rules.
3. Inspect the current repository state.
4. Establish the authoritative branch/baseline.
5. Confirm what is implemented versus verified.
6. Define the next increment and verification requirements.
7. Update this control plane when the process state materially changes.

This file is the primary recovery point for this development-process effort.
