# Horizontal Pod Autoscaler (HPA) in Kubernetes

## What is HPA?

The **Horizontal Pod Autoscaler (HPA)** is a Kubernetes resource that automatically adjusts the number of pod replicas in a deployment, replica set, or stateful set based on observed metrics such as CPU utilization, memory usage, or custom metrics. This enables dynamic workload management, ensuring applications can scale up to handle increased demand and scale down to save resources during low demand.

## How HPA Works

HPA continuously monitors specified metrics and compares them to target values. If the observed value exceeds the target, HPA increases the number of pods; if it falls below, HPA decreases the number of pods. The controller checks metrics at regular intervals (default: every 15 seconds).

### Supported Metrics

- **Resource Metrics:** CPU and memory usage.
- **Custom Metrics:** Application-specific metrics (e.g., requests per second).
- **External Metrics:** Metrics from outside the cluster (e.g., queue length).

## Example 1: HPA Based on CPU Utilization

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
    name: cpu-hpa
spec:
    scaleTargetRef:
        apiVersion: apps/v1
        kind: Deployment
        name: my-app
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

**Explanation:**  
This HPA scales the `my-app` deployment between 2 and 10 replicas to maintain average CPU utilization at 50%.

## Example 2: HPA Based on Memory Usage

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
    name: memory-hpa
spec:
    scaleTargetRef:
        apiVersion: apps/v1
        kind: Deployment
        name: memory-app
    minReplicas: 1
    maxReplicas: 5
    metrics:
    - type: Resource
        resource:
            name: memory
            target:
                type: Utilization
                averageUtilization: 70
```

**Explanation:**  
This HPA manages the `memory-app` deployment, scaling pods to keep average memory usage at 70%.

## Example 3: HPA Using Custom Metrics

To use custom metrics, you need to configure the Kubernetes Metrics Server and possibly a custom metrics adapter.

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
    name: custom-metric-hpa
spec:
    scaleTargetRef:
        apiVersion: apps/v1
        kind: Deployment
        name: custom-app
    minReplicas: 1
    maxReplicas: 8
    metrics:
    - type: Pods
        pods:
            metric:
                name: http_requests_per_second
            target:
                type: AverageValue
                averageValue: "100"
```

**Explanation:**  
This HPA scales `custom-app` based on the average number of HTTP requests per second per pod.

## Best Practices

- Set reasonable `minReplicas` and `maxReplicas` to avoid resource exhaustion.
- Monitor application performance and tune HPA targets accordingly.
- Use custom metrics for more granular scaling based on business logic.

## Conclusion

HPA is a powerful tool for dynamic workload management in Kubernetes, enabling applications to efficiently handle varying loads with minimal manual intervention.