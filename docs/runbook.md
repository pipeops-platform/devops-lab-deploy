# Deployment Runbook

## Manual Validation (if needed)

1. Apply manifests to target cluster
2. Validate rollout status
3. Validate service and ingress reachability
4. Confirm no configuration drift

## GitOps Validation

1. Commit manifest changes
2. Confirm ArgoCD sync status
3. Confirm healthy application status

## Definition of Done

Deployment changes are declarative, synced by ArgoCD, and validated in target environment.
