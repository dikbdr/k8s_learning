## Network Policy Attributes

A Kubernetes NetworkPolicy defines how groups of pods are allowed to communicate with each other and with other network endpoints. The main attributes of a NetworkPolicy are:

- **podSelector**: Selects the pods to which the policy applies.
- **policyTypes**: Specifies whether the policy applies to ingress, egress, or both.
- **ingress**: Defines rules for incoming traffic to the selected pods.
- **egress**: Defines rules for outgoing traffic from the selected pods.

### Example

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
    name: allow-nginx
spec:
    podSelector:
        matchLabels:
            app: nginx
    policyTypes:
    - Ingress
    ingress:
    - from:
        - podSelector:
                matchLabels:
                    access: "true"
```

---

## Default Policies

By default, if there are **no NetworkPolicies** applied to a namespace, all pods can communicate with each other without restrictions. Once a NetworkPolicy is applied to a pod, all traffic is **denied by default** except what is explicitly allowed by the policy.

---

## Selectors

Selectors are used to specify which pods or namespaces the policy applies to:

- **podSelector**: Selects pods using labels.
- **namespaceSelector**: Selects namespaces using labels.
- **ipBlock**: Selects IP ranges.

### Example with Selectors

```yaml
spec:
    podSelector:
        matchLabels:
            role: db
    ingress:
    - from:
        - namespaceSelector:
                matchLabels:
                    project: myproject
        - podSelector:
                matchLabels:
                    access: "frontend"
        - ipBlock:
                cidr: 172.17.0.0/16
                except:
                - 172.17.1.0/24
```

- **podSelector**: Applies policy to pods with `role: db`.
- **namespaceSelector**: Allows traffic from namespaces with `project: myproject`.
- **ipBlock**: Allows traffic from a specific IP range, except a subrange.
