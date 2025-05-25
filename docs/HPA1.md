# Configuring Horizontal Pod Autoscaling (HPA)

## Problem Statement

You have been assigned a task to create and configure horizontal pod autoscaling to optimize performance and implement efficient resource utilization.

---

## Steps to Configure HPA

### 1. Prerequisites

- Kubernetes cluster running (v1.6+)
- Metrics Server installed and running
- A deployment to autoscale (e.g., `nginx-deployment`)

### 2. Create a Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
    name: nginx-deployment
spec:
    replicas: 1
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
                image: nginx
                resources:
                    requests:
                        cpu: 100m
                    limits:
                        cpu: 200m
```

Apply the deployment:

```sh
kubectl apply -f nginx-deployment.yaml
```

### 3. Create the HPA Resource

```sh
kubectl autoscale deployment nginx-deployment --cpu-percent=50 --min=1 --max=5
```

Or use YAML:

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
    name: nginx-hpa
spec:
    scaleTargetRef:
        apiVersion: apps/v1
        kind: Deployment
        name: nginx-deployment
    minReplicas: 1
    maxReplicas: 5
    metrics:
    - type: Resource
        resource:
            name: cpu
            target:
                type: Utilization
                averageUtilization: 50
```

Apply the HPA:

```sh
kubectl apply -f nginx-hpa.yaml
```

### 4. Verify HPA

```sh
kubectl get hpa
```

### 5. Test Autoscaling

- Generate load to see scaling in action.
- Monitor pods and HPA status.

---

## References

- [Kubernetes HPA Documentation](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/)
- [Metrics Server](https://github.com/kubernetes-sigs/metrics-server)
