# Development Model

## Objective

Use ChatGPT for architecture, requirements, cross-service reasoning, review, and verification planning while delegating repository implementation and mechanical development work to a coding agent running on the local Windows workstation where that improves throughput.

Use local AI inference through Ollama where practical so the baseline development workflow does not require a paid hosted coding service or OpenAI API dependency.

The local environment must be reusable for both this development-process repository and the actual AI Legal Platform / edge repositories.

The coding model is expected to perform repository-scale implementation with architectural fidelity suitable for this project. It must preserve existing working behavior and project goals and must not perform broad unsolicited rewrites.

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
8. Automation synchronizes source into matching ADO repository/chatgpt
        |
        v
9. Build-service repositories enter existing ADO service CI/CD;
   sync-only repositories enter their public-maintenance pipeline
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
11. The long-term `chatgpt` → `public` promotion mechanism remains a separate process concern.

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
