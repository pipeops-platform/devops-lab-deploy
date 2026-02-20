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

## GitOps Validation

1. Commit manifest changes
2. Confirm ArgoCD sync status
3. Confirm healthy application status

## Definition of Done

Deployment changes are declarative, synced by ArgoCD, and validated in target environment.
