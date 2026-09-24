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

`public` represents the release-candidate / last-known-good ADO-maintained state.

Promotion/maintenance of `public` requires the established build, deployment, runtime verification, and documentation workflow appropriate to the repository class.

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

Automation may build, test, deploy, collect evidence, and prepare public maintenance. Promotion to the release-candidate baseline remains tied to the verified operational workflow.

## D-009 — Repository State Before Conversation Memory

**Status:** Accepted

When current repository state can be inspected, it takes precedence over prior conversation memory for implementation and documentation decisions.

## D-010 — No Legacy chatgpt.md Handoff Documents

**Status:** Accepted

The platform repositories explicitly retire legacy `chatgpt.md` handoff documents. Durable state belongs in the owned project/service documents and this process repository where appropriate.

## D-011 — GitHub-to-ADO Source Synchronization

**Status:** Accepted

GitHub `chatgpt` is the authoritative development source for all covered project repositories.

`edge-platform-automation` synchronizes the authoritative source into the matching ADO repository's `chatgpt` branch.

The seven covered repositories are classified as either `build_service` or `sync_only`:

- `build_service` repositories continue into their existing service CI/CD.
- `sync_only` repositories use their ADO `chatgpt` branch for repository-maintenance work and their existing `azure-pipelines.yaml` public-maintenance pipeline.

The ADO repository is an operational mirror. Its commit history does not need to match GitHub.

The synchronization is complete-tree: files deleted from authoritative GitHub source are removed from the ADO working tree.

Only GitHub `chatgpt` events are accepted by the synchronization automation. This provides the recursion boundary when `edge-platform-automation` public maintenance generates a subsequent GitHub `public` event.


## D-012 — Local Multi-Agent AI Is a Reusable Development Environment

**Status:** Accepted

The local AI environment is a reusable engineering capability for both `ai-legal-platform-development` and the actual AI Legal Platform / edge repositories.

The baseline environment uses local inference through Ollama and must not require paid OpenAI API usage or a paid hosted coding-agent subscription.

The environment separates implementation from independent rules/compliance and testing/review functions. The coding agent is not the sole authority for adherence to project rules.

The coding model must be capable of repository-scale implementation while preserving established architecture, working behavior, service boundaries, evidence semantics, temporal authority, and overall project goals. Broad unsolicited rewrites are prohibited.

ChatGPT remains the human-facing architecture, requirements, cross-service reasoning, and difficult-review resource. The human remains the authority for architectural decisions, operational confirmation, and final release decisions.

## D-013 — ChatGPT Free Is Not an API Dependency

**Status:** Accepted

The development process may use ChatGPT Free interactively, but local agents must not depend on the ChatGPT Free service as an external API backend.

The base workflow must continue to function using local Ollama inference without OpenAI API calls.
