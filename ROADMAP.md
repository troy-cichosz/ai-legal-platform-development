# Development Process Roadmap

## Phase 0 — Development Workflow Design

**Status:** IN PROGRESS

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
- [ ] Define replacement GitHub `chatgpt` → ADO trigger model.
- [ ] Define replacement validation/build/deployment pipeline contract.
- [ ] Define automated runtime/integration verification contract.
- [ ] Define exact `chatgpt` → `public` promotion gate.

## Phase 1 — ADO Integration Mapping

- [ ] Document ADO projects.
- [ ] Document replacement pipeline names and repository mappings.
- [ ] Document self-hosted agent pools/capabilities.
- [ ] Document reusable service connections and registry access.
- [ ] Document deployment targets.
- [ ] Document environment/test topology.
- [ ] Define the minimum CI validation contract for changes on `chatgpt`.
- [ ] Define promotion verification required before `public`.
- [ ] Build and validate the first replacement pipeline.

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

- [ ] Trigger ADO validation from GitHub changes using the new integration.
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
- [ ] Keep `public` synchronized with the verified known-good state.
- [ ] Define rollback/recovery procedure.
- [ ] Retire or repurpose legacy automatic force-push behavior after replacement verification.

## Phase 7 — Process Maturity

- [ ] Reduce manual handoffs without weakening verification.
- [ ] Add automated documentation checks where useful.
- [ ] Add agent-generated test evidence.
- [ ] Measure development cycle time and failure/rework points.
- [ ] Periodically audit the process against actual repository/runtime behavior.
