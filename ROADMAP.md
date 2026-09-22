# Development Process Roadmap

## Phase 0 — Development Workflow Design

**Status:** IN PROGRESS

- [x] Create process control-plane repository.
- [x] Establish `chatgpt` working / `public` release-candidate model.
- [x] Review current edge repository rules and status.
- [x] Record current platform baseline.
- [ ] Inventory existing ADO environment.
- [ ] Identify current GitHub → ADO triggers.
- [ ] Identify current build/deploy/test boundaries.

## Phase 1 — ADO Integration Mapping

- [ ] Document ADO projects.
- [ ] Document pipelines and repositories.
- [ ] Document self-hosted agent pools/capabilities.
- [ ] Document service connections and deployment targets.
- [ ] Document environment/test topology.
- [ ] Define the minimum CI validation contract for PRs targeting `chatgpt`.
- [ ] Define promotion verification required before `public`.

## Phase 2 — GitHub Development Standard

- [ ] Standardize issue structure.
- [ ] Standardize agent task instructions.
- [ ] Standardize PR requirements.
- [ ] Establish repository-specific agent instruction discovery.
- [ ] Establish labels/milestones appropriate to development-process tracking.

## Phase 3 — Coding Agent

- [ ] Evaluate free/local coding-agent options against the real workflow.
- [ ] Evaluate local Ollama model suitability on RTX 3060 12 GB.
- [ ] Define agent permissions and safety boundaries.
- [ ] Establish repeatable Windows workstation setup.
- [ ] Validate agent implementation on a low-risk repository change.

## Phase 4 — GitHub → ADO Automation

- [ ] Trigger ADO validation from GitHub changes using the existing integration where possible.
- [ ] Build affected repositories automatically.
- [ ] Run automated tests.
- [ ] Produce identifiable build artifacts/results.
- [ ] Make deployment to the test environment repeatable.

## Phase 5 — Automated Integration Verification

- [ ] Deploy approved artifacts to test nodes.
- [ ] Run service health checks.
- [ ] Run cross-service integration checks.
- [ ] Run relevant evidence/time/provenance checks.
- [ ] Run failure/recovery checks where required.
- [ ] Record verification results in the appropriate system.

## Phase 6 — Release-Candidate Promotion

- [ ] Define exact `chatgpt` → `public` promotion mechanism.
- [ ] Ensure only verified state reaches `public`.
- [ ] Keep `public` synchronized with the ADO-mirrored known-good state.
- [ ] Define rollback/recovery procedure.

## Phase 7 — Process Maturity

- [ ] Reduce manual handoffs without weakening verification.
- [ ] Add automated documentation checks where useful.
- [ ] Add agent-generated test evidence.
- [ ] Measure development cycle time and failure/rework points.
- [ ] Periodically audit the process against actual repository/runtime behavior.
