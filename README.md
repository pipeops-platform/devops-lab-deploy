# devops-lab-deploy

Kubernetes deployment repository for the DevOps Hybrid Lab.

## Purpose

This repository contains declarative Kubernetes definitions used by GitOps and release promotion.

## Documentation

- [Documentation Home](docs/README.md)
- [Architecture](docs/architecture.md)
- [Manifest Strategy](docs/manifest-strategy.md)
- [Runbook](docs/runbook.md)

## Integration

- ArgoCD reconciles changes from this repository into target clusters
- CI pipelines update image references here after successful builds
- Project-level guidance is maintained in `devops-lab-hub`