# GitHub Workflow

## Branch Roles

```
chatgpt = primary working branch
public  = release candidate / last-known-good ADO-mirrored state
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

## Promotion

Normal promotion is:

```
chatgpt
  -> PR/review
  -> ADO build/test
  -> deployment
  -> runtime verification
  -> documentation audit
  -> public
```

The exact promotion mechanism is to be defined after the ADO inventory.

## Repository-Specific Rules

The platform repositories retain their own authoritative rules. This document defines the process-level workflow and must not override repository-specific architecture.
