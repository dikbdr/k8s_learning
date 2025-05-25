## Health Checks in Kubernetes

Kubernetes uses health checks to manage the lifecycle of pods and containers. There are two main types:

### 1. Readiness Probe

- **Purpose:** Determines if a container is ready to accept traffic.
- **Behavior:** If the readiness probe fails, the pod is removed from service endpoints, so it won’t receive requests.

**Example:**
```yaml
readinessProbe:
    httpGet:
        path: /healthz
        port: 8080
    initialDelaySeconds: 5
    periodSeconds: 10
```
This probe sends an HTTP GET request to `/healthz` on port `8080`. If the endpoint returns a success code, the pod is marked as ready.

### 2. Liveness Probe

- **Purpose:** Checks if a container is still running as expected.
- **Behavior:** If the liveness probe fails, Kubernetes restarts the container.

**Example:**
```yaml
livenessProbe:
    tcpSocket:
        port: 8080
    initialDelaySeconds: 15
    periodSeconds: 20
```
This probe tries to open a TCP connection on port `8080`. If it fails, Kubernetes restarts the container.

### Probe Types

- **HTTP GET:** Checks an HTTP endpoint.
- **TCP Socket:** Checks if a TCP port is open.
- **Exec:** Runs a command inside the container.

**Exec Example:**
```yaml
livenessProbe:
    exec:
        command:
        - cat
        - /tmp/healthy
    initialDelaySeconds: 5
    periodSeconds: 5
```

### Summary Table

| Probe Type      | Purpose                                 | Example Use Case            |
|-----------------|-----------------------------------------|-----------------------------|
| Readiness Probe | Pod is ready to serve traffic           | Wait for app to initialize  |
| Liveness Probe  | Pod is running as expected              | Detect deadlocks or crashes |

Properly configuring these probes ensures your applications are robust and self-healing in Kubernetes.