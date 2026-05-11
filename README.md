# demo-manifests

Kubernetes manifests deployed by ArgoCD for the devops lab.

## Structure

- `apps/demo-ci-cd/`: app reading a static secret from Vault
- `apps/vault-db-demo/`: app using Vault dynamic PostgreSQL credentials
- `vault/`: Vault-related Kubernetes resources

## Applications

### demo-ci-cd
Node.js demo application that authenticates to Vault using Kubernetes auth
and reads a static secret.

### vault-db-demo
Node.js demo application that authenticates to Vault using Kubernetes auth,
requests dynamic PostgreSQL credentials from Vault, and queries the database.

## Deployment
This repository is synced by ArgoCD.

## Namespaces
- `demo`
- `vault`
- `database`

## Troubleshooting

### Vault Kubernetes auth returns 403 permission denied

Symptoms:
- `demo-ci-cd` returns `Vault login failed: 403`
- `vault-db-demo` returns `permission denied`

Possible cause:
- the `vault-auth` ServiceAccount was recreated
- the reviewer token configured in Vault is no longer valid for Kubernetes auth

Fix:

```bash
REVIEWER_JWT=$(kubectl create token vault-auth -n vault)

kubectl exec -n vault -i vault-0 -- sh <<EOF
vault write auth/kubernetes/config \
  token_reviewer_jwt="$REVIEWER_JWT" \
  kubernetes_host="https://kubernetes.default.svc" \
  kubernetes_ca_cert=@/var/run/secrets/kubernetes.io/serviceaccount/ca.crt \
  disable_iss_validation=true
EOF

