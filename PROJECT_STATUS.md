# Project Status

**Repository:** `ai-legal-platform-development`  
**Current phase:** Phase 4 — GitHub → ADO Automation  
**Status:** VERIFIED — Increment C complete  
**Last reviewed:** September 24, 2026

## Authoritative Sources

- **GitHub:** source of truth for committed source, documentation, issues, branches, and pull requests.
- **ADO:** CI/CD and operational verification authority.
- **ADO service-repository branches:** operational build inputs/mirrors only; their commit history is not authoritative.
- **Platform repositories:** authoritative for service implementation and service/project architecture.
- **Runtime verification:** authoritative for whether implemented behavior actually works.

## Branch Model

| Location | Branch | Role |
|---|---|---|
| GitHub project repositories | `chatgpt` | Authoritative working/development branch and history |
| GitHub project repositories | `public` | Release candidate / last-known-good release state |
| ADO project repositories | `chatgpt` | Operational build/synchronization mirror |

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
GitHub repository/chatgpt
        |
        | push webhook
        v
edge-platform-automation
        |
        | identify repository/ref/SHA
        | retrieve exact GitHub source
        | synchronize matching ADO repository/chatgpt
        | create ADO synchronization commit
        v
ADO repository/chatgpt
        |
        +--> build_service: existing service CI/CD
        |
        +--> sync_only: public-maintenance pipeline
        |
        v
runtime / public maintenance
```

GitHub `chatgpt` remains authoritative. ADO `chatgpt` branches are operational mirrors.

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
- The service's required pipeline definition must exist in authoritative GitHub source because complete-tree synchronization removes files absent from GitHub.

### Increment C — Expansion and Hardening

**GitHub Issue:** #3  
**Status:** CORE SYNCHRONIZATION IMPLEMENTED AND END-TO-END RUNS VERIFIED

The automation registry now covers all seven current project repositories:

| Repository | Class | ADO mirror | Downstream behavior |
|---|---|---|---|
| `edge-controller` | `build_service` | `chatgpt` | Existing service CI/CD |
| `edge-time` | `build_service` | `chatgpt` | Existing service CI/CD |
| `edge-gps` | `build_service` | `chatgpt` | Existing service CI/CD |
| `edge-video` | `build_service` | `chatgpt` | Existing service CI/CD |
| `edge-audio` | `build_service` | `chatgpt` | Existing service CI/CD |
| `ai-legal-platform-development` | `sync_only` | `chatgpt` | Public-maintenance pipeline |
| `edge-platform-automation` | `sync_only` | `chatgpt` | Public-maintenance pipeline |

The implemented workflow now provides:

- complete-tree synchronization;
- exact GitHub SHA checkout and verification;
- matching ADO `chatgpt` repository synchronization;
- stale-file removal;
- remote ADO SHA verification;
- build-service preflight protection for required `azure-pipelines.yaml`;
- repository classification so sync-only repositories do not enter service Docker CI/CD;
- harmless non-`chatgpt` handling;
- an explicit recursion boundary because only `chatgpt` webhook events are accepted by the automation pipeline;
- existing service CI/CD definitions remain unchanged.

The expanded workflow has been validated across all seven covered repositories. The validation established the intended repository classifications, exact GitHub `chatgpt` SHA acceptance, ADO mirror targeting, idempotent behavior for unchanged source, complete-tree deletion propagation, harmless handling of non-`chatgpt` events, the `edge-platform-automation` recursion boundary, and GitHub SHA → ADO synchronization SHA correlation.

A controlled `edge-audio` `chatgpt` source change additionally verified the changed-source downstream path through ADO synchronization, the existing ADO CI/CD pipelines, and the resulting GitHub `public` maintenance.

The five `build_service` repositories were each exercised through the synchronization workflow. The changed-source downstream CI/CD path was explicitly exercised with `edge-audio`; the other service repositories remain on their unchanged existing CI/CD definitions.

Issue #3 / Increment C is therefore complete. Future work is tracked in Phase 5 and later roadmap phases.

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
- exact long-term controlled `chatgpt` → `public` promotion mechanism.

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
