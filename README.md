# homelab-gitops-tenant

GitOps repository for tenant's applications.

## Structure

```
apps/
└── example-nginx.yaml/    # example application
```

## Allowed Namespaces

- `iresharma`

## How to Deploy

1. Add your application manifests to the app directory
2. Push to the `main` branch
3. ArgoCD will automatically sync the changes

## Restrictions

- Cannot create new namespaces
- Cannot access secrets
- Cannot deploy to infra namespaces (monitoring, argocd, etc.)