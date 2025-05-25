## Pod Disruption Budgets in Kubernetes

A **Pod Disruption Budget (PDB)** is a Kubernetes resource that helps you limit the number of pods of a replicated application that can be voluntarily disrupted at a time. Voluntary disruptions include actions like node draining for maintenance, cluster upgrades, or manual pod evictions. PDBs ensure that your application maintains a minimum level of availability during such operations.

### Why Use Pod Disruption Budgets?

Without a PDB, Kubernetes might evict too many pods simultaneously, causing downtime or degraded service. PDBs let you specify how many pods must always be available, protecting your workloads from excessive voluntary disruptions.

### How PDBs Work

A PDB defines either:
- `minAvailable`: The minimum number of pods that must be available after a disruption.
- `maxUnavailable`: The maximum number of pods that can be unavailable during a disruption.

Kubernetes uses PDBs to decide whether to allow voluntary disruptions. If a disruption would violate the PDB, Kubernetes blocks it.

### Example: Defining a Pod Disruption Budget

Suppose you have a deployment named `my-app` with 5 replicas. You want to ensure at least 4 pods are always available.

```yaml
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
    name: my-app-pdb
spec:
    minAvailable: 4
    selector:
        matchLabels:
            app: my-app
```

- `minAvailable: 4` ensures that at least 4 pods are running at any time.
- The `selector` matches pods with the label `app: my-app`.

Alternatively, you can use `maxUnavailable`:

```yaml
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
    name: my-app-pdb
spec:
    maxUnavailable: 1
    selector:
        matchLabels:
            app: my-app
```

- `maxUnavailable: 1` allows only one pod to be disrupted at a time.

### Key Points

- PDBs only affect **voluntary** disruptions (e.g., node drain, eviction). They do **not** protect against involuntary disruptions (e.g., node crashes).
- PDBs work with controllers like Deployments, StatefulSets, and ReplicaSets.
- If you set a PDB too strict (e.g., `minAvailable` equals total replicas), voluntary disruptions may be blocked entirely.

### References

- [Kubernetes Official Documentation: Pod Disruption Budgets](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/)
- [Best Practices for Pod Disruption Budgets](https://kubernetes.io/docs/tasks/run-application/configure-pdb/)
