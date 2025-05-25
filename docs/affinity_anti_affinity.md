# Affinity and Anti-affinity in Kubernetes

Kubernetes uses **affinity** and **anti-affinity** rules to influence how pods are scheduled onto nodes. These rules help you control pod placement for reasons like high availability, performance, or regulatory requirements.

## Types of Affinity

### 1. Node Affinity

Node affinity allows you to constrain which nodes your pod is eligible to be scheduled on, based on node labels.

**Example: Schedule pods only on nodes with SSDs**
```yaml
spec:
    affinity:
        nodeAffinity:
            requiredDuringSchedulingIgnoredDuringExecution:
                nodeSelectorTerms:
                - matchExpressions:
                    - key: disktype
                        operator: In
                        values:
                        - ssd
```

**Use case:** Run database pods only on nodes with fast SSD storage.

---

### 2. Pod Affinity

Pod affinity allows you to schedule pods to nodes where other specific pods are running.

**Example: Schedule pods on the same node as pods with label `app=frontend`**
```yaml
spec:
    affinity:
        podAffinity:
            requiredDuringSchedulingIgnoredDuringExecution:
            - labelSelector:
                    matchLabels:
                        app: frontend
                topologyKey: "kubernetes.io/hostname"
```

**Use case:** Co-locate microservices that communicate frequently to reduce network latency.

---

### 3. Pod Anti-affinity

Pod anti-affinity prevents pods from being scheduled on the same node as other specified pods.

**Example: Spread pods with label `app=backend` across different nodes**
```yaml
spec:
    affinity:
        podAntiAffinity:
            requiredDuringSchedulingIgnoredDuringExecution:
            - labelSelector:
                    matchLabels:
                        app: backend
                topologyKey: "kubernetes.io/hostname"
```

**Use case:** Ensure replicas of a service are not scheduled on the same node for high availability.

---

## Summary Table

| Type             | Purpose                                 | Example Use Case                       |
|------------------|-----------------------------------------|----------------------------------------|
| Node Affinity    | Schedule pods on specific nodes         | Use SSD nodes for databases            |
| Pod Affinity     | Co-locate pods on same node             | Group microservices for performance    |
| Pod Anti-affinity| Spread pods across nodes                | High availability for replicas         |

---

## References

- [Kubernetes Official Docs: Affinity and Anti-affinity](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/)
- [Kubernetes Node Affinity](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#node-affinity)