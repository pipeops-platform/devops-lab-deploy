# Manifest Strategy

## Repository Structure

```text
apps/
	devops-lab-app/
		base/
			deployment.yaml
			service.yaml
			ingress.yaml
			kustomization.yaml
		overlays/
			dev/
			staging/
			prod/
```

## Baseline Resources

- Deployment
- Service
- Ingress
- Readiness/Liveness probes
- Resource requests/limits

## Overlay Model

- `base` contains portable defaults and shared labels
- `overlays/dev` defines lower footprint and first validation host
- `overlays/staging` increases replicas/resources for pre-prod confidence
- `overlays/prod` defines production-oriented capacity

## Environment Promotion

- `dev` receives first deployment
- `staging` validates release candidate behavior
- `prod` receives approved stable version

## Versioning Model

- Image tags must be immutable
- Promotion updates only manifest references
- Every promotion must be traceable by commit history

## Local Validation Commands

```powershell
# render manifests
kubectl kustomize apps/devops-lab-app/overlays/dev

# apply dev overlay
kubectl apply -k apps/devops-lab-app/overlays/dev

# rollout status
kubectl -n devops-lab-app-dev rollout status deploy/devops-lab-app
```
