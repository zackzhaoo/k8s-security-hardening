# Kubernetes Security Hardening Reference

collection of production-ready k8s security YAML manifests
demonstrating defense-in-depth across identity, network, workload, and secrets management control domains

Built as a refrence for teams securing mission-critical workloads on Kubernetes
Each control is documented with the threat it targets/mitigates along with its mapping to NIST 800-53 and MITRE ATT&CK

## Architecture 
Admission layer: OPA Gatekeeper (blocks non-compliant resources at creation)

Identity layer: RBAC (least-privilege roles scoped to namespace + SA)

Network layer: Network Policies (default deny + selective allow)

Workload layer: Pod Security Standards + hardened container spec

Secrets layer: HashiCorp Vault Agent Injector (dynamic, zero-static-secret)

## Control Domains

| Folder | Control | Framework |
|---|---|---|
| `rbac/` | Least-privilege namespace-scoped roles | NIST AC-6, MITRE T1078 |
| `network-policies/` | Default deny + selective allow | NIST SC-7, MITRE T1021 |
| `pod-security/` | Restricted PSS + hardened container spec | NIST CM-7, MITRE T1611 |
| `opa-gatekeeper/` | Admission control — block root containers | NIST SI-7, MITRE T1548 |
| `vault-integration/` | Dynamic secrets injection via Vault Agent | NIST IA-5, SC-28 |

## Design Principles

- **Default deny, then selectively allow** — every control starts
  from zero trust and opens only what is explicitly required
- **Defense-in-depth** — no single control is sufficient; each layer
  catches what the layer above it misses
- **Admission over runtime where possible** — blocking misconfigured
  workloads at creation is cheaper than detecting them at runtime
- **No static secrets** — credentials are never stored in environment
  variables, image layers, or unencrypted Kubernetes Secrets

## How to Use

Each folder is self-contained with its own README explaining the
threat model, framework mappings, and apply instructions.
Controls are designed to be layered — apply all five for full coverage,
or adopt individual controls incrementally.

## Prerequisites

- Kubernetes 1.25+ (for Pod Security Standards)
- OPA Gatekeeper installed for `opa-gatekeeper/` controls
- HashiCorp Vault with K8s Auth Method for `vault-integration/`
