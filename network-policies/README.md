## Network Policies - Default Deny + Selective Allow

### What this does
Applies a default-deny policy to block all ingress and egress traffic within the 'production' namespace, then selectively reopens pod communication within the namespace

### Threat target
By default, kubernetes clusters allow unrestricted pod to pod communication.
Without network policies, a compromised workload can reach other services in the cluster, enabling lateral movement and data exfiltration

### Notes
Network policies enforced by CNI plugin
Should be layered with mTLS at application layer for traffic encryption

### Apply
kubectl apply -f default-deny-all.yaml
kubectl apply -f allow-same-namesplace.yaml
