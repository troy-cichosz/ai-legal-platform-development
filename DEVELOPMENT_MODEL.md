# Development Model

## Objective

Use ChatGPT for architecture, requirements, cross-service reasoning, review, and verification planning while delegating repository implementation and mechanical development work to coding agents where that improves throughput.

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
4. Work on chatgpt branch
        |
        v
5. Coding agent implements/tests/reviews
        |
        v
6. Pull request / code review
        |
        v
7. GitHub change triggers ADO
        |
        v
8. ADO validates/builds/publishes
        |
        v
9. Deploy to the appropriate test environment
        |
        v
10. Runtime / integration / failure-recovery verification
        |
        v
11. Documentation completion audit
        |
        v
12. Verified state becomes eligible for public promotion
```

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

### Azure DevOps

- Detect applicable GitHub development changes.
- Build affected repositories.
- Run project-standard automated tests.
- Build container images/artifacts.
- Publish identifiable build outputs.
- Deploy to existing test/integration targets.
- Execute automated deployment/runtime verification.
- Preserve build/deployment evidence.
- Provide the operational verification gate before a candidate is eligible for `public`.

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

The replacement ADO workflow should:

1. Start from GitHub development state, normally `chatgpt`.
2. Build the exact source revision that triggered the run.
3. Produce identifiable artifacts and container images.
4. Reuse the existing self-hosted PIs and Linux/x86 infrastructure where appropriate.
5. Reuse the existing local Docker registry and security scanning where practical.
6. Deploy only the candidate artifact that was built and identified by the run.
7. Perform automated verification appropriate to the affected service.
8. Preserve enough build/deployment information to correlate runtime results with the source revision.
9. Make promotion to `public` a distinct release action rather than an incidental side effect of every build.

The existing `azure-pipelines.yaml` files are reference implementations for service-specific build requirements. They are not architectural constraints on the replacement pipeline design.

## Documentation

The platform's existing document ownership model remains in force.

Do not replace service/project status files with agent-generated summaries. The process repository records the development workflow; the owning platform repository records service behavior and verified state.
