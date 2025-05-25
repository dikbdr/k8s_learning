# Taints and Tolerations in Kubernetes

Taints and tolerations are mechanisms in Kubernetes that allow you to control which pods can be scheduled on specific nodes. Taints are applied to nodes, and tolerations are applied to pods. If a node is tainted, only pods with matching tolerations can be scheduled on it.

## How Taints Work

A taint on a node has three parts:
- **Key**
- **Value**
- **Effect** (`NoSchedule`, `PreferNoSchedule`, or `NoExecute`)

Example:
```shell
kubectl taint nodes node1 key=value:NoSchedule
```
This prevents pods without a matching toleration from being scheduled on `node1`.

## How Tolerations Work

A toleration on a pod allows it to be scheduled on nodes with matching taints.

Example:
```yaml
tolerations:
- key: "key"
    operator: "Equal"
    value: "value"
    effect: "NoSchedule"
```

## Use Case Examples

### 1. Dedicated Nodes for Critical Workloads

**Scenario:** You want only critical workloads to run on certain nodes.

- **Taint the node:**
    ```shell
    kubectl taint nodes node1 dedicated=critical:NoSchedule
    ```
- **Add toleration to critical pods:**
    ```yaml
    tolerations:
    - key: "dedicated"
        operator: "Equal"
        value: "critical"
        effect: "NoSchedule"
    ```

### 2. Prevent Scheduling During Maintenance

**Scenario:** Temporarily prevent new pods from being scheduled during node maintenance.

- **Taint the node:**
    ```shell
    kubectl taint nodes node2 maintenance=true:NoSchedule
    ```
- **Pods without a matching toleration won't be scheduled.**

### 3. Evict Pods from a Node

**Scenario:** Remove all pods from a node for decommissioning.

- **Taint the node:**
    ```shell
    kubectl taint nodes node3 decommission=true:NoExecute
    ```
- **Pods without a matching toleration will be evicted immediately.**

### 4. Allow Specific Pods on GPU Nodes

**Scenario:** Only GPU workloads should run on GPU-enabled nodes.

- **Taint the node:**
    ```shell
    kubectl taint nodes gpu-node gpu=true:NoSchedule
    ```
- **Add toleration to GPU workload pods:**
    ```yaml
    tolerations:
    - key: "gpu"
        operator: "Equal"
        value: "true"
        effect: "NoSchedule"
    ```

## Summary

- **Taints** repel pods from nodes.
- **Tolerations** allow pods to tolerate (and thus be scheduled on) tainted nodes.
- Use them together to control pod placement and node usage.
