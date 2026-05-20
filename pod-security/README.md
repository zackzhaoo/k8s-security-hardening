## Pod Security - Restricted profile + hardened container spec

### What this does
Enforces Kubernetes Pod Security standards at the 'restricted' profile  on the 'production' namespace
Demonstrates a hardened pod spec that explicitly blocks harmful container configurations

### Field-by-field threat mapping
| Field | Threat mitigated |
|---|---|
| `runAsNonRoot: true` | Prevents container breakout with root UID |
| `allowPrivilegeEscalation: false` | Blocks setuid/setgid privilege escalation |
| `readOnlyRootFilesystem: true` | Limits persistence after compromise |
| `capabilities: drop: ALL` | Removes Linux kernel capabilities (e.g. NET_RAW) |
| `seccompProfile: RuntimeDefault` | Restricts syscall surface |
| `hostPID / hostNetwork` (absent) | Prevents host namespace access |

### Pod Security Standards vs PodSecurityPolicy
PSP was deprecated in K8s 1.21 and removed in 1.25. PSS (enforced via
admission control labels on namespaces) is the current replacement.
OPA/Gatekeeper provides more granular policy control beyond what PSS offers.

### Apply
kubectl apply -f restricted-pss.yaml
kubectl apply -f no-privileged-containers.yaml
