# devops-lab-deploy Documentation

This repository contains Kubernetes deployment definitions used by GitOps.

## Documentation Index

- [Architecture](architecture.md)
- [Manifest Strategy](manifest-strategy.md)
- [Runbook](runbook.md)

## Main Responsibilities

- Store declarative Kubernetes manifests
- Manage environment overlays (`dev`, `staging`, `prod-blue`, `prod-green`)
- Provide ArgoCD-compatible application definitions
- Track image version promotion
