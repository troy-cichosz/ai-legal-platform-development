# Azure DevOps Integration

## Role

Azure DevOps remains the CI/CD and operational verification authority.

The existing self-hosted agents and deployment environment should be reused rather than duplicated.

## Current Pipeline Baseline — Reviewed September 22, 2026

The current `azure-pipelines.yaml` files were reviewed from the `public` branches of all five edge repositories:

- `edge-controller`
- `edge-time`
- `edge-gps`
- `edge-video`
- `edge-audio`

All five currently use a two-job pattern: `BuildAndPush` followed by `Push`.

`BuildAndPush` uses a self-hosted ARM or x86 agent, reads the `GitHub` variable group, builds Docker images for the local registry `docker.spoocannon.com:5000`, performs the current Trivy scanning steps, pushes images to the registry, and publishes metadata/files/scans as ADO build artifacts.

`Push` uses a self-hosted `Linux x86 64bit` agent, creates an orphan branch named by the hidden `branch2Push` pipeline variable, removes selected files, and force-pushes that branch to GitHub using an SSH key from secure pipeline configuration.

Important findings:

- The current YAML is **not yet a `chatgpt` → validate → `public` workflow**.
- ADO currently contains an explicit GitHub force-push/mirroring step. The actual value of `branch2Push` must be verified in ADO.
- The YAML has no explicit `chatgpt`/`public` branch logic. It treats `master` specially and all other source branch names as `develop` for image naming/versioning.
- All five pipelines use `trigger: '*.*'`; effective trigger behavior must be verified against the ADO pipeline/repository configuration.
- No YAML templates are referenced by these five files.
- The `GitHub` variable group is used by all five pipelines; its secret values were not retrieved.
- The YAML itself establishes build/package/publish and GitHub mirroring, but does not establish Pi deployment or runtime/integration verification.
- Repository-specific differences include: `edge-controller` builds x86; `edge-time` uses Buildx for amd64/arm64; `edge-video` currently skips the non-master Trivy scan; `edge-gps` and `edge-audio` use the standard single-platform build/push pattern.

These are inventory findings only; no pipeline behavior has been changed.

## Required Inventory

Before changing pipelines, record:

- ADO organization/project;
- repositories connected to each pipeline;
- pipeline names/IDs;
- YAML versus classic pipeline usage;
- repository trigger configuration;
- branch filters;
- self-hosted agent pools;
- agent capabilities;
- Docker availability;
- registry/service connections;
- deployment targets;
- test environments;
- integration-test scripts;
- current promotion/mirroring mechanism;
- secrets/service connections involved.

## Desired Trigger

A change to the relevant GitHub development branch should initiate the appropriate ADO validation workflow.

The exact trigger mechanism must be taken from the existing environment or implemented deliberately after inventory. Do not assume a particular GitHub/ADO integration is already configured.

## Desired Verification

For an affected repository:

1. Source checkout.
2. Static/unit tests as applicable.
3. Container build.
4. Artifact identification.
5. Deployment to the test target.
6. Health/functional verification.
7. Cross-service verification when applicable.
8. Failure/recovery verification when required.
9. Preserve build/deployment evidence.
10. Report result to the development workflow.

## Promotion Gate

Only verified state should become the `public` release-candidate baseline.

ADO does not become the source of truth for source code; GitHub remains authoritative.
