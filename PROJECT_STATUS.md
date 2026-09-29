# Project Status

**Repository:** `ai-legal-platform-development`  
**Current phase:** Phase 3 - Local Multi-Agent AI Development Environment  
**Status:** Phase 4 automation complete; local multi-agent environment is the active next increment  
**Last reviewed:** September 28, 2026

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

### Increment B - Pilot Result

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

### Increment C - Expansion and Hardening

**GitHub Issue:** #3  
**Status:** CORE SYNCHRONIZATION IMPLEMENTED AND END-TO-END RUNS VERIFIED

The automation registry now covers all eight current project repositories:

| Repository | Class | ADO mirror | Downstream behavior |
|---|---|---|---|
| `edge-controller` | `build_service` | `chatgpt` | Existing service CI/CD |
| `edge-time` | `build_service` | `chatgpt` | Existing service CI/CD |
| `edge-gps` | `build_service` | `chatgpt` | Existing service CI/CD |
| `edge-video` | `build_service` | `chatgpt` | Existing service CI/CD |
| `edge-audio` | `build_service` | `chatgpt` | Existing service CI/CD |
| `ai-legal-platform-development` | `sync_only` | `chatgpt` | Public-maintenance pipeline |
| `edge-platform-automation` | `sync_only` | `chatgpt` | Public-maintenance pipeline |
| `edge-ai` | `sync_only` | `chatgpt` | Public-maintenance pipeline |

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

The expanded workflow has been validated across all eight covered repositories. The validation established the intended repository classifications, exact GitHub `chatgpt` SHA acceptance, ADO mirror targeting, idempotent behavior for unchanged source, complete-tree deletion propagation, harmless handling of non-`chatgpt` events, the `edge-platform-automation` recursion boundary, and GitHub SHA -> ADO synchronization SHA correlation.

A controlled `edge-audio` `chatgpt` source change additionally verified the changed-source downstream path through ADO synchronization, the existing ADO CI/CD pipelines, and the resulting GitHub `public` maintenance.

The five `build_service` repositories were each exercised through the synchronization workflow. The changed-source downstream CI/CD path was explicitly exercised with `edge-audio`; the other service repositories remain on their unchanged existing CI/CD definitions.

Issue #3 / Increment C is therefore complete. Future work is tracked in Phase 5 and later roadmap phases.

## Development-Agent / Local-AI State

The local-AI environment is the active development increment. The coding-agent framework evaluation has moved from the bespoke runner to established repository-oriented frameworks.

The immediate objective is not merely to benchmark individual models. It is to establish a practical local development agent that can continue the existing AI Legal Platform edge-platform work from the real repository and project state. The first capability target is reliable Python development with architecture-aware repository comprehension, testing/debugging, documentation discipline, and accurate validation reporting. Legal-AI workloads remain a later use of the same local AI environment.

### Target architecture

The workstation will host a reusable local multi-agent development environment using Ollama/local models. It must support both this control-plane repository and the actual AI Legal Platform / edge service repositories.

The coding model must be capable of repository-scale implementation while preserving the platform's established architecture, invariants, evidence model, temporal model, service boundaries, and overall project goal. Broad unsolicited rewrites are not acceptable.

The planned role separation is:

- **ChatGPT:** architecture, requirements, cross-service reasoning, difficult review, coordination, and human-facing design discussion.
- **Coding agent:** scoped implementation, local tests, diff inspection, and development-branch commits.
- **Rules/compliance agent:** independent checks of repository rules, architectural constraints, issue scope, invariants, and documentation requirements.
- **Testing/review agents:** independent test/result analysis and implementation review where useful.
- **Human:** architecture authority, environment control, operational confirmation, and final release decisions.
- **Ollama:** local inference layer for the local agents.

The rules/compliance layer is intentionally independent of the coding model so adherence is checked rather than assumed.

### Cost boundary

The base workflow must remain usable at **$0 incremental AI cost**:

- ChatGPT Free may be used interactively for architecture/reasoning.
- Local agents use Ollama/local models.
- The workflow must not require OpenAI API calls.
- The workflow must not require a paid hosted coding-agent subscription.

ChatGPT Free is not treated as a free external API endpoint for local agents.

### Current hardware baseline

- Windows 11 workstation.
- RTX 3060 12 GB.
- VS Code.
- Git/GitHub.
- Azure DevOps access.

### Coding-Agent Framework Evaluation

The local-AI environment is the active development increment. The substantive evaluation is now framework-first: use an established free/local repository-oriented coding-agent framework before expanding bespoke infrastructure.

Aider was the first established framework evaluated, followed by Continue and OpenCode. Aider and Continue did not qualify from the common TASK-AIDER-001 end-to-end qualification. OpenCode + `gpt-oss:20b` passed TASK-AIDER-001 but failed TASK-PY-005 because its focused test did not demonstrate preservation of an actual temporal context and its completion report misstated repository state. No final framework or model default has been selected. These results remain historical end-to-end evidence and are not an overall ranking.

The immediate objective is to establish a practical local development agent that can continue the existing AI Legal Platform edge-platform work from the real repository and project state. The workflow must preserve architecture, evidence and temporal models, service boundaries, documentation ownership, project intent, and the chatgpt -> ADO -> public release boundary.

### Target architecture

The workstation will host a reusable local development environment using Ollama/local models. It must support both this control-plane repository and the actual AI Legal Platform / edge repositories.

The role separation remains:

- **ChatGPT:** architecture, requirements, cross-service reasoning, difficult review, coordination, and verification planning.
- **Coding agent:** scoped implementation, local tests, diff inspection, and development-branch work.
- **Rules/compliance agent:** independent checks of repository rules, architectural constraints, issue scope, invariants, and documentation requirements.
- **Testing/review agents:** independent test/result analysis and implementation review where useful.
- **Human:** architecture authority, environment control, operational confirmation, and final release decisions.
- **Ollama:** local inference layer for the local agents.

### Framework-first qualification path

1. Survey established free/local candidates: Aider, Cline, Roo Code, Continue, OpenHands, and other suitable candidates discovered during evaluation.
2. Run a common small qualification task from clean disposable worktrees.
3. Record the complete end-to-end result and independently inspect the worktree before considering a framework qualified.
4. Select the practical framework configuration based on repository handling, tool reliability, validation support, control boundaries, and end-to-end task results.
5. Evaluate local models within the selected framework and establish resource policy.
6. Establish deterministic repository/branch/permission/release controls and independent compliance/testing review.
7. Validate the accepted workflow against a low-risk real edge-repository task.
8. Human acceptance is required before treating the environment as operational.

The common TASK-AIDER-001 qualification has been completed with Aider, Continue, and OpenCode using `gpt-oss:20b`. OpenCode passed TASK-AIDER-001, while Aider and Continue did not. OpenCode was then tested with TASK-PY-005 against the real `edge-video` repository baseline; the production change and tests were correct, but the qualification failed because the time-context test did not demonstrate preservation of an actual temporal context and the completion report misstated repository state. The OpenCode result remains qualification evidence for the tested configuration, not final framework or model selection. Independent review, deterministic controls, low-risk real-repository validation, and human acceptance remain required.

This framework-first path replaces the previous plan of continuing an indefinite sequence of bespoke-runner model benchmarks. Historical benchmark evidence remains retained in edge-ai BENCHMARK.md.

No failed benchmark implementation is to be repaired and promoted as a benchmark success. No benchmark implementation is to be promoted to chatgpt or public without the normal acceptance workflow.

### Still to establish

- model-to-role/resource policy;
- agent permissions and safety boundaries;
- durable agent handoff format;
- independent rules/compliance gate;
- independent testing/review stage;
- low-risk accepted real-repository validation;
- reuse of the accepted environment against the actual edge repositories.

Automated runtime/integration verification and exact long-term controlled `chatgpt` -> `public` promotion remain later process work.

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