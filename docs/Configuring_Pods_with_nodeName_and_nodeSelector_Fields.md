# Configuring Pods with `nodeName` and `nodeSelector` Fields

## Problem Statement

You have been given a task to configure pods with `nodeName` and `nodeSelector` fields for efficient resource use, compliance, and specific application needs in a cluster.

---

## 1. Using `nodeName`

The `nodeName` field in a Pod spec assigns the Pod to a specific node by its name.

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: example-pod-nodename
spec:
    nodeName: worker-node-1
    containers:
        - name: nginx
            image: nginx:latest
```

**Note:** The scheduler is bypassed when `nodeName` is set.

---

## 2. Using `nodeSelector`

The `nodeSelector` field schedules Pods onto nodes with matching labels.

**Step 1:** Label your node:

```sh
kubectl label nodes worker-node-2 disktype=ssd
```

**Step 2:** Use `nodeSelector` in your Pod spec:

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: example-pod-nodeselector
spec:
    nodeSelector:
        disktype: ssd
    containers:
        - name: nginx
            image: nginx:latest
```

---

## 3. When to Use Each

- **`nodeName`:** For strict placement on a specific node (e.g., licensing, hardware).
- **`nodeSelector`:** For flexible placement based on node attributes (e.g., SSD, region).

---

## References

- [Kubernetes: Assigning Pods to Nodes](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/)