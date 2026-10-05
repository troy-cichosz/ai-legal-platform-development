# GitHub Workflow

## Branch Roles

```
chatgpt = primary working branch
public  = release candidate / last-known-good release state
```

## Issue -> Implementation

A development increment should begin with a GitHub Issue describing:

- problem/objective;
- affected repository/repositories;
- current verified behavior;
- desired behavior;
- constraints/invariants;
- acceptance criteria;
- verification plan;
- documentation impact.

The issue becomes the durable task reference for the coding agent.

## Pull Requests

PRs should identify:

- issue reference;
- implementation summary;
- tests;
- affected services;
- architectural impact;
- deployment/verification requirements;
- documentation changes;
- remaining uncertainty.

PR approval is not a substitute for runtime verification.

## GitHub -> ADO Trigger

The approved migration path is:

```
GitHub service/chatgpt
        |
        v
GitHub webhook
        |
        v
edge-platform-automation - CI
        |
        | pull GitHub chatgpt source
        v
matching ADO service/chatgpt
        |
        v
existing ADO service CI/CD
```

The automation pipeline identifies the repository, validates the `chatgpt` branch, retrieves the source, and synchronizes it into the matching ADO service repository. The ADO branch is an operational build mirror; its commit history does not need to match GitHub.

## Promotion

Current migration flow is:

```
GitHub chatgpt
  -> edge-platform-automation synchronization
  -> ADO service/chatgpt
  -> existing ADO CI/CD
  -> deployment
  -> runtime verification
  -> documentation audit
  -> verified release candidate
  -> existing public behavior
```

`public` must not be treated as a normal development branch.

The existing ADO force-push behavior remains during this migration so the established build/release flow is not disrupted. `edge-platform-automation` does not directly modify GitHub `public`.

## Repository-Specific Rules

The platform repositories retain their own authoritative rules. This document defines the process-level workflow and must not override repository-specific architecture.
