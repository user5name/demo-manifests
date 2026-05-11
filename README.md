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

