# Development Process Roadmap

## Phase 0 — Development Workflow Design

**Status:** COMPLETE

- [x] Create process control-plane repository.
- [x] Establish `chatgpt` working / `public` release-candidate model.
- [x] Review current edge repository rules and status.
- [x] Record current platform baseline.
- [x] Review current `azure-pipelines.yaml` files from all five `public` branches.
- [x] Establish current `branch2Push` = `public` behavior.
- [x] Establish current `repoName` and `gitCommit` semantics.
- [x] Establish existing ADO self-hosted agent-pool scope.
- [x] Establish existing ADO → GitHub authentication purpose without retrieving secrets.
- [x] Decide that existing pipeline YAML is reference material rather than a required replacement design.
- [x] Define GitHub webhook → `edge-platform-automation` trigger model.
- [x] Define GitHub `chatgpt` → ADO service-repository synchronization model.
- [x] Define ADO service `chatgpt` as an operational build-triggering mirror, not authoritative history.
- [x] Verify the first end-to-end synchronization and existing service CI trigger with `edge-gps`.
- [x] Establish the local-first coding-agent/Ollama direction as part of the target development workflow.

## Phase 1 — ADO Integration Mapping

**Status:** BASELINE COMPLETE; deeper replacement work deferred until automation expansion is proven.

- [x] Document ADO project and covered service mappings.
- [x] Document existing self-hosted agent-pool scope.
- [x] Document existing registry/scanning/build mechanics.
- [x] Document current deployment/verification responsibilities at the process level.
- [ ] Define the minimum CI validation contract for changes on `chatgpt`.
- [ ] Define promotion verification required before `public`.
- [ ] Define any replacement pipeline contract needed after the existing service CI/CD is no longer sufficient.

## Phase 2 — GitHub Development Standard

- [ ] Standardize issue structure.
- [ ] Standardize agent task instructions.
- [ ] Standardize PR requirements.
- [ ] Establish repository-specific agent instruction discovery.
- [ ] Establish labels/milestones appropriate to development-process tracking.

## Phase 3 — Local Coding Agent + AI

- [ ] Evaluate free/local coding-agent options against the real GitHub/ADO workflow.
- [ ] Evaluate local Ollama model suitability on RTX 3060 12 GB.
- [ ] Define agent permissions and safety boundaries.
- [ ] Establish repeatable Windows workstation setup.
- [ ] Validate agent implementation on a low-risk repository change.
- [ ] Connect the agent workflow to the standard GitHub Issue → `chatgpt` process.

## Phase 4 — GitHub → ADO Automation

### Increment B — `edge-gps` Pilot

**Status:** VERIFIED END-TO-END

- [x] Trigger `edge-platform-automation` from GitHub `chatgpt` changes.
- [x] Identify supported repository/ref/SHA.
- [x] Retrieve and verify exact GitHub source.
- [x] Synchronize complete source tree into matching ADO `chatgpt`.
- [x] Remove stale files.
- [x] Create/push and verify ADO synchronization commit.
- [x] Confirm synchronized ADO `chatgpt` commit triggers existing `edge-gps - CI`.
- [x] Preserve existing service CI/CD definitions.
- [x] Confirm automation does not directly modify GitHub `public`.
- [x] Record GitHub → ADO revision correlation.

### Increment C — Expand and Harden

**Status:** NEXT

Repository coverage is intentionally expanded to **all seven current project repositories**.

**Build/deploy services:**
- `edge-controller`
- `edge-time`
- `edge-gps`
- `edge-video`
- `edge-audio`

**Sync-only development/control repositories:**
- `ai-legal-platform-development`
- `edge-platform-automation`

Required work:

- [ ] Expand synchronization to all five build-service repositories.
- [ ] Add both sync-only repositories to the automation registry.
- [ ] Introduce repository classification so sync-only repositories do not enter service Docker CI/CD unnecessarily.
- [ ] Establish an ADO `chatgpt` mirror for every covered repository.
- [ ] Define controlled public-maintenance behavior for sync-only repositories.
- [ ] Design explicit recursion protection for `edge-platform-automation` self-synchronization.
- [ ] Verify each build-service ADO `chatgpt` branch triggers its existing service CI.
- [ ] Verify complete-tree deletion propagation.
- [ ] Verify idempotent synchronization of unchanged source.
- [ ] Verify non-`chatgpt` webhook events are harmless no-ops.
- [ ] Add preflight protection for missing required ADO service pipeline definitions.
- [ ] Preserve GitHub SHA → ADO synchronization SHA correlation.
- [ ] Document verified results and remaining limitations.

## Phase 5 — Automated Integration Verification

- [ ] Define a common verification contract with service-specific checks.
- [ ] Deploy approved artifacts to test nodes.
- [ ] Run service health checks.
- [ ] Run cross-service integration checks.
- [ ] Run relevant evidence/time/provenance checks.
- [ ] Run failure/recovery checks where required.
- [ ] Record verification results in the appropriate system.
- [ ] Make verification results usable as a promotion gate.

## Phase 6 — Release-Candidate Promotion

- [ ] Define exact `chatgpt` → `public` promotion mechanism.
- [ ] Ensure only verified state reaches `public`.
- [ ] Keep `public` synchronized with the verified known-good state.
- [ ] Define rollback/recovery procedure.
- [ ] Retire or repurpose legacy automatic force-push behavior after replacement verification.

## Phase 7 — Process Maturity

- [ ] Reduce manual handoffs without weakening verification.
- [ ] Add automated documentation checks where useful.
- [ ] Add agent-generated test evidence.
- [ ] Measure development cycle time and failure/rework points.
- [ ] Periodically audit the process against actual repository/runtime behavior.
