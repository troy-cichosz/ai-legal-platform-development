# Windows Development Workstation

## Baseline

- Windows 11
- RTX 3060 12 GB
- VS Code
- Git/GitHub workflow
- Azure DevOps access
- Existing local development resources

## Role

The workstation is the preferred location for:

- repository checkout;
- coding-agent execution;
- local static analysis/tests where practical;
- local Ollama inference;
- Git diff/review;
- issue/PR preparation.

## Docker

Docker Desktop is not required for the baseline process.

Container builds and deployment remain ADO responsibilities unless a concrete local workflow demonstrates that local Docker is useful.

## GPU / Ollama

The RTX 3060 is available for local model inference.

Model selection is deliberately deferred until the coding-agent workflow and repository task requirements are defined.

Do not assume that a model that fits in VRAM is automatically suitable for repository-scale coding work.
