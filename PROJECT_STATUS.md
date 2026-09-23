# Project Status

**Repository:** `ai-legal-platform-development`  
**Current phase:** Phase 0 — Development Workflow Design  
**Status:** IN PROGRESS  
**Last reviewed:** September 23, 2026

## Authoritative Sources

- **GitHub:** source of truth for committed source, documentation, issues, branches, and pull requests.
- **ADO:** CI/CD and operational verification authority.
- **ADO service-repository branches:** operational build inputs/mirrors only; their commit history is not authoritative.
- **Platform repositories:** authoritative for service implementation and service/project architecture.
- **Runtime verification:** authoritative for whether implemented behavior actually works.

## Branch Model

| Branch | Role |
|---|---|
| Location | Branch | Role |
|---|---|---|
| GitHub service repositories | `chatgpt` | Authoritative working/development branch and history |
| GitHub service repositories | `public` | Release candidate / last-known-good release state |
| ADO service repositories | `chatgpt` | Operational build-triggering mirror |

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

## ADO Pipeline Baseline Reviewed

The current `azure-pipelines.yaml` files in all five `public` branches have been reviewed.

The existing pipelines use a common self-hosted build → Docker build/push → Trivy/ADO artifacts → GitHub mirroring pattern. The current `Push` job creates an orphan branch and force-pushes it to GitHub.

The existing ADO configuration has now clarified the previously unknown pipeline variables:

- `branch2Push` is the GitHub `public` branch.
- `repoName` identifies the corresponding GitHub repository.
- `gitCommit` supplies the common commit message used when ADO updates `public`.
- Git/SSH files, keys, and related variables provide ADO's authenticated ability to push to GitHub; secret values are not required for the process inventory.
- Self-hosted ADO agent pools are the existing PIs and Linux/x86 systems.

The existing YAML remains useful as implementation reference, but it does not need to constrain the replacement CI/CD design.

## Current CI/CD Architecture

The approved migration preserves the existing service CI/CD:

```text
GitHub service/chatgpt
        |
        | push webhook
        v
edge-platform-automation - CI
        |
        | identify service + pull GitHub chatgpt source
        | synchronize matching ADO service/chatgpt
        | create ADO mirror commit
        v
ADO service/chatgpt
        |
        | existing branch-change trigger
        v
existing edge-<service> - CI/CD
        |
        | existing build / scan / registry / deployment
        v
existing GitHub public behavior
```

GitHub `chatgpt` history remains authoritative. ADO service `chatgpt` is an operational build mirror; its history does not need to match GitHub. Synchronization replaces the ADO working tree so GitHub deletions also remove stale ADO files.

The existing five service CI/CD definitions are not modified for this migration.

## Process Design State

Established:

- GitHub source-of-truth model.
- `chatgpt` working branch / `public` release-candidate model.
- ADO remains CI/CD and operational verification authority.
- Existing ADO self-hosted infrastructure should be reused.
- Current ADO pipeline YAML baseline reviewed from all five `public` branches.
- Existing `branch2Push`, `repoName`, and `gitCommit` semantics established.
- Existing GitHub authentication mechanism understood at a non-secret level.
- Windows 11 workstation with RTX 3060 12 GB is the primary local development/agent host.
- Local Ollama is an available candidate for free local inference.
- Docker Desktop is not a prerequisite for the process.
- New development-process repository established as this control plane.

Not yet established:

- Exact ADO pipeline definitions and GitHub service-hook/repository trigger configuration to use for the new workflow.
- Standard ADO pipeline template for development validation.
- Automated deployment/integration-test contract across the existing edge environment.
- Exact `chatgpt` → `public` promotion mechanism.
- Coding-agent selection and operating procedure.
- Local Ollama model selection and resource policy.
- Repository-wide GitHub Issue/PR templates and labels for agent work.

## Current Next Increment

**Increment B — GitHub → ADO service-repository synchronization**

Pilot: `edge-gps`.

Acceptance:

- GitHub `edge-gps/chatgpt` push reaches `edge-platform-automation - CI`.
- Automation identifies the service and triggering SHA.
- Automation retrieves the GitHub `chatgpt` source.
- Automation synchronizes the source tree into ADO `Docker/edge-gps` branch `chatgpt`.
- Deleted GitHub files are absent from the synchronized ADO working tree.
- An ADO synchronization commit is pushed.
- Existing `edge-gps - CI` triggers from that ADO branch change.
- Existing service CI/CD runs without modification.
- GitHub `chatgpt` remains authoritative.
- Automation does not directly modify GitHub `public`.

The existing service pipelines remain operational throughout this increment.

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
