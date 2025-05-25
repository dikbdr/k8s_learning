# Configuring Pod Affinity and Anti-Affinity in Kubernetes

## Problem Statement

You have been assigned a task to configure pod affinity and anti-affinity rules in a Kubernetes cluster to ensure specific deployment patterns of pods across nodes.

---

## What are Pod Affinity and Anti-Affinity?

- **Pod Affinity**: Ensures that pods are scheduled on nodes where certain other pods are running.
- **Pod Anti-Affinity**: Ensures that pods are *not* scheduled on nodes where certain other pods are running.

These rules help control pod placement for high availability, fault tolerance, or workload colocation.

---

## Example: Configuring Pod Affinity

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
    name: web-app
spec:
    replicas: 3
    selector:
        matchLabels:
            app: web
    template:
        metadata:
            labels:
                app: web
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
                - name: web
                    image: nginx
```

*This configuration ensures that `web-app` pods are scheduled on nodes where pods with label `app=backend` are running.*

---

## Example: Configuring Pod Anti-Affinity

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
    name: backend-app
spec:
    replicas: 3
    selector:
        matchLabels:
            app: backend
    template:
        metadata:
            labels:
                app: backend
        spec:
            affinity:
                podAntiAffinity:
                    requiredDuringSchedulingIgnoredDuringExecution:
                        - labelSelector:
                                matchExpressions:
                                    - key: app
                                        operator: In
                                        values:
                                            - backend
                            topologyKey: "kubernetes.io/hostname"
            containers:
                - name: backend
                    image: my-backend-image
```

*This configuration ensures that no two `backend-app` pods are scheduled on the same node.*

---

## Key Fields

- `requiredDuringSchedulingIgnoredDuringExecution`: Hard requirement for scheduling.
- `preferredDuringSchedulingIgnoredDuringExecution`: Soft preference for scheduling.
- `topologyKey`: Defines the domain (e.g., node, zone) for affinity/anti-affinity.

---

## References

- [Kubernetes Official Documentation: Affinity and Anti-Affinity](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#affinity-and-anti-affinity)
- [Pod Affinity and Anti-Affinity Examples](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#affinity-and-anti-affinity)
