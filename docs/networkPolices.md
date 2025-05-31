# Kubernetes Network Policies

## What are Network Policies?

Network Policies in Kubernetes are resources that control the traffic flow between pods, namespaces, and external endpoints. They act as firewalls for pods, specifying how groups of pods are allowed to communicate with each other and with other network endpoints.

## Types of Network Policies

Network Policies can control:
- **Ingress traffic**: Incoming connections to pods.
- **Egress traffic**: Outgoing connections from pods.
- **Both ingress and egress**: Combined control.

## NetworkPolicy Resource

A `NetworkPolicy` is a Kubernetes resource defined in YAML. It uses labels to select pods and specifies rules for allowed traffic.

### Example: Allow Ingress from Specific Namespace

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
    name: allow-frontend
spec:
    podSelector:
        matchLabels:
            role: backend
    ingress:
    - from:
        - namespaceSelector:
                matchLabels:
                    name: frontend
```
**Use case:** Only pods in the `frontend` namespace can access pods with the label `role: backend`.

### Example: Deny All Traffic

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
    name: deny-all
spec:
    podSelector: {}
    policyTypes:
    - Ingress
    - Egress
```
**Use case:** Deny all ingress and egress traffic to all pods in the namespace unless explicitly allowed by another policy.

### Example: Allow Egress to External Database

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
    name: allow-db-egress
spec:
    podSelector:
        matchLabels:
            app: myapp
    egress:
    - to:
        - ipBlock:
                cidr: 10.0.0.5/32
    policyTypes:
    - Egress
```
**Use case:** Allow pods with label `app: myapp` to connect only to an external database at `10.0.0.5`.

## Summary

Network Policies help secure Kubernetes clusters by controlling pod communication. They are essential for multi-tenant environments and for meeting security requirements.