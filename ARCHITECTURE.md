# Development Process Architecture

## System Boundary

This repository is the control plane for the development process, not the runtime control plane of the AI Legal Platform.

```
                         HUMAN
                           |
                           v
                CHATGPT / ARCHITECTURE
                           |
                           v
                    GITHUB CONTROL
             Issues / branches / PRs / docs
                           |
                           v
                 LOCAL AI AGENT SYSTEM
                 Windows + Ollama
                           |
              +------------+------------+
              |            |            |
              v            v            v
           CODING       RULES/        TEST/
           AGENT       COMPLIANCE     REVIEW
              |            |            |
              +------------+------------+
                           |
                           v
                    GitHub chatgpt
                           |
                           v
                    GITHUB WEBHOOK
                           |
                           v
              EDGE-PLATFORM-AUTOMATION
                           |
                           v
                ADO service/chatgpt
                           |
                           v
                 EXISTING ADO CI/CD
                           |
                    +------+------+
                    |             |
                    v             v
                 BUILD/TEST    DEPLOYMENT
                                  |
                                  v
                           EDGE TEST TARGETS
                                  |
                                  v
                         RUNTIME VERIFICATION
                                  |
                                  v
                         RELEASE CANDIDATE
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

### Local Coding Agent / Ollama

The local workstation is an implementation aid, not an authority.

The agent may inspect, edit, test, diff, commit, and prepare PRs within explicit repository and issue constraints. Local model output does not establish architecture, runtime correctness, or legal conclusions.

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

A source file, model response, or successful build cannot establish that runtime behavior works.

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

Agents must not silently redefine architectural boundaries, branch roles, evidence semantics, temporal authority, or verification standards.

## CI/CD Boundary

The process is intentionally ADO-centric because the existing self-hosted infrastructure already performs the required build/deployment work.

The automation layer moves authoritative GitHub source into ADO service mirrors so existing service CI/CD can remain intact. It is not a second build system.

## Runtime Platform Boundary

The development process does not change the existing edge runtime architecture:

- edge services remain container-first;
- hardware ownership remains with the owning service;
- local `edge-time` remains the evidence-facing temporal authority;
- `edge-controller` remains management/policy plane;
- cross-service HTTP remains host-addressed;
- original evidence remains locally owned and immutable after finalization.

## Design Principle

Automate mechanical movement, build, deployment, and verification where evidence is available.

Keep architecture, source-of-truth decisions, evidence authority, and release decisions explicit and reviewable.

Local AI accelerates implementation; it does not become an authority over the platform.
