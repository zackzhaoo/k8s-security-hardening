## RBAC - LPA

### What this does
Defines a role scoped to the 'production' namespace, granting read-only access to pods, configmaps, and deployments. Bound to a ServiceAccount, not a user or group

### Threat target
Without scoped RBAC, a compromised pod or misconfigured workload can enumerate cluster resources, exfiltrate secrets, or modify deployments.
Having this in place will contain the blast radius to read-only operations within a singular namespace.

### Notes
Opted to use Role instead of ClusterRoles, as ClusterRoles apply cluster-wide.
Unless a workload needs cross-namespace access, Role is preferred, accordingly to principle of least privilege

## Apply
kubectl apply -f least-privilege-role.yaml
kubectl apply -f role-binding.yaml
