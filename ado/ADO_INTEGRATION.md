# Azure DevOps Integration

## Role

Azure DevOps remains the CI/CD and operational verification authority.

The existing self-hosted agents, registry, scanning, and deployment environment should be reused where practical rather than duplicated.

## Existing Pipeline Baseline — Reviewed September 22, 2026

The current `azure-pipelines.yaml` files were reviewed from the `public` branches of all five edge repositories:

- `edge-controller`
- `edge-time`
- `edge-gps`
- `edge-video`
- `edge-audio`

All five currently use a two-job pattern: `BuildAndPush` followed by `Push`.

`BuildAndPush` uses self-hosted ARM or x86 agents, reads the `GitHub` variable group, builds Docker images for the local registry `docker.spoocannon.com:5000`, performs the current Trivy scanning steps, pushes images to the registry, and publishes metadata/files/scans as ADO build artifacts.

`Push` uses a self-hosted `Linux x86 64bit` agent, creates an orphan branch named by `branch2Push`, removes selected files, and force-pushes that branch to GitHub using SSH authentication.

The existing service YAML is reference material during the automation migration. It remains unchanged unless a separate increment explicitly changes a service pipeline.

The two management/control repositories now use `azure-pipelines.yaml` for their sync-only public-maintenance pipeline:

- `ai-legal-platform-development`
- `edge-platform-automation`

These pipelines do not perform service Docker builds. They synchronize the ADO `chatgpt` working tree to GitHub `public` using the established ADO-to-GitHub maintenance mechanism.

## Approved Architecture

```
GitHub repository/chatgpt
       |
       | GitHub push webhook
       v
edge-platform-automation - CI
       |
       | identify repo/ref/SHA
       | retrieve exact GitHub chatgpt source
       | synchronize matching ADO repository/chatgpt
       | create ADO sync commit
       v
ADO repository/chatgpt
       |
       +--> build_service --> existing service CI/CD
       |
       +--> sync_only --> azure-pipelines.yaml public maintenance
       |
       v
build / deploy or public maintenance
```

GitHub `chatgpt` is authoritative for source and history. ADO `chatgpt` is an operational mirror. Synchronization replaces the ADO working tree, including removal of files deleted in GitHub.

## Repository Mapping

| GitHub repository | Class | ADO project | ADO repository | ADO branch | Downstream |
|---|---|---|---|---|---|
| `troy-cichosz/edge-controller` | `build_service` | `Docker` | `edge-controller` | `chatgpt` | `edge-controller - CI` |
| `troy-cichosz/edge-time` | `build_service` | `Docker` | `edge-time` | `chatgpt` | `edge-time - CI` |
| `troy-cichosz/edge-gps` | `build_service` | `Docker` | `edge-gps` | `chatgpt` | `edge-gps - CI` |
| `troy-cichosz/edge-video` | `build_service` | `Docker` | `edge-video` | `chatgpt` | `edge-video - CI` |
| `troy-cichosz/edge-audio` | `build_service` | `Docker` | `edge-audio` | `chatgpt` | `edge-audio - CI` |
| `troy-cichosz/ai-legal-platform-development` | `sync_only` | `Docker` | `ai-legal-platform-development` | `chatgpt` | `azure-pipelines.yaml` public maintenance |
| `troy-cichosz/edge-platform-automation` | `sync_only` | `Docker` | `edge-platform-automation` | `chatgpt` | `azure-pipelines.yaml` public maintenance |

## Increment B — Verified Pilot

The `edge-gps` pilot proved:

1. GitHub `chatgpt` push reaches automation.
2. Repository/ref/SHA are identified.
3. Exact source is retrieved and verified.
4. Complete source tree is synchronized to ADO `edge-gps/chatgpt`.
5. Stale files are removed.
6. ADO synchronization commit is pushed and verified.
7. Existing `edge-gps - CI` triggers from the ADO branch change.
8. Existing service CI/CD remains unchanged.
9. The authoritative source must contain the required service pipeline definition before complete-tree synchronization.

## Increment C — Expansion and Hardening

Increment C expands the pilot to all seven covered repositories.

Implemented:

- repository registry and classification;
- complete-tree synchronization;
- exact GitHub SHA verification;
- remote ADO SHA verification;
- build-service preflight for `azure-pipelines.yaml`;
- sync-only downstream public-maintenance handling;
- `chatgpt`-only acceptance at the automation boundary;
- explicit recursion boundary for `edge-platform-automation`;
- preservation of existing service CI/CD.

The expanded workflow has been validated across all seven covered repositories. Validation established exact GitHub `chatgpt` SHA handling, correct repository classification and ADO targeting, idempotent synchronization, complete-tree deletion propagation, non-`chatgpt` no-op behavior, the explicit `edge-platform-automation` recursion boundary, and GitHub SHA → ADO synchronization SHA correlation.

A controlled `edge-audio` `chatgpt` change also verified that a changed ADO mirror triggers the existing service CI/CD path and the downstream GitHub `public` maintenance.

Increment C hardening verification is complete. Existing service pipeline definitions remain unchanged.

## Trigger Behavior

The webhook pipeline receives GitHub push events and performs a pipeline-level branch check. Only `refs/heads/chatgpt` is synchronized. Other supported repository push events complete as no-ops and do not modify ADO.

A `public` push from the sync-only maintenance pipeline can therefore produce another webhook event, but that event is rejected by the `chatgpt` branch check and does not recurse into another synchronization.

## Verification Contract

For each affected repository, distinguish:

1. source synchronization;
2. downstream pipeline behavior;
3. build/deployment where applicable;
4. public maintenance where applicable;
5. runtime verification where applicable;
6. documentation update.

A successful synchronization or build is not runtime proof.

## Promotion Gate

Only verified state should become the `public` release-candidate baseline.

The existing ADO orphan-branch/force-push behavior remains during migration so current service and management-repository maintenance behavior is not disrupted. A separate increment will define and verify the long-term promotion mechanism.

## Security

Do not place credentials, private keys, passwords, PATs, or secret variable values in this repository.

The existing ADO GitHub authentication mechanism may be reused, replaced, or migrated without exposing its secret material.

## Migration Rule

Do not remove or modify existing working service pipelines until the replacement workflow has been implemented and verified.

The current sequence is:

1. Verify Increment B pilot.
2. Expand/harden synchronization in Increment C.
3. Establish the local coding-agent/Ollama workflow.
4. Define automated runtime/integration verification.
5. Define and verify controlled `public` promotion.
6. Retire or repurpose legacy automatic mirroring only after the replacement is proven.
