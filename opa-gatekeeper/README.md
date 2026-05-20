## OPA Gatekeeper - Admission Control Policy

### What this does
Defines a ConstraintTemplate using Rego policy logic that blocks any pod from running as root (UID 0 or `runAsNonRoot` unset), and a Constraint that applies it to the `production` namespace at admission time

### Admission controllers vs runtime security
OPA Gatekeeper operates as a ValidatingAdmissionWebhook, intercepts API server requests before resources are created.
Different from runtime tools like Falco or CrowdStrike, which detect anomalies after a container is already running

Admission control = gate
Runtime security = guard

### Notes
Rego is a declarative policy language built for authorization.
The ConstraintTemplate model separate policy logic (the template) from policy application (the constraint), making policies reusable across namespaces and clusters

## Apply
# Install Gatekeeper
kubectl apply -f https://raw.githubusercontent.com/open-policy-agent/gatekeeper/release-3.14/deploy/gatekeeper.yaml

kubectl apply -f constraint-template-no-root.yaml
kubectl apply -f constraint-no-root.yaml

# Test a violation
kubectl run test --image=nginx --overrides='{"spec":{"containers":[{"name":"test","image":"nginx","securityContext":{"runAsUser":0}}]}}'
