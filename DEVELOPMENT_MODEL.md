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
7. ADO CI/CD builds and tests
        |
        v
8. Deploy to the appropriate test environment
        |
        v
9. Runtime / integration / failure-recovery verification
        |
        v
10. Documentation completion audit
        |
        v
11. Verified state promoted to public
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

- Build affected repositories.
- Run project-standard automated tests.
- Build container images/artifacts.
- Deploy to existing test/integration targets.
- Execute deployment/runtime verification.
- Preserve build/deployment evidence.
- Provide the operational gate before known-good state is promoted.

### Human Operator

- Controls ADO integration and operational environment.
- Performs or authorizes deployment actions not safely automated.
- Confirms runtime results.
- Confirms substantive architectural or implementation changes before commitment where required by existing repository rules.

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

## Documentation

The platform's existing document ownership model remains in force.

Do not replace service/project status files with agent-generated summaries. The process repository records the development workflow; the owning platform repository records service behavior and verified state.
