# GitHub Workflow

## Branch Roles

```
chatgpt = primary working branch
public  = release candidate / last-known-good release state
```

## Issue → Implementation

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

## ADO Trigger

The replacement CI/CD model should use a GitHub-driven trigger for applicable development changes.

The intended path is:

```
GitHub chatgpt change
        |
        v
Azure DevOps validation/build
```

The exact ADO/GitHub integration mechanism will be selected during ADO pipeline implementation. It must trigger against the intended development branch and build the revision that caused the run.

## Promotion

Normal promotion is:

```
chatgpt
  -> ADO validation/build
  -> deployment
  -> runtime verification
  -> documentation audit
  -> verified release candidate
  -> public
```

`public` must not be treated as a normal development branch.

The existing ADO force-push mechanism is legacy implementation behavior. It may be replaced when the new workflow is implemented and verified.

## Repository-Specific Rules

The platform repositories retain their own authoritative rules. This document defines the process-level workflow and must not override repository-specific architecture.
