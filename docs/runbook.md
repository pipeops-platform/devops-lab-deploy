# Deployment Runbook

## Phase 4 Local Execution (k3d)

1. Ensure target image exists locally

```powershell
docker images pipeops/devops-lab-app
```

2. Import image into k3d cluster

```powershell
k3d image import pipeops/devops-lab-app:0.3.0 -c devops-lab-local
```

3. Apply `dev` overlay

```powershell
kubectl apply -k apps/devops-lab-app/overlays/dev
```

4. Validate rollout and resources

```powershell
kubectl -n devops-lab-app-dev get deploy,po,svc,ingress,pvc,secret
kubectl -n devops-lab-app-dev rollout status deploy/devops-lab-app
kubectl -n devops-lab-app-dev rollout status deploy/devops-lab-postgres
```

5. Validate ingress access

```powershell
curl.exe -s -H "Host: devops-lab-app-dev.localtest.me" http://127.0.0.1/health
```

Expected response:

```text
{"status":"ok"}
```

## Manual Validation (if needed)

1. Apply manifests to target cluster
2. Validate rollout status
3. Validate service and ingress reachability
4. Confirm no configuration drift

## Blue/Green Shared DB Validation

1. Apply shared production database stack

```powershell
kubectl apply -k apps/devops-lab-shared-db/overlays/prod
kubectl -n devops-lab-app-prod-data rollout status deploy/devops-lab-postgres-shared
```

2. Apply both production slots

```powershell
kubectl apply -k apps/devops-lab-app/overlays/prod-blue
kubectl apply -k apps/devops-lab-app/overlays/prod-green
```

3. Validate both slots and live endpoint

```powershell
curl.exe -s -H "Host: devops-lab-app-prod-blue.localtest.me" http://127.0.0.1/health
curl.exe -s -H "Host: devops-lab-app-prod-green.localtest.me" http://127.0.0.1/health
curl.exe -s -H "Host: devops-lab-app-prod.localtest.me" http://127.0.0.1/health
```

## GitOps Validation

1. Commit manifest changes
2. Confirm ArgoCD sync status
3. Confirm healthy application status

## Environment Version Tracking

Use `docs/environment-version-matrix.md` as the operational snapshot of target versions per environment.

During each promotion:

1. Update overlay references in manifests.
2. Verify matrix entries are still accurate (tag, overlay path, app name).
3. Include matrix validation evidence in PR description.
4. After merge, confirm ArgoCD `Synced/Healthy` for all affected applications.

## Promotion Between Environments (Deploy Repo)

Use workflow `Promote Environment` in this repository to promote the same built image between overlays without rebuilding.

Allowed paths:

- `dev -> staging`
- `staging -> prod-blue`
- `staging -> prod-green`

Execution:

1. Open Actions in `devops-lab-deploy`.
2. Run `Promote Environment`.
3. Select `source_env` and `target_env`.
4. Review generated PR with overlay diff (image `name` + `newTag`).
5. Merge after required approvals/checks.
6. Confirm ArgoCD for target app is `Synced` and `Healthy`.

## Definition of Done

Deployment changes are declarative, synced by ArgoCD, and validated in target environment.
