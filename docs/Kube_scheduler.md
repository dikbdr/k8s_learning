# Kube-scheduler

## Kubernetes Scheduler

The Kubernetes scheduler is a control plane component responsible for assigning newly created pods to nodes in a cluster. It ensures that workloads are distributed efficiently across available resources.

**Example:**
When you create a deployment, the scheduler decides on which node each pod will run.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
    name: nginx-deployment
spec:
    replicas: 2
    selector:
        matchLabels:
            app: nginx
    template:
        metadata:
            labels:
                app: nginx
        spec:
            containers:
            - name: nginx
                image: nginx:latest
```

## How Kubernetes Scheduler Works

When a new pod is created, the scheduler evaluates all available nodes and selects the most suitable one based on resource requirements, constraints, and policies.

**Example:**
If a pod requests 2 CPUs and only one node has enough free CPU, the scheduler will choose that node.

## Working of a Kubernetes Scheduler

1. **Pod Queue:** Unschedulable pods are placed in a scheduling queue.
2. **Filtering:** The scheduler filters out nodes that do not meet the pod's requirements (e.g., insufficient resources, taints).
3. **Scoring:** Remaining nodes are scored based on factors like resource availability and affinity rules.
4. **Binding:** The scheduler assigns the pod to the highest-scoring node.

**Example:**
A pod with a node selector for `disktype: ssd` will only be scheduled on nodes with that label.

```yaml
spec:
    nodeSelector:
        disktype: ssd
```

## Running Multiple Schedulers

Kubernetes supports running multiple schedulers. You can specify a custom scheduler for a pod by setting the `schedulerName` field in the pod specification.

**Example:**
```yaml
spec:
    schedulerName: my-custom-scheduler
```
You can run your own scheduler alongside the default one for specialized scheduling needs.

## Node Selection in Kubernetes Scheduler

Node selection is based on several criteria:
- **Resource requests and limits:** Pods are scheduled to nodes with sufficient resources.
- **Node selectors and affinity/anti-affinity rules:** Control which nodes are eligible.
- **Taints and tolerations:** Prevent or allow pods to be scheduled on certain nodes.
- **Custom scheduling policies:** Implement custom logic for pod placement.

**Example: Using affinity:**
```yaml
spec:
    affinity:
        nodeAffinity:
            requiredDuringSchedulingIgnoredDuringExecution:
                nodeSelectorTerms:
                - matchExpressions:
                    - key: zone
                        operator: In
                        values:
                        - us-central1-a
```

These features allow fine-grained control over pod placement in your Kubernetes cluster.
