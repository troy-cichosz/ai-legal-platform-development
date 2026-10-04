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

Aider was the first established framework evaluated, followed by Continue and OpenCode. Aider and Continue did not qualify from the common TASK-AIDER-001 end-to-end qualification. OpenCode + `gpt-oss:20b` passed TASK-AIDER-001 but failed TASK-PY-005 because its focused test did not demonstrate preservation of an actual temporal context and its completion report misstated repository state. OpenCode + `gpt-oss:20b` also failed TASK-PY-003 because the corrected run materialized only a partial production change (`self.live_process = None` after live-process exit and BrokenPipeError/OSError) and did not produce the required focused tests or demonstrate the required live-failure disable and authoritative-evidence continuation behavior. The agent also did not complete the required validation/reporting. No final framework or model default has been selected. These results remain historical end-to-end evidence and are not an overall ranking.

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

The common TASK-AIDER-001 qualification has been completed with Aider, Continue, and OpenCode using `gpt-oss:20b`. OpenCode passed TASK-AIDER-001, while Aider and Continue did not. OpenCode was then tested with TASK-PY-005 against the real `edge-video` repository baseline; the production change and tests were correct, but the qualification failed because the time-context test did not demonstrate preservation of an actual temporal context and the completion report misstated repository state. OpenCode was subsequently tested with TASK-PY-003 against the real `edge-video` repository baseline; the corrected run inspected the relevant implementation and materialized only a partial production change (`self.live_process = None` after live-process exit and BrokenPipeError/OSError), but did not produce the required focused tests or demonstrate the required live-failure disable and authoritative-evidence continuation behavior. The agent also did not complete the required validation/reporting, so the qualification failed. The OpenCode results remain qualification evidence for the tested configuration, not final framework or model selection. Independent review, deterministic controls, low-risk real-repository validation, and human acceptance remain required.

This framework-first path replaces the previous plan of continuing an indefinite sequence of bespoke-runner model benchmarks. Historical benchmark evidence remains retained in edge-ai BENCHMARK.md.

No failed benchmark implementation is to be repaired and promoted as a benchmark success. No benchmark implementation is to be promoted to chatgpt or public without the normal acceptance workflow.

### Current Development-Agent Qualification Checkpoint - 2026-10-04

The local development-agent qualification has now completed four additional controlled OpenCode tasks against disposable worktrees without promoting benchmark implementations to the authoritative repositories:

- **TASK-AGENT-002:** Passed with OpenCode + qwen3-coder:30b for a narrowly scoped Python implementation.
- **TASK-AGENT-003:** Passed with OpenCode + qwen3-coder:30b for a multi-file Python implementation and focused tests.
- **TASK-AGENT-004:** Passed / qualified as a controlled cross-service Capture Time Context contract task. Independent pristine-baseline testing subsequently confirmed that all five edge-time failures observed during the run were pre-existing. The agent's initial attribution lacked baseline evidence, so this remains a reporting-process caveat.
- **TASK-AGENT-005:** Passed as a repository-continuation/state-accuracy task. OpenCode made only the authorized `tests/test_evidence.py` change, passed the focused and complete edge-video tests, inspected the diff, and preserved the disposable Git boundary. A minor reporting overclaim about the prior existence of `.pytest_cache` was identified.

These results strengthen the evidence that the tested OpenCode configurations can perform scoped repository work, multi-file changes, cross-service contract work, continuation from existing project state, testing, diff inspection, and explicit verified/unverified reporting. They do not establish a final model or framework selection.

The next qualification stage is **TASK-AGENT-006: controlled planning and independent review**, with no implementation promotion. It is intended to test whether the local workflow can separate planning from implementation and independently verify compliance, scope, architecture, and validation claims before any change is accepted.

The authoritative recovery point remains this file plus `edge-ai/BENCHMARK.md`. The next chat should begin by reading this current project status, `edge-ai/BENCHMARK.md`, and the applicable repository rules before continuing TASK-AGENT-006.

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

### TASK-AGENT-006 Planning Checkpoint - 2026-10-04

TASK-AGENT-006 planning has completed as a planning-only local development-agent qualification. The planner inspected the disposable edge-video worktree and a supplied read-only snapshot of the edge-audio worktree, with no implementation promotion.

The planner successfully produced a repository-specific plan for a future common evidence-model and manifest integration across edge-video and edge-audio. It preserved the existing evidence, temporal, service-boundary, and immutability requirements and explicitly distinguished verified facts from recommendations and unavailable information.

The planner identified several decisions that must be resolved before implementation: the authoritative location/content of the common evidence contract, the required versus optional Capture Time Context fields, common-envelope versus service-specific manifest layering, audio versus video envelope persistence strategy, temporal-unavailability representation, and whether audio metadata atomicity is part of scope.

The initial planner attempt was correctly stopped when OpenCode denied access to the sibling edge-audio qualification worktree. A read-only inspection snapshot was then supplied inside the primary qualification worktree, and the planner was relaunched without broader access. This was a tooling/permission-boundary finding, not an implementation failure.

Result: **TASK-AGENT-006 planning PASS / QUALIFIED; implementation not approved.**

The immediate next step is **TASK-AGENT-006 independent review**. The reviewer must independently validate the plan against actual available repository evidence before any implementation work is authorized.

The authoritative recovery points remain this file and edge-ai/BENCHMARK.md.

### TASK-AGENT-006 Independent Review Checkpoint - 2026-10-04

The independent review of TASK-AGENT-006 planning has completed against the actual disposable edge-video worktree and the supplied read-only edge-audio inspection snapshot.

The review was substantively valid. An attempted raw GitHub retrieval of the plan returned HTTP 404, but `PLAN-AGENT-006.md` was already present at the root of the qualification worktree and the reviewer successfully read it. The expected prompted path had been `benchmark-inputs\\PLAN-AGENT-006.md`; the actual plan artifact available to the reviewer was at the worktree root. This path discrepancy is recorded as a process caveat.

The reviewer independently verified the planner's repository-specific findings, including the edge-video baseline, the existing but currently unused video evidence-envelope builder, the differing Capture Time Context validation requirements, audio temporal-unavailability handling, the absence of audio `integrity` and `temporal_provenance` fields, existing schema identifiers, and the controller/time-service boundary. The reviewer also confirmed that unavailable information was identified rather than invented.

One material planner overstatement was corrected: the plan stated that both services already emit `ai-legal.evidence.envelope.v1` envelopes. The inspected edge-video implementation contains `build_evidence_envelope()`, but that builder is not currently invoked for runtime persistence. The corrected plan must preserve that distinction.

**Independent-review disposition: APPROVE WITH REQUIRED PLAN CORRECTIONS — NOT READY FOR IMMEDIATE IMPLEMENTATION.**

The reviewer identified nine blockers/decisions that must be resolved before implementation:

1. canonical temporal-unavailable representation;
2. required versus optional Capture Time Context fields;
3. envelope persistence strategy (embedded versus sidecar);
4. controlled `time_semantics` vocabulary;
5. envelope v1 versioning/compatibility strategy;
6. authoritative location of the common contract;
7. intentionality of the current video envelope-builder deferral;
8. exact scope of audio metadata atomicity;
9. semantics of `capture.end`.

Required test-plan corrections include:

- a test proving manifest-byte preservation when an envelope sidecar is added;
- durability/failure testing for manifest and envelope writes, including both-or-neither semantics where applicable;
- cross-service compatibility tests based on a shared contract validator with per-service fixtures rather than sibling-service readers.

The reviewer verified that the qualification boundary remained intact: the detached baseline HEAD was unchanged, no implementation repository files were modified, no commit/push/branch mutation occurred, and only the review artifact was created.

Result: **TASK-AGENT-006 planning/review PASS as a qualification capability, with required plan corrections; implementation remains blocked.** No model, framework, or service implementation has been selected or promoted as a result.

The next step is explicit architectural/contract decision resolution and correction of the plan. No service repository, `edge-ai/DECISIONS.md`, or GitHub `public` branch should be modified for this checkpoint.
