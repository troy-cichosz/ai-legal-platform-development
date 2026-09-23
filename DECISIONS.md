# Development Process Decisions

## D-001 — GitHub Is the Development Source of Truth

**Status:** Accepted

GitHub is authoritative for committed source, documentation, issues, branches, pull requests, and agent instructions.

ADO remains the operational CI/CD authority.

## D-002 — chatgpt Is the Primary Working Branch

**Status:** Accepted

`chatgpt` is the primary working/development branch across the covered repositories.

Changes should not be developed directly on `public`.

## D-003 — public Is the Release-Candidate Branch

**Status:** Accepted

`public` represents the release-candidate / last-known-good ADO-mirrored state.

Promotion to `public` requires the established build, deployment, runtime verification, and documentation workflow.

## D-004 — ADO Remains CI/CD Authority

**Status:** Accepted

Existing Azure DevOps self-hosted agents and deployment infrastructure remain the preferred CI/CD execution environment.

Do not introduce a second CI/CD platform merely to duplicate existing capability.

## D-005 — Local-First Agent Execution

**Status:** Accepted

The preferred coding-agent environment is the Windows 11 development workstation, using existing hardware and local inference where practical.

The baseline process must not require a paid hosted coding service.

## D-006 — Docker Desktop Is Not a Baseline Requirement

**Status:** Accepted

The Windows workstation does not need Docker Desktop solely to establish the development process. Container builds/deployments can remain an ADO responsibility unless a concrete local-development requirement demonstrates otherwise.

## D-007 — Preserve Existing Platform Rules

**Status:** Accepted

The new process repository does not replace or weaken the platform's existing architectural rules, documentation ownership model, evidence model, or verification requirements.

## D-008 — Do Not Automate Promotion Without Verification

**Status:** Accepted

Automation may build, test, deploy, collect evidence, and prepare promotion. Promotion to the release-candidate baseline must remain tied to the verified operational workflow.

## D-009 — Repository State Before Conversation Memory

**Status:** Accepted

When current repository state can be inspected, it takes precedence over prior conversation memory for implementation and documentation decisions.

## D-010 — No Legacy chatgpt.md Handoff Documents

**Status:** Accepted

The platform repositories explicitly retire legacy `chatgpt.md` handoff documents. Durable state belongs in the owned project/service documents and this process repository where appropriate.

## D-011 — GitHub-to-ADO Source Synchronization

**Status:** Accepted

GitHub service `chatgpt` is the authoritative development source. `edge-platform-automation` synchronizes that source into the matching ADO service `chatgpt` branch so the existing service CI/CD can run without replacing its trigger model.

The ADO service repository is an operational build mirror. Its commit history does not need to match GitHub.

The synchronization is complete-tree: files deleted from authoritative GitHub source are removed from the ADO working tree.

`edge-platform-automation` does not directly modify GitHub `public`.
