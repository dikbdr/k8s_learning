# Self-Healing Pods in Kubernetes

Kubernetes is designed with self-healing capabilities to ensure high availability and reliability of applications. Self-healing means that Kubernetes can automatically detect and recover from failures in pods, nodes, or containers without manual intervention.

## What is Self-Healing?

Self-healing in Kubernetes refers to the system's ability to automatically replace or restart failed pods or containers to maintain the desired state defined by the user. This is achieved using controllers like Deployments, ReplicaSets, and StatefulSets.

## How Does Self-Healing Work?

Kubernetes continuously monitors the health of pods and nodes. If a pod crashes or becomes unresponsive, Kubernetes will:

- **Restart the pod** if it fails (using the pod's `restartPolicy`).
- **Reschedule the pod** on another node if the current node fails.
- **Replace the pod** if it is deleted or evicted.

This ensures that the desired number of pod replicas are always running.

## Example: Self-Healing with Deployments

Below is a simple example of a Deployment manifest:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
    name: nginx-deployment
spec:
    replicas: 3
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

If one of the `nginx` pods crashes or is deleted, Kubernetes will automatically create a new pod to maintain three running replicas.

## Liveness and Readiness Probes

Kubernetes uses **liveness** and **readiness** probes to check the health of containers:

- **Liveness Probe:** Checks if the container is running. If it fails, Kubernetes restarts the container.
- **Readiness Probe:** Checks if the container is ready to serve traffic. If it fails, the pod is removed from service endpoints.

Example liveness probe:

```yaml
livenessProbe:
    httpGet:
        path: /
        port: 80
    initialDelaySeconds: 15
    periodSeconds: 20
```

## Summary

Self-healing is a core feature of Kubernetes that helps maintain application availability by automatically detecting and recovering from failures. By defining the desired state and using probes, Kubernetes ensures your applications are resilient and reliable.