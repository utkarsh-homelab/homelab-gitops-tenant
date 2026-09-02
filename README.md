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

## Domain

- **Domain**: `iresharma.com`
- **Wildcard**: `*.iresharma.com` → 192.168.0.200 (Traefik)
- **TLS Certificate**: `wildcard-iresharma-tls` (Let's Encrypt)
- **ClusterIssuer**: `letsencrypt-prod-iresh`

## Ingress Template

When creating Ingress resources, use:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: your-app
  namespace: iresharma
  annotations:
    cert-manager.io/cluster-issuer: letsencrypt-prod-iresh
spec:
  ingressClassName: traefik
  rules:
    - host: your-app.iresharma.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: your-app
                port:
                  number: 80
  tls:
    - hosts:
        - your-app.iresharma.com
      secretName: wildcard-iresharma
```

## Restrictions

- Cannot create new namespaces
- Cannot access secrets
- Cannot deploy to infra namespaces (monitoring, argocd, etc.)