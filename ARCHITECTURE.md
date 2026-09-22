# Development Process Architecture

## System Boundary

This repository is the control plane for the development process, not the runtime control plane of the AI Legal Platform.

```
                         HUMAN
                           |
                           v
                    CHATGPT / REVIEW
                           |
                           v
                    GITHUB CONTROL
             Issues / branches / PRs / docs
                           |
                           v
                    CODING AGENT
                    Windows workstation
                           |
                           v
                    chatgpt branch
                           |
                           v
                   AZURE DEVOPS CI/CD
                           |
                 +---------+---------+
                 |                   |
                 v                   v
              BUILD/TEST        DEPLOYMENT
                                     |
                                     v
                              EDGE TEST TARGETS
                                     |
                                     v
                           RUNTIME VERIFICATION
                                     |
                                     v
                         known-good public state
```

## Authority Boundaries

### GitHub

Authoritative for:

- committed source;
- issues;
- pull requests;
- development documentation;
- branch history;
- agent instructions.

### Platform Repositories

Authoritative for:

- service implementation;
- service interfaces;
- service architecture;
- service maturity/status;
- project-level architectural rules maintained by the platform repository.

### Azure DevOps

Authoritative for:

- CI/CD execution;
- build results;
- deployment execution;
- operational test execution;
- deployment/runtime evidence.

ADO is not a second source of truth for source code.

### Runtime

Authoritative for observed behavior of deployed artifacts.

A source file cannot establish that runtime behavior works.

## Agent Boundary

Coding agents operate against real repositories and GitHub issues.

Agents may:

- inspect;
- edit;
- test;
- diff;
- commit;
- push;
- prepare PRs.

Agents must follow repository-specific rules before making changes.

Agents must not silently redefine architectural boundaries, branch roles, evidence semantics, or verification standards.

## CI/CD Boundary

The process is intentionally ADO-centric because the existing self-hosted infrastructure already performs the required build/deployment work.

GitHub changes should trigger the applicable ADO validation pipeline. The exact trigger mechanism is an implementation item and must be established from the current ADO inventory rather than assumed.

## Runtime Platform Boundary

The development process does not change the existing edge runtime architecture:

- edge services remain container-first;
- hardware ownership remains with the owning service;
- local `edge-time` remains the evidence-facing temporal authority;
- `edge-controller` remains management/policy plane;
- cross-service HTTP remains host-addressed;
- original evidence remains locally owned and immutable after finalization.
