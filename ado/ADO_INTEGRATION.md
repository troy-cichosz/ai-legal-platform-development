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

The current pipeline variables have been clarified:

- `branch2Push` = GitHub `public`.
- `repoName` = corresponding GitHub repository name.
- `gitCommit` = common commit message used when ADO updates `public`.
- Git/SSH files and keys = authentication material allowing ADO to push to GitHub. Secret values are intentionally excluded from this documentation.
- Agent pools = existing self-hosted PIs and Linux/x86 systems.

The existing YAML is therefore sufficient to document the legacy build/mirroring mechanics. It is reference material, not a requirement for the replacement design.

## Approved Migration Architecture

The migration preserves the existing service CI/CD:

```
GitHub service/chatgpt
       |
       v
edge-platform-automation - CI
       |
       | pull source
       | synchronize matching ADO service/chatgpt
       | create ADO mirror commit
       v
ADO service/chatgpt
       |
       v
existing edge-<service> - CI/CD
```

GitHub `chatgpt` is authoritative for source and history. ADO service `chatgpt` is an operational build mirror. The synchronization should make the ADO working tree match GitHub, including deletion of files no longer present in GitHub.

## Service Repository Mapping

| GitHub repository | ADO project | ADO repository | ADO branch | Existing CI |
|---|---|---|---|---|
| `troy-cichosz/edge-controller` | `Docker` | `edge-controller` | `chatgpt` | `edge-controller - CI` |
| `troy-cichosz/edge-time` | `Docker` | `edge-time` | `chatgpt` | `edge-time - CI` |
| `troy-cichosz/edge-gps` | `Docker` | `edge-gps` | `chatgpt` | `edge-gps - CI` |
| `troy-cichosz/edge-video` | `Docker` | `edge-video` | `chatgpt` | `edge-video - CI` |
| `troy-cichosz/edge-audio` | `Docker` | `edge-audio` | `chatgpt` | `edge-audio - CI` |

## Trigger

The automation pipeline does not queue or replace service CI. It updates the matching ADO `chatgpt` branch and lets the existing branch-change trigger run normally.

## Synchronization

The automation uses the existing GitHub SSH mechanism for source access and the ADO pipeline OAuth/System.AccessToken for writes to the target ADO repository.

The synchronization:
1. retrieves GitHub `chatgpt`;
2. replaces the ADO working tree;
3. removes stale files;
4. commits the synchronized tree in ADO;
5. pushes ADO `chatgpt`;
6. records GitHub and ADO revisions for correlation.

The ADO commit history is not authoritative.

## Migration Rule

Do not modify existing service pipelines for this increment. Pilot with `edge-gps`, verify the ADO branch change triggers `edge-gps - CI`, then expand to the remaining services.
## Trigger

A change to the GitHub `chatgpt` branch should initiate the applicable ADO validation workflow.

The implementation should use the existing GitHub/ADO integration if it satisfies this requirement; otherwise a new GitHub/ADO integration may be created.

The trigger must:

- identify the correct repository;
- identify the intended development branch;
- build the triggering revision;
- avoid treating `public` as the normal development trigger;
- provide an observable ADO run associated with the GitHub change.

## Build

Each service pipeline should retain only the service-specific behavior required by the service.

The existing YAML establishes useful baseline requirements:

- self-hosted Linux agents;
- Docker build/push;
- local registry `docker.spoocannon.com:5000`;
- service-specific architecture requirements;
- versioning from `ver.txt`;
- Trivy scanning;
- ADO artifacts.

The replacement should preserve these requirements unless a deliberate improvement is documented.

## Deployment

Deployment should use the existing test environment where possible.

A deployment must identify:

- source revision;
- image/artifact version;
- target node/environment;
- deployment result.

Do not infer runtime success from a successful Docker build or registry push.

## Verification

For an affected repository:

1. Source checkout.
2. Static/unit tests as applicable.
3. Container build.
4. Security scan.
5. Artifact/image publication.
6. Deployment to the appropriate test target.
7. Health/functional verification.
8. Cross-service verification when applicable.
9. Failure/recovery verification when required.
10. Preserve build/deployment/runtime evidence.
11. Report the candidate as verified or failed.

Verification must be appropriate to the service. For example, edge services that depend on local hardware or `edge-time` require runtime checks that cannot be established from source compilation alone.

## Promotion Gate

Only verified state should become the `public` release-candidate baseline.

The current automatic ADO orphan-branch/force-push behavior should not be carried forward automatically merely because it exists today.

The replacement promotion mechanism should be a distinct release operation with an explicit relationship to the verified source revision.

The exact mechanism—manual promotion, protected branch/PR, or another controlled ADO/GitHub operation—will be selected during implementation.

## Security

Do not place credentials, private keys, passwords, PATs, or secret variable values in this repository.

The existing ADO GitHub authentication mechanism may be reused, replaced, or migrated without exposing its secret material.

## Migration Rule

Do not remove or modify the existing working pipelines until the replacement workflow has been implemented and verified.

Migration should be:

1. Define replacement pipeline.
2. Implement.
3. Test against a non-destructive change.
4. Verify build/deployment/runtime behavior.
5. Verify `chatgpt` → ADO triggering.
6. Verify controlled `public` promotion.
7. Only then retire or repurpose legacy mirroring behavior.
