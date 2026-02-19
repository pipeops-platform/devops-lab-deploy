# Manifest Strategy

## Baseline Resources

- Deployment
- Service
- Ingress
- ConfigMap/Secret references

## Environment Promotion

- `dev` receives first deployment
- `staging` validates release candidate behavior
- `prod` receives approved stable version

## Versioning Model

- Image tags must be immutable
- Promotion updates only manifest references
- Every promotion must be traceable by commit history
