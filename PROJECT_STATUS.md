# Project Status

**Repository:** `ai-legal-platform-development`  
**Current phase:** Phase 4 — GitHub → ADO Automation  
**Status:** IN PROGRESS  
**Last reviewed:** September 23, 2026

## Authoritative Sources

- **GitHub:** source of truth for committed source, documentation, issues, branches, and pull requests.
- **ADO:** CI/CD and operational verification authority.
- **ADO service-repository branches:** operational build inputs/mirrors only; their commit history is not authoritative.
- **Platform repositories:** authoritative for service implementation and service/project architecture.
- **Runtime verification:** authoritative for whether implemented behavior actually works.

## Branch Model

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

The reviewed baseline shows:

- `edge-controller`: generic management foundation is at maintenance-level status.
- `edge-time`: authoritative time foundation and Capture Time Context are operational.
- `edge-gps`: functional GNSS foundation exists, but context-provider integration is incomplete and development is paused.
- `edge-video`: operational MVP with local `edge-time` temporal integration complete and verified.
- `edge-audio`: working MVP with local `edge-time` integration complete and verified; calibration refinement remains active.

## Automation State

The approved automation architecture is:

```
GitHub service/chatgpt
        |
        | push webhook
        v
edge-platform-automation
        |
        | identify repository/ref/SHA
        | retrieve exact GitHub source
        | synchronize matching ADO service/chatgpt
        | create ADO synchronization commit
        v
ADO service/chatgpt
        |
        | existing branch-change trigger
        v
existing edge-<service> - CI/CD
        |
        v
build / scan / registry / deployment
        |
        v
runtime verification
```

### Increment B — Pilot Result

**Pilot:** `edge-gps`  
**Status:** VERIFIED END-TO-END

The pilot demonstrated:

- GitHub `edge-gps/chatgpt` push reaches the automation webhook pipeline.
- The automation identifies repository, `chatgpt` ref, and triggering SHA.
- Exact GitHub source is retrieved and verified.
- The complete source tree is synchronized into ADO `edge-gps/chatgpt`.
- Stale files are removed.
- The ADO synchronization commit is pushed and its remote SHA is verified.
- The existing `edge-gps - CI` triggers from the ADO branch change.
- Existing service CI/CD remains unchanged.
- GitHub `public` is not modified directly by `edge-platform-automation`.
- The service's legacy ADO pipeline was restored to GitHub `chatgpt` during the pilot because complete-tree synchronization correctly removes files absent from the authoritative source.

The pilot proves the synchronization mechanism and the preserved existing-CI trigger path. It does not yet establish the full five-service automation contract.

## Current Automation Increment

**Increment C — Expand and harden GitHub → ADO synchronization**  
**GitHub Issue:** #3

Scope:

- Expand from `edge-gps` to all five covered services.
- Verify each service's existing CI trigger.
- Verify complete-tree deletion propagation.
- Verify idempotent synchronization.
- Verify harmless no-op behavior for non-`chatgpt` pushes.
- Preserve GitHub SHA → ADO synchronization SHA correlation.
- Add preflight protection against missing required ADO service pipeline definitions.
- Keep existing service CI/CD definitions unchanged.

## Development-Agent / Local-AI State

The intended development loop is local-first:

- Windows 11 workstation is the preferred coding-agent host.
- RTX 3060 12 GB is available for local inference.
- Ollama is the planned local inference runtime.
- GitHub Issues provide durable task input.
- Coding agents inspect repositories, implement scoped issues, run local tests, review diffs, and commit to `chatgpt`.
- `edge-platform-automation` transports authoritative source into ADO.
- ADO performs build/deployment/operational verification.

Still to establish:

- coding-agent selection and repeatable operating procedure;
- Ollama model selection/resource policy;
- agent permissions and safety boundaries;
- standard issue/PR templates and labels;
- automated runtime/integration verification;
- exact controlled `chatgpt` → `public` promotion mechanism.

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
