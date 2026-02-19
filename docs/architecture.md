# Deployment Architecture

## Objective

Define a declarative, auditable deployment model for multi-environment Kubernetes delivery.

## Planned Structure

- `apps/` application manifests
- `environments/` overlays per environment
- `argocd/` ArgoCD application definitions

## Design Principles

- Git is the source of truth
- Declarative configuration only
- Environment separation with controlled promotion
- No manual drift in cluster state
