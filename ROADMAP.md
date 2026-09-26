# Development Process Roadmap

## Phase 0 - Development Workflow Design

**Status:** COMPLETE

- [x] Create process control-plane repository.
- [x] Establish `chatgpt` working / `public` release-candidate model.
- [x] Review current edge repository rules and status.
- [x] Record current platform baseline.
- [x] Review current `azure-pipelines.yaml` files from all five `public` branches.
- [x] Establish current `branch2Push` = `public` behavior.
- [x] Establish current `repoName` and `gitCommit` semantics.
- [x] Establish existing ADO self-hosted agent-pool scope.
- [x] Establish existing ADO -> GitHub authentication purpose without retrieving secrets.
- [x] Decide that existing pipeline YAML is reference material rather than a required replacement design.
- [x] Define GitHub webhook -> `edge-platform-automation` trigger model.
- [x] Define GitHub `chatgpt` -> ADO service-repository synchronization model.
- [x] Define ADO service `chatgpt` as an operational build-triggering mirror, not authoritative history.
- [x] Verify the first end-to-end synchronization and existing service CI trigger with `edge-gps`.
- [x] Establish the local-first coding-agent/Ollama direction as part of the target development workflow.

## Phase 1 - ADO Integration Mapping

**Status:** BASELINE COMPLETE; deeper replacement work deferred until automation expansion is proven.

- [x] Document ADO project and covered service mappings.
- [x] Document existing self-hosted agent-pool scope.
- [x] Document existing registry/scanning/build mechanics.
- [x] Document current deployment/verification responsibilities at the process level.
- [ ] Define the minimum CI validation contract for changes on `chatgpt`.
- [ ] Define promotion verification required before `public`.
- [ ] Define any replacement pipeline contract needed after the existing service CI/CD is no longer sufficient.

## Phase 2 - GitHub Development Standard

- [ ] Standardize issue structure.
- [ ] Standardize agent task instructions.
- [ ] Standardize PR requirements.
- [ ] Establish repository-specific agent instruction discovery.
- [ ] Establish labels/milestones appropriate to development-process tracking.

## Phase 3 - Local Multi-Agent AI Development Environment

**Status:** NEXT ACTIVE INCREMENT

### Architecture and governance

- [ ] Define the reusable local multi-agent architecture.
- [ ] Define distinct roles for coding, rules/compliance, testing/review, and architecture/coordination.
- [ ] Define explicit permissions and boundaries for each role.
- [ ] Define durable handoff artifacts and required context between roles.
- [ ] Define disagreement and escalation handling.
- [ ] Ensure the coding agent cannot silently redefine platform architecture or established project goals.

### Local inference and tooling

- [ ] Evaluate free/local coding-agent options against the real GitHub/ADO workflow.
- [x] Ollama installation exists on the Windows workstation; verification is the next action.
- [ ] Verify the installed Ollama version and local model inventory.
- [ ] Establish the workstation/GPU resource baseline.
- [ ] Evaluate local model suitability on RTX 3060 12 GB.
- [ ] Define model-to-role and resource policy.
- [ ] Establish controlled local agent execution.
- [ ] Establish repeatable Windows workstation setup.

### Validation

- [ ] Validate repository instruction discovery and adherence.
- [ ] Validate implementation quality on a low-risk real repository change.
- [ ] Validate independent rules/compliance review against the same change.
- [ ] Validate local test execution and diff review.
- [ ] Validate the complete Issue -> local agents -> GitHub `chatgpt` workflow.
- [ ] Validate the same environment against an actual edge service repository.
- [ ] Confirm the environment preserves the established AI Legal Platform / edge architecture and does not introduce broad unsolicited rewrites.
- [ ] Document the resulting operating procedure.

## Phase 4 - GitHub -> ADO Automation

### Increment B - `edge-gps` Pilot

**Status:** VERIFIED END-TO-END

- [x] Trigger `edge-platform-automation` from GitHub `chatgpt` changes.
- [x] Identify supported repository/ref/SHA.
- [x] Retrieve and verify exact GitHub source.
- [x] Synchronize complete source tree into matching ADO `chatgpt`.
- [x] Remove stale files.
- [x] Create/push and verify ADO synchronization commit.
- [x] Confirm synchronized ADO `chatgpt` commit triggers existing `edge-gps - CI`.
- [x] Preserve existing service CI/CD definitions.
- [x] Record GitHub -> ADO revision correlation.

### Increment C - Expand and Harden

**Status:** COMPLETE - VALIDATED

**Build/deploy services:**

- `edge-controller`
- `edge-time`
- `edge-gps`
- `edge-video`
- `edge-audio`

**Sync-only development/control repositories:**

- `ai-legal-platform-development`
- `edge-platform-automation`

Completed implementation/verification:

- [x] Expand synchronization registry to all seven repositories.
- [x] Add both sync-only repositories to the automation registry.
- [x] Introduce `build_service` / `sync_only` classification.
- [x] Establish an ADO `chatgpt` mirror for each covered repository through the synchronization workflow.
- [x] Keep build-service repositories on their existing service CI/CD path.
- [x] Add preflight protection for required build-service `azure-pipelines.yaml`.
- [x] Define sync-only downstream public-maintenance behavior.
- [x] Establish explicit recursion protection for `edge-platform-automation`.
- [x] Preserve complete-tree synchronization behavior.
- [x] Preserve GitHub SHA -> ADO synchronization SHA correlation.
- [x] Verify successful controlled `chatgpt` runs across the covered repositories.
- [x] Preserve harmless handling of non-`chatgpt` events in the automation definition.

Validated outcomes:

- [x] Complete-tree deletion propagation.
- [x] Idempotent synchronization of unchanged source.
- [x] Non-`chatgpt` webhook no-op execution.
- [x] `edge-platform-automation` recursion-boundary execution.
- [x] GitHub SHA -> ADO SHA correlation across the covered repositories.
- [x] Final documentation audit and closure of Issue #3.

A controlled `edge-audio` `chatgpt` source change also verified the changed-source downstream path through synchronization, the existing ADO CI/CD pipelines, and GitHub `public` maintenance.

## Phase 5 - Automated Integration Verification

- [ ] Define a common verification contract with service-specific checks.
- [ ] Deploy approved artifacts to test nodes.
- [ ] Run service health checks.
- [ ] Run cross-service integration checks.
- [ ] Run relevant evidence/time/provenance checks.
- [ ] Run failure/recovery checks where required.
- [ ] Record verification results in the appropriate system.
- [ ] Make verification results usable as a promotion gate.

## Phase 6 - Release-Candidate Promotion

- [ ] Define exact `chatgpt` -> `public` promotion mechanism.
- [ ] Ensure only verified state reaches `public`.
- [ ] Keep `public` synchronized with the verified known-good state.
- [ ] Define rollback/recovery procedure.
- [ ] Retire or repurpose legacy automatic force-push behavior after replacement verification.

## Phase 7 - Process Maturity

- [ ] Reduce manual handoffs without weakening verification.
- [ ] Add automated documentation checks where useful.
- [ ] Add agent-generated test evidence.
- [ ] Measure development cycle time and failure/rework points.
- [ ] Periodically audit the process against actual repository/runtime behavior.
