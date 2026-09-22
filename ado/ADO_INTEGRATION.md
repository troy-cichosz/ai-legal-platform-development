# Azure DevOps Integration

## Role

Azure DevOps remains the CI/CD and operational verification authority.

The existing self-hosted agents and deployment environment should be reused rather than duplicated.

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
