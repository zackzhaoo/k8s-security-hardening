## Vault Agent Injector - Dynamic Secrets in Kubernetes

### What this does
Demonstrates how the Vault Agent Injector sidecar injects secrets dynamically into a pod at runtime via annotations, removing the need to store credentials in environment variables, Kubernetes Secrets, or container images


### Notes
Static secrets are a cloud security misconfiguration risk
They appear in:
- `kubectl describe pod` output
- docker inspect
- CI/CD logs if printed
- Image layers if baked in at build time

- Vault Agent Injector overcomes these issues by writing secrets to an in-memory tmpfs volume at `/vault/secrets/`
  The credentials never touch disk and totate automatically upon lease expiry


  ### Workflow
1. The Vault Agent Injector webhook intercepts pod creation
2. It injects a Vault Agent init container and sidecar
3. The init container authenticates to Vault using the pod's
   Kubernetes ServiceAccount token (K8s Auth Method)
4. Vault validates the token against the cluster's API server
5. Secrets are written to `/vault/secrets/` as a tmpfs mount
6. The sidecar keeps secrets refreshed as leases renew

### Vault K8s Auth vs static tokens
Never use static Vault tokens in Kubernetes workloads. The K8s Auth
Method binds secret access to a ServiceAccount + namespace + Vault role —
fully auditable, short-lived, and automatically revocable.

### Prerequisites
- Vault cluster running and accessible from the cluster
- Vault Agent Injector installed (included in the Vault Helm chart)
- K8s Auth Method enabled and configured in Vault
- `app-role` Vault role bound to `app-service-account` in `production`

### Apply
kubectl apply -f vault-agent-pod.yaml
