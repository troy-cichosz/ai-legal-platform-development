# Development Model

## Objective

Use ChatGPT for architecture, requirements, cross-service reasoning, review, and verification planning while delegating repository implementation and mechanical development work to a coding agent running on the local Windows workstation where that improves throughput.

Use local AI inference through Ollama where practical so the baseline development workflow does not require a paid hosted coding service or OpenAI API dependency.

The local environment must be reusable for both this development-process repository and the actual AI Legal Platform / edge repositories.

The coding model is expected to perform repository-scale implementation with architectural fidelity suitable for this project. It must preserve existing working behavior and project goals and must not perform broad unsolicited rewrites.

The current target is practical development-agent capability: the agent must be able to pick up an existing repository and project state and continue the work from authoritative documentation, current source, issue/task scope, and relevant history. Python implementation is a first-class requirement because the edge services are currently Python-based. The capability must extend beyond isolated coding to multi-file and cross-repository changes, testing/debugging, documentation updates, and accurate development-state reporting.

The model itself is not the sole rule-enforcement mechanism. Independent compliance and review roles are part of the design.

The model must preserve human control over architectural decisions and operational verification.

## Normal Lifecycle

```
1. Define the development increment
        |
        v
2. Review authoritative GitHub rules/status
        |
        v
3. Create or update GitHub Issue
        |
        v
4. Local agent system inspects repository + issue
        |
        v
5. Coding agent implements/tests/reviews on chatgpt
        |
        v
6. Independent rules/compliance and test/review checks
        |
        v
7. Pull request / code review as appropriate
        |
        v
8. GitHub chatgpt push triggers edge-platform-automation
        |
        v
9. Automation synchronizes source into matching ADO repository/chatgpt
        |
        v
10. Build-service repositories enter existing ADO service CI/CD;
    sync-only repositories enter their public-maintenance pipeline
        |
        v
11. Runtime / integration / failure-recovery verification
        |
        v
12. Documentation completion audit
        |
        v
12. Verified state becomes eligible for controlled public promotion
```

## Coding-Agent Framework Selection

Use an established free/open-source repository-oriented coding-agent framework before building equivalent bespoke infrastructure when the framework can satisfy the required workflow. Framework selection comes before model optimization.

Aider was the first established framework evaluated with local Ollama inference, followed by Continue and OpenCode. Aider and Continue did not qualify from the common TASK-AIDER-001 qualification. OpenCode + `gpt-oss:20b` passed TASK-AIDER-001 but failed TASK-PY-005 because its focused test did not demonstrate preservation of an actual temporal context and its completion report misstated the repository state. OpenCode + `gpt-oss:20b` also failed TASK-PY-003 because the corrected run materialized only a partial production change (`self.live_process = None` after live-process exit and BrokenPipeError/OSError) and did not produce the required focused tests or demonstrate the required live-failure disable and authoritative-evidence continuation behavior. The agent also did not complete the required validation/reporting. The environment should continue evaluating established local candidates such as Cline, Roo Code, and OpenHands before finalizing the framework.

Evaluate each framework as an end-to-end development-agent configuration with the local model. Acceptance requires correct implementation, focused tests, preservation of unrelated content, scoped changes, required validation, documentation accuracy, ASCII compliance, controllable permissions, and accurate reporting.

The model and framework do not need to self-enforce every project rule. Deterministic controls must enforce hard repository, branch, permission, and release boundaries, while independent compliance/testing review checks the result. For Aider, tasks that prohibit agent-created commits must use its no-auto-commits configuration rather than relying only on natural-language instructions.

Failed benchmark worktrees are disposable evidence and must not be repaired and promoted as benchmark successes. A passed qualification establishes qualification evidence for the tested framework/model configuration, but does not by itself establish a final framework or model default. Human acceptance remains required.

## Development-Agent Qualification Boundary

The qualified unit is:

```text
Agent Framework + Local Model + Repository Rules
+ Tool Permissions + Repository/Worktree Boundary
+ Independent Validation
```

Framework selection precedes model optimization. A model is not accepted as a coding-agent default from isolated generation tests or runtime performance alone.

The framework must expose or enforce the repository and tool boundaries required by the task. Deterministic controls should enforce hard repository, branch, permission, and release boundaries where practical. The coding agent's own validation is informational; independent repository validation remains required.

The qualification sequence is:

1. framework qualification;
2. tool/permission/write-boundary qualification;
3. minimal controlled repository task;
4. Python single-file implementation;
5. multi-file edge-service task;
6. cross-service task;
7. documentation/project-state continuation;
8. independent compliance/testing;
9. human acceptance.

Failed disposable worktrees remain evidence and are not repaired and promoted as benchmark successes.
## Local Agent + AI Boundary

The local workstation is the implementation environment.

The coding agent is responsible for repository-scale mechanical work. Ollama provides local model inference where practical and where supported by the selected agent.

The local agent workflow must:

- take a GitHub Issue as durable task input;
- establish the current project/repository state and relevant authoritative history before editing;
- read repository-specific instructions before editing;
- inspect actual source and current branch state;
- implement only the approved scope;
- run practical local tests;
- inspect its diff;
- report limitations and unresolved issues;
- commit/push only to the designated development workflow.

Architecture, cross-service contracts, evidence semantics, temporal authority, and release policy remain explicit design decisions rather than agent assumptions.

## Responsibilities

### ChatGPT

- Architecture and system-boundary reasoning.
- Cross-service impact analysis.
- Requirements and acceptance criteria.
- Review of authoritative repository documentation.
- Review of proposed implementation.
- Verification planning.
- Documentation ownership/audit.
- Recovery/handoff state.
- Identification of contradictions or architectural drift.

ChatGPT should not invent current implementation state when GitHub or runtime evidence is available.

### Development-State Continuity

When continuing an existing increment, the agent must treat current repository state and authoritative project/status documentation as the starting point. It must distinguish verified state from planned or historical state and must not silently rewrite project scope, architecture, or status. If the task requires a cross-repository contract or a change to authoritative project state, it must escalate or follow the explicitly defined project-management step rather than inventing a new direction.

### Coding Agent

- Inspect the real repository.
- Read applicable agent instructions and project rules.
- Implement the approved issue.
- Run local tests that are practical in the available environment.
- Review its own diff.
- Keep changes scoped to the issue.
- Report tests, limitations, and unresolved questions.
- Commit/push to the designated working branch or create a task branch that ultimately targets `chatgpt`, according to the repository workflow.

The coding agent does not decide platform architecture independently.

### edge-platform-automation

- Receive the GitHub `chatgpt` push webhook.
- Validate the supported repository and development branch.
- Retrieve the GitHub source.
- Synchronize the source tree into the matching ADO repository's `chatgpt` branch.
- Remove stale ADO files absent from GitHub.
- Create and push the ADO synchronization commit.
- Preserve correlation between the GitHub revision and ADO synchronization.

For `build_service` repositories, the resulting ADO branch change enters the existing service CI/CD.

For `sync_only` repositories, the resulting ADO branch change enters the repository's public-maintenance pipeline. That pipeline maintains the GitHub `public` branch using the established ADO-to-GitHub mechanism.

The `edge-platform-automation` public update can generate another GitHub webhook event, but the synchronization pipeline accepts only `chatgpt` events. This is the explicit recursion boundary.

### Azure DevOps

- Detect the synchronization commit on the ADO repository `chatgpt` branch.
- Run the appropriate downstream pipeline.
- For build services, build container images/artifacts, run existing scans/tests, publish identifiable outputs, and deploy using existing service behavior.
- For sync-only repositories, run public-maintenance behavior without unnecessary Docker build/deployment.
- Preserve build/deployment or maintenance evidence.

ADO does not become the source of truth for source code.

### Human Operator

- Controls ADO integration and operational environment.
- Performs or authorizes deployment actions not safely automated.
- Confirms runtime results.
- Confirms substantive architectural or implementation changes where required by existing repository rules.
- Controls or authorizes final promotion to `public` unless a later explicitly approved policy delegates that action.

## Verification States

Use explicit states:

```
planned
  -> implemented
  -> built
  -> deployed
  -> runtime verified
  -> documented
  -> release candidate
```

Do not collapse these into a single "done" state.

## Change Scope

Each issue should identify:

- affected repositories;
- intended behavior;
- architectural constraints;
- acceptance criteria;
- tests;
- deployment target;
- runtime verification;
- documentation impact.

Avoid broad refactoring during a focused increment unless the issue explicitly establishes that scope.

### Commit Consolidation

Related changes belonging to the same development increment should be consolidated into a single Git commit per repository whenever practical. Do not create separate commits merely because individual files or individual corrections were completed at different points during the same increment. Complete the related changes, inspect the resulting repository state and complete diff, then create one commit and push it through the normal `chatgpt` workflow.

Separate commits are appropriate when changes are genuinely independent, require separate review or rollback boundaries, or when the repository workflow explicitly requires them.

This rule is intended to minimize unnecessary GitHub `chatgpt` push events and therefore unnecessary overlapping ADO synchronization and downstream pipeline runs.

## CI/CD Design Principles

The migration deliberately preserves the existing service CI/CD.

1. GitHub `chatgpt` is authoritative for source and history.
2. A GitHub `chatgpt` push triggers `edge-platform-automation`.
3. Automation identifies the repository and retrieves its exact GitHub source.
4. Automation synchronizes that source tree into the matching ADO `chatgpt` branch.
5. Build-service ADO branch changes trigger the existing service CI/CD.
6. Sync-only ADO branch changes trigger their public-maintenance pipelines.
7. Existing service build, scan, registry, deployment, and release behavior remains in place.
8. Runtime verification remains separate from successful build/deployment.
9. ADO mirror history is not authoritative.
10. Only `chatgpt` events are accepted by the synchronization automation, providing the recursion boundary for `edge-platform-automation`.
11. The long-term `chatgpt` -> `public` promotion mechanism remains a separate process concern.

### Commit/Pipeline Discipline

The normal development workflow should minimize successive `chatgpt` pushes. A related repository increment should normally produce one GitHub `chatgpt` push and therefore one corresponding automation synchronization cycle. Do not intentionally generate multiple rapid commits for a single increment.

If genuinely independent increments must be processed separately, allow the corresponding automation and downstream pipeline activity to settle before starting another dependent increment.

## Documentation

Update only documents whose owned information changed.

Do not create legacy `chatgpt.md` handoff documents.

Distinguish:

- implemented;
- built;
- deployed;
- runtime verified;
- future/deferred.

The process repository records the development workflow. The owning platform repository records service behavior and verified service state.
