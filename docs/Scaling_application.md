Kubernetes provides robust scalability features by automatically adjusting workloads based on resource usage. Autoscaling in Kubernetes operates across three main dimensions:

## 1. Horizontal Pod Autoscaler (HPA)

**HPA** automatically adjusts the number of pod replicas in a deployment or replica set based on observed CPU utilization or other select metrics.

**Example:**
Suppose you have a deployment running a web application. You can configure HPA to maintain an average CPU utilization of 50% across all pods:

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
    name: webapp-hpa
spec:
    scaleTargetRef:
        apiVersion: apps/v1
        kind: Deployment
        name: webapp
    minReplicas: 2
    maxReplicas: 10
    metrics:
    - type: Resource
        resource:
            name: cpu
            target:
                type: Utilization
                averageUtilization: 50
```

If CPU usage increases, HPA will create more pods to handle the load; if usage drops, it will reduce the number of pods.

---

## 2. Cluster Autoscaler

**Cluster Autoscaler** automatically adjusts the number of nodes in your cluster. If pods cannot be scheduled due to insufficient resources, it adds nodes; if nodes are underutilized, it removes them.

**Example:**
In a cloud environment (like GKE, EKS, or AKS), you can enable cluster autoscaler for your node pool. If your workloads require more resources than available, the autoscaler will provision new nodes. When demand decreases, it scales down unused nodes to save costs.

---

## 3. Vertical Pod Autoscaler (VPA)

**VPA** automatically adjusts the CPU and memory resource requests and limits for containers in your pods. This ensures each pod has the right amount of resources based on historical usage.

**Example:**
You can deploy a VPA resource to recommend and update resource requests for a deployment:

```yaml
apiVersion: autoscaling.k8s.io/v1
kind: VerticalPodAutoscaler
metadata:
    name: webapp-vpa
spec:
    targetRef:
        apiVersion: "apps/v1"
        kind:       Deployment
        name:       webapp
    updatePolicy:
        updateMode: "Auto"
```

VPA will monitor the resource usage and automatically update the pod specifications to optimize resource allocation.

---

**Summary:**  
Kubernetes autoscaling—through HPA, Cluster Autoscaler, and VPA—enables applications to efficiently handle varying workloads by dynamically adjusting pods, nodes, and resource allocations.