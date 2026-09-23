# Development Model

## Objective

Use ChatGPT for architecture, requirements, cross-service reasoning, review, and verification planning while delegating repository implementation and mechanical development work to a coding agent running on the local Windows workstation where that improves throughput.

Use local AI inference through Ollama where practical so the baseline development workflow does not require a paid hosted coding service.

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
4. Local coding agent inspects repository + issue
        |
        v
5. Agent implements/tests/reviews on chatgpt
        |
        v
6. Pull request / code review as appropriate
        |
        v
7. GitHub chatgpt push triggers edge-platform-automation
        |
        v
8. Automation synchronizes source into matching ADO service/chatgpt
        |
        v
9. Existing ADO service CI/CD builds/publishes/deploys
        |
        v
10. Runtime / integration / failure-recovery verification
        |
        v
11. Documentation completion audit
        |
        v
12. Verified state becomes eligible for controlled public promotion
```

## Local Agent + AI Boundary

The local workstation is the implementation environment.

The coding agent is responsible for repository-scale mechanical work. Ollama provides local model inference where practical and where supported by the selected agent.

The local agent workflow must:

- take a GitHub Issue as durable task input;
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
- Synchronize the source tree into the matching ADO service repository's `chatgpt` branch.
- Remove stale ADO files absent from GitHub.
- Create and push the ADO synchronization commit.
- Preserve correlation between the GitHub revision and ADO synchronization.

It does not build service images, deploy services directly, or modify GitHub `public`.

### Azure DevOps

- Detect the synchronization commit on the ADO service `chatgpt` branch.
- Run the existing service CI/CD pipeline.
- Build container images/artifacts.
- Run existing scans/tests.
- Publish identifiable build outputs.
- Deploy using existing service behavior.
- Preserve build/deployment evidence.

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

## CI/CD Design Principles

The migration deliberately preserves the existing service CI/CD.

1. GitHub service `chatgpt` is authoritative for source and history.
2. A GitHub `chatgpt` push triggers `edge-platform-automation`.
3. Automation identifies the service and retrieves its GitHub source.
4. Automation synchronizes that source tree into the matching ADO service `chatgpt` branch.
5. The ADO branch change triggers the existing service CI/CD.
6. Existing build, scan, registry, deployment, and release behavior remains in place.
7. Runtime verification remains separate from successful build/deployment.
8. ADO mirror history is not authoritative.
9. `edge-platform-automation` does not directly modify GitHub `public`.
10. The legacy automatic `public` behavior remains until a separate promotion design is implemented and verified.

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
