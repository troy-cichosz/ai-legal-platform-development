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

The existing YAML is reference material during the automation migration. It remains unchanged unless a separate increment explicitly changes a service pipeline.

## Approved Architecture

```
GitHub service/chatgpt
       |
       | GitHub push webhook
       v
edge-platform-automation - CI
       |
       | identify repo/ref/SHA
       | retrieve exact GitHub source
       | synchronize matching ADO service/chatgpt
       | create ADO sync commit
       v
ADO service/chatgpt
       |
       | existing branch-change trigger
       v
existing edge-<service> - CI/CD
       |
       v
build / scan / registry / deployment
       |
       v
runtime verification
```

GitHub `chatgpt` is authoritative for source and history. ADO service `chatgpt` is an operational build mirror. Synchronization replaces the ADO working tree, including removal of files deleted in GitHub.

## Service Repository Mapping

| GitHub repository | ADO project | ADO repository | ADO branch | Existing CI |
|---|---|---|---|---|
| `troy-cichosz/edge-controller` | `Docker` | `edge-controller` | `chatgpt` | `edge-controller - CI` |
| `troy-cichosz/edge-time` | `Docker` | `edge-time` | `chatgpt` | `edge-time - CI` |
| `troy-cichosz/edge-gps` | `Docker` | `edge-gps` | `chatgpt` | `edge-gps - CI` |
| `troy-cichosz/edge-video` | `Docker` | `edge-video` | `chatgpt` | `edge-video - CI` |
| `troy-cichosz/edge-audio` | `Docker` | `edge-audio` | `chatgpt` | `edge-audio - CI` |

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
9. Automation does not directly modify GitHub `public`.

A service's required ADO pipeline definition must exist in the authoritative GitHub source before complete-tree synchronization, because synchronization intentionally removes files absent from GitHub.

## Increment C — Expansion and Hardening

The next automation increment is to expand the proven pilot to all remaining covered services and harden the workflow.

Required checks:

- each service has its expected `azure-pipelines.yaml` on GitHub `chatgpt`;
- each service can synchronize into ADO `chatgpt`;
- each synchronized ADO branch triggers the existing service CI;
- deletion propagation works;
- unchanged source is idempotent;
- non-`chatgpt` webhook events do not modify ADO;
- GitHub and ADO revisions remain correlated.

The automation may add preflight validation, but it must not replace the service CI trigger with direct queueing during this increment.

## Trigger Behavior

The webhook pipeline currently receives GitHub push events and performs a pipeline-level branch check. Only `refs/heads/chatgpt` is synchronized. Other supported repository push events complete as no-ops and do not modify ADO.

## Verification Contract

For each affected service, distinguish:

1. source synchronization;
2. ADO CI trigger;
3. build;
4. scan/test;
5. artifact/image publication;
6. deployment;
7. runtime verification;
8. cross-service verification where applicable;
9. documentation update.

A successful synchronization or build is not runtime proof.

## Promotion Gate

Only verified state should become the `public` release-candidate baseline.

The existing automatic ADO orphan-branch/force-push behavior remains during migration so current service release behavior is not disrupted. A separate increment will define and verify the long-term promotion mechanism.

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
