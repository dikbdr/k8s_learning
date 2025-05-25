# Inter-Pod Affinity and Anti-affinity in Kubernetes

Kubernetes **Inter-Pod Affinity** and **Anti-affinity** allow you to control how pods are scheduled relative to other pods. These features help ensure high availability, fault tolerance, and efficient resource utilization.

---

## 1. Inter-Pod Affinity

**Inter-Pod Affinity** lets you specify that a pod should be scheduled on a node only if certain other pods are already running on that node (or in the same topology domain).

### Example: Schedule pods together

Suppose you want all pods with label `app=frontend` to be scheduled on nodes where pods with label `app=backend` are running.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
    name: frontend
spec:
    replicas: 3
    selector:
        matchLabels:
            app: frontend
    template:
        metadata:
            labels:
                app: frontend
        spec:
            affinity:
                podAffinity:
                    requiredDuringSchedulingIgnoredDuringExecution:
                        - labelSelector:
                                matchExpressions:
                                    - key: app
                                        operator: In
                                        values:
                                            - backend
                            topologyKey: "kubernetes.io/hostname"
            containers:
                - name: frontend
                    image: nginx
```

- **`topologyKey`**: Defines the domain (e.g., node, zone) for affinity. `"kubernetes.io/hostname"` means pods will be co-located on the same node.

---

## 2. Inter-Pod Anti-affinity

**Inter-Pod Anti-affinity** ensures that pods are **not** scheduled on nodes where certain other pods are running.

### Example: Spread pods across nodes

Suppose you want to avoid scheduling multiple pods with label `app=frontend` on the same node for high availability.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
    name: frontend
spec:
    replicas: 3
    selector:
        matchLabels:
            app: frontend
    template:
        metadata:
            labels:
                app: frontend
        spec:
            affinity:
                podAntiAffinity:
                    requiredDuringSchedulingIgnoredDuringExecution:
                        - labelSelector:
                                matchExpressions:
                                    - key: app
                                        operator: In
                                        values:
                                            - frontend
                            topologyKey: "kubernetes.io/hostname"
            containers:
                - name: frontend
                    image: nginx
```

- This ensures that no two `frontend` pods are scheduled on the same node.

---

## 3. Soft vs. Hard Affinity/Anti-affinity

- **requiredDuringSchedulingIgnoredDuringExecution**: **Hard** requirement. Scheduler must satisfy this rule.
- **preferredDuringSchedulingIgnoredDuringExecution**: **Soft** preference. Scheduler will try to satisfy but may ignore if not possible.

### Example: Soft Affinity

```yaml
affinity:
    podAffinity:
        preferredDuringSchedulingIgnoredDuringExecution:
            - weight: 1
                podAffinityTerm:
                    labelSelector:
                        matchExpressions:
                            - key: app
                                operator: In
                                values:
                                    - backend
                    topologyKey: "kubernetes.io/hostname"
```

---

## 4. Use Cases

- **Affinity**: Co-locate tightly coupled services (e.g., web and cache).
- **Anti-affinity**: Spread replicas for high availability.

---

## References

- [Kubernetes Official Docs: Affinity and Anti-affinity](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#affinity-and-anti-affinity)
