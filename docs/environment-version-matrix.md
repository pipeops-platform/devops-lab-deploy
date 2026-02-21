# Environment Version Matrix

This matrix is the canonical operational view of application version targets by environment.

It complements (not replaces) manifest truth in this repository and runtime truth in ArgoCD.

## How to Read

- **Target Image Tag**: image tag declared in deploy manifests for that environment.
- **GitOps Application**: ArgoCD application name responsible for reconciliation.
- **Promotion Source**: from which environment the version is promoted.
- **Status Evidence**: where to verify runtime reconciliation (`Synced/Healthy`).

## Matrix

| Environment | Overlay Path | GitOps Application | Target Image Tag | Promotion Source | Status Evidence |
|---|---|---|---|---|---|
| dev | `apps/devops-lab-app/overlays/dev` | `devops-lab-app-dev` | `sha-7462ef02c1f65208e7e8d9c879563a5b43bb3270` | automatic from `app/main` CI | ArgoCD app + `/health` |
| staging | `apps/devops-lab-app/overlays/staging` | `devops-lab-app-staging` | `sha-7462ef02c1f65208e7e8d9c879563a5b43bb3270` | manual promotion PR from `dev` | ArgoCD app + `/health` |
| prod-blue | `apps/devops-lab-app/overlays/prod-blue` | `devops-lab-app-prod-blue` | `sha-7462ef02c1f65208e7e8d9c879563a5b43bb3270` | manual promotion PR from `staging` | ArgoCD app + `/health` |
| prod-green | `apps/devops-lab-app/overlays/prod-green` | `devops-lab-app-prod-green` | `sha-7462ef02c1f65208e7e8d9c879563a5b43bb3270` | manual promotion PR from `staging` | ArgoCD app + `/health` |
| prod-shared-db | `apps/devops-lab-shared-db/overlays/prod` | `devops-lab-shared-db-prod` | `postgres:16-alpine` | controlled DB lifecycle | ArgoCD app + deployment status |

## Promotion Policy (Current)

- App CI auto-opens promotion PR only for `dev` overlay.
- `staging` and `prod-*` must be promoted by explicit PRs in `devops-lab-deploy`.
- Production exposure remains controlled by blue/green cutover script after slot validation.

## Update Procedure

1. Update image/tag references in the target overlay manifests.
2. Open PR in `devops-lab-deploy` describing promotion intent and validation evidence.
3. Merge PR only after checks and required approvals.
4. Confirm ArgoCD applications are `Synced` and `Healthy`.
5. Refresh this matrix if any environment target, app name, or path changed.

## Quick Verification Commands

```powershell
kubectl -n argocd get applications \
  devops-lab-app-dev \
  devops-lab-app-staging \
  devops-lab-app-prod-blue \
  devops-lab-app-prod-green \
  devops-lab-shared-db-prod \
  -o custom-columns=NAME:.metadata.name,TARGET:.spec.source.targetRevision,SYNC:.status.sync.status,HEALTH:.status.health.status
```

```powershell
curl.exe -s -H "Host: devops-lab-app-dev.localtest.me" http://127.0.0.1/health
curl.exe -s -H "Host: devops-lab-app-staging.localtest.me" http://127.0.0.1/health
curl.exe -s -H "Host: devops-lab-app-prod-blue.localtest.me" http://127.0.0.1/health
curl.exe -s -H "Host: devops-lab-app-prod-green.localtest.me" http://127.0.0.1/health
```
