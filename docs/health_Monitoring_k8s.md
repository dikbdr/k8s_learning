## Health Monitoring in Kubernetes

Health monitoring in Kubernetes (K8s) refers to the process of continuously checking the status and health of applications, pods, and cluster components to ensure they are running as expected. Kubernetes provides built-in mechanisms for health checks using **liveness** and **readiness probes**.

### Liveness Probe

A liveness probe checks if an application inside a pod is running. If the liveness probe fails, Kubernetes will restart the container.

**Example:**

```yaml
livenessProbe:
    httpGet:
        path: /healthz
        port: 8080
    initialDelaySeconds: 3
    periodSeconds: 10
```

### Readiness Probe

A readiness probe checks if an application is ready to accept traffic. If the readiness probe fails, the pod is removed from the service endpoints.

**Example:**

```yaml
readinessProbe:
    httpGet:
        path: /ready
        port: 8080
    initialDelaySeconds: 5
    periodSeconds: 10
```

### Benefits

- **Automatic Recovery:** Unhealthy containers are restarted automatically.
- **Traffic Management:** Only healthy pods receive traffic.
- **Improved Reliability:** Early detection of issues helps maintain application uptime.

Health monitoring is essential for building resilient and self-healing applications in Kubernetes.