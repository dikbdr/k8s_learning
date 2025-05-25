# Liveness Probe Example in Kubernetes

## Problem Statement

You have been assigned a task to create and configure a pod using liveness probes to ensure the stability and reliability of the applications running inside the pod.

## Solution

Below is an example of a Kubernetes Pod manifest with a liveness probe configured:

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: liveness-probe-example
spec:
    containers:
    - name: my-app
        image: nginx:latest
        ports:
        - containerPort: 80
        livenessProbe:
            httpGet:
                path: /
                port: 80
            initialDelaySeconds: 10
            periodSeconds: 5
            failureThreshold: 3
```

### Explanation

- **livenessProbe**: Checks if the application is running. If the probe fails, Kubernetes restarts the container.
- **httpGet**: Performs an HTTP GET request on the root path `/` at port 80.
- **initialDelaySeconds**: Waits 10 seconds before performing the first probe.
- **periodSeconds**: Performs the probe every 5 seconds.
- **failureThreshold**: After 3 consecutive failures, the container is restarted.

## How to Apply

1. Save the manifest as `liveness-probe-example.yaml`.
2. Apply it using:
     ```sh
     kubectl apply -f liveness-probe-example.yaml
     ```

## References

- [Kubernetes Official Documentation: Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)