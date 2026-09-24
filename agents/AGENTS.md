# Coding Agent Operating Contract

## Before Work

1. Read this file.
2. Read the target repository's applicable agent instructions.
3. Read `chatrules.md` where present.
4. Read `projectrules.md` where the work may affect platform architecture.
5. Read the affected service README/status/sprint documents.
6. Inspect the actual source before proposing implementation.

## Branches

- Primary development branch: `chatgpt`
- Release candidate: `public`
- Do not commit development work directly to `public`.
- Do not assume `main` is authoritative for the covered platform repositories.

## Implementation

- Work only within the issue scope.
- Treat the coding-agent role as implementation, not architecture ownership.
- Preserve the established project goal; do not reinterpret a focused issue as permission for a broad redesign.
- Do not replace working code merely because a different implementation appears cleaner or more modern.
- Preserve existing architecture unless the issue explicitly changes it.
- Prefer the smallest change that satisfies the acceptance criteria.
- Do not replace working code merely for stylistic reasons.
- Do not invent APIs, runtime behavior, hardware capabilities, or deployment facts.
- Keep service-specific behavior in the owning service.
- Preserve evidence immutability and provenance rules.
- Use host-addressed HTTP for cross-service communication.
- Do not route evidence capture through `edge-controller`.

## Verification

At minimum, report:

- files changed;
- tests run;
- test results;
- build result if available;
- unresolved warnings/failures;
- behavior that remains unverified.

A successful local test does not replace ADO/runtime verification.

## Documentation

Update only documents whose owned information changed.

Do not create legacy `chatgpt.md` handoff documents.

Distinguish:

- implemented;
- built;
- deployed;
- runtime verified;
- future/deferred.

## Git

Before finishing:

1. Review the diff.
2. Check for accidental files/secrets.
3. Run applicable tests.
4. Report the exact commit/branch state.
5. Do not claim runtime verification that the agent did not perform.

## Independent Compliance Review

Before a change is considered ready for commit:

1. The coding agent reports its own tests and diff review.
2. An independent rules/compliance check should inspect the same change where the local environment supports it.
3. Compliance failures are corrected or explicitly escalated; they are not self-approved by the coding agent.
4. Architectural questions are surfaced rather than resolved through assumption.

The compliance check should verify, as applicable:

- repository and project rules;
- issue scope;
- branch/workflow rules;
- architecture and service boundaries;
- evidence/provenance and temporal rules;
- documentation ownership;
- required tests and verification evidence;
- unexpected broad changes.

## Architectural Escalation

Stop and surface the issue when implementation appears to require:

- a new cross-service contract;
- a change to evidence authority/immutability;
- a change to temporal authority;
- a controller boundary change;
- a database architecture change;
- a new deployment dependency;
- a branch/workflow rule change.

Those are design decisions, not routine coding details.
