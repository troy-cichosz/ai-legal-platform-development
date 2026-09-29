# Development Process Decisions

## D-001 - GitHub Is the Development Source of Truth

**Status:** Accepted

GitHub is authoritative for committed source, documentation, issues, branches, pull requests, and agent instructions.

ADO remains the operational CI/CD authority.

## D-002 - chatgpt Is the Primary Working Branch

**Status:** Accepted

`chatgpt` is the primary working/development branch across the covered repositories.

Changes should not be developed directly on `public`.

## D-003 - public Is the Release-Candidate Branch

**Status:** Accepted

`public` represents the release-candidate / last-known-good ADO-maintained state.

Promotion/maintenance of `public` requires the established build, deployment, runtime verification, and documentation workflow appropriate to the repository class.

## D-004 - ADO Remains CI/CD Authority

**Status:** Accepted

Existing Azure DevOps self-hosted agents and deployment infrastructure remain the preferred CI/CD execution environment.

Do not introduce a second CI/CD platform merely to duplicate existing capability.

## D-005 - Local-First Agent Execution

**Status:** Accepted

The preferred coding-agent environment is the Windows 11 development workstation, using existing hardware and local inference where practical.

The baseline process must not require a paid hosted coding service.

## D-006 - Docker Desktop Is Not a Baseline Requirement

**Status:** Accepted

The Windows workstation does not need Docker Desktop solely to establish the development process. Container builds/deployments can remain an ADO responsibility unless a concrete local-development requirement demonstrates otherwise.

## D-007 - Preserve Existing Platform Rules

**Status:** Accepted

The new process repository does not replace or weaken the platform's existing architectural rules, documentation ownership model, evidence model, or verification requirements.

## D-008 - Do Not Automate Promotion Without Verification

**Status:** Accepted

Automation may build, test, deploy, collect evidence, and prepare public maintenance. Promotion to the release-candidate baseline remains tied to the verified operational workflow.

## D-009 - Repository State Before Conversation Memory

**Status:** Accepted

When current repository state can be inspected, it takes precedence over prior conversation memory for implementation and documentation decisions.

## D-010 - No Legacy chatgpt.md Handoff Documents

**Status:** Accepted

The platform repositories explicitly retire legacy `chatgpt.md` handoff documents. Durable state belongs in the owned project/service documents and this process repository where appropriate.

## D-011 - GitHub-to-ADO Source Synchronization

**Status:** Accepted

GitHub `chatgpt` is the authoritative development source for all covered project repositories.

`edge-platform-automation` synchronizes the authoritative source into the matching ADO repository's `chatgpt` branch.

The seven covered repositories are classified as either `build_service` or `sync_only`:

- `build_service` repositories continue into their existing service CI/CD.
- `sync_only` repositories use their ADO `chatgpt` branch for repository-maintenance work and their existing `azure-pipelines.yaml` public-maintenance pipeline.

The ADO repository is an operational mirror. Its commit history does not need to match GitHub.

The synchronization is complete-tree: files deleted from authoritative GitHub source are removed from the ADO working tree.

Only GitHub `chatgpt` events are accepted by the synchronization automation. This provides the recursion boundary when `edge-platform-automation` public maintenance generates a subsequent GitHub `public` event.


## D-012 - Local Multi-Agent AI Is a Reusable Development Environment

**Status:** Accepted

The local AI environment is a reusable engineering capability for both `ai-legal-platform-development` and the actual AI Legal Platform / edge repositories.

The baseline environment uses local inference through Ollama and must not require paid OpenAI API usage or a paid hosted coding-agent subscription.

The environment separates implementation from independent rules/compliance and testing/review functions. The coding agent is not the sole authority for adherence to project rules.

The coding model must be capable of repository-scale implementation while preserving established architecture, working behavior, service boundaries, evidence semantics, temporal authority, and overall project goals. Broad unsolicited rewrites are prohibited.

ChatGPT remains the human-facing architecture, requirements, cross-service reasoning, and difficult-review resource. The human remains the authority for architectural decisions, operational confirmation, and final release decisions.

## D-013 - ChatGPT Free Is Not an API Dependency

**Status:** Accepted

The development process may use ChatGPT Free interactively, but local agents must not depend on the ChatGPT Free service as an external API backend.

The base workflow must continue to function using local Ollama inference without OpenAI API calls.

## D-014 - Development-Agent Replacement Objective

**Status:** Accepted

The immediate local-AI objective is to establish whether a local development agent can reliably continue the actual AI Legal Platform edge-platform development work currently performed through the ChatGPT collaboration, at ChatGPT-level or better for the required development tasks.

Evaluation is based on representative real repository work: Python implementation, repository comprehension, architecture preservation, testing and debugging, documentation accuracy, project-state continuity, cross-repository consistency, validation discipline, and honest reporting. Model size, vendor reputation, or raw token throughput is not a substitute for successful development work.

The same workstation and local inference environment may later be reused for legal-AI workloads. That later workload is a separate capability objective and must not distort the current development-agent evaluation.

The coding agent remains subject to independent compliance/testing review and human acceptance. It does not independently redefine project architecture, authoritative project state, or release decisions.

## D-015 - Prefer Established Coding-Agent Frameworks Before Bespoke Infrastructure

**Status:** Accepted

When a repository-oriented coding-agent framework can satisfy the required local development workflow, evaluate and use an established framework before extending or replacing it with bespoke agent infrastructure.

The first framework under evaluation is Aider, using local Ollama inference. The bespoke PowerShell runner remains useful as benchmark and harness history, but its transport/tooling limitations are not a reason by themselves to expand custom infrastructure.

Framework selection does not select a model or role default. Coding-agent results remain subject to repository rules, independent compliance/testing review, human acceptance, and the GitHub `chatgpt` development workflow. Agents must not push GitHub `public` directly.

## D-016 - Framework-First Local Agent Selection

**Status:** Accepted

Use an established free/open-source repository-oriented coding-agent framework before extending or replacing the local development workflow with bespoke agent infrastructure, provided the framework fits the Windows workstation, Ollama, Git/repository instructions, testing, permissions, and project control boundaries.

Framework selection precedes model optimization. Local models are evaluated as part of the selected framework configuration rather than as isolated rankings.

Deterministic repository, branch, permission, and release controls must enforce hard boundaries. The coding model is not the sole authority for project-rule compliance or acceptance. Independent testing/compliance review and human acceptance remain required.

The initial framework discovery set includes Aider, Cline, Roo Code, Continue, OpenHands, and OpenCode. Aider was the first active framework evaluation, followed by Continue and OpenCode. When using Aider for tasks that prohibit agent-created commits, its no-auto-commits configuration must be used.

## D-017 - Do Not Select a Framework From Partial Qualification Evidence

**Status:** Accepted

A coding-agent framework is not selected for the local development workflow unless it passes the common end-to-end qualification requirements from a clean disposable worktree. The qualification must include repository inspection, scoped implementation, focused testing, preservation of unrelated content, ASCII compliance, required validation, Git/branch boundary behavior, and accurate reporting.

Aider, Continue, and OpenCode have been evaluated with `gpt-oss:20b` using the common TASK-AIDER-001 qualification, and OpenCode + `gpt-oss:20b` was additionally tested with TASK-PY-005 and TASK-PY-003. Aider and Continue did not pass TASK-AIDER-001. OpenCode passed TASK-AIDER-001 but did not pass TASK-PY-005 because the focused test did not demonstrate preservation of an actual temporal context and the completion report misstated repository state. OpenCode also did not pass TASK-PY-003 because it could not materialize the required production edit or focused tests after two unsuccessful edit attempts. These results remain end-to-end observations of the tested framework/model configurations and do not establish universal claims about the frameworks or model.

A passed qualification is evidence for the tested framework/model configuration, but the local coding-agent default still requires the remaining documented controls, independent review, low-risk real-repository validation, and human acceptance.

## Source-Controlled Text Encoding

All source-controlled text files must contain ASCII characters only. Non-ASCII Unicode characters, Unicode punctuation, Unicode symbols, and emojis are prohibited. Agents and development tooling must use deterministic UTF-8 handling when reading and writing files and must verify that source-controlled text remains ASCII-only before commit.
