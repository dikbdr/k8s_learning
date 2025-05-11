# Features of Kubernetes

Kubernetes is a powerful container orchestration platform that provides several features to manage containerized applications effectively. Below are its key features, along with examples and diagrams:

## 1. **Automated Scheduling**
Kubernetes automatically schedules containers across nodes in a cluster based on resource requirements and constraints.

**Example:**
```yaml
apiVersion: v1
kind: Pod
metadata:
    name: nginx-pod
spec:
    containers:
    - name: nginx
        image: nginx
```
Kubernetes schedules the `nginx-pod` to an appropriate node.

**Diagram:**
![Automated Scheduling](https://kubernetes.io/images/docs/architecture/scheduler.svg)

---

## 2. **Self-Healing**
Kubernetes restarts failed containers, replaces and reschedules containers when nodes die, and kills containers that don’t respond to health checks.

**Example:**
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
                image: nginx
                livenessProbe:
                    httpGet:
                        path: /
                        port: 80
```
If a container fails the liveness probe, Kubernetes restarts it.

**Diagram:**
![Self-Healing](https://kubernetes.io/images/docs/architecture/self-healing.svg)

---

## 3. **Horizontal Scaling**
Kubernetes allows scaling applications up or down based on demand.

**Example:**
```bash
kubectl scale deployment nginx-deployment --replicas=5
```
This command scales the `nginx-deployment` to 5 replicas.

**Diagram:**
![Horizontal Scaling](https://kubernetes.io/images/docs/architecture/scaling.svg)

---

## 4. **Load Balancing and Service Discovery**
Kubernetes provides built-in service discovery and load balancing for distributing traffic across pods.

**Example:**
```yaml
apiVersion: v1
kind: Service
metadata:
    name: nginx-service
spec:
    selector:
        app: nginx
    ports:
    - protocol: TCP
        port: 80
        targetPort: 80
    type: LoadBalancer
```
This creates a load balancer for the `nginx` pods.

**Diagram:**
![Load Balancing](https://kubernetes.io/images/docs/architecture/load-balancing.svg)

---

## 5. **Storage Orchestration**
Kubernetes automatically mounts the storage system of your choice, such as local storage, cloud storage, or network storage.

**Example:**
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
    name: pvc-example
spec:
    accessModes:
    - ReadWriteOnce
    resources:
        requests:
            storage: 1Gi
```
This creates a persistent volume claim for storage.

**Diagram:**
![Storage Orchestration](https://kubernetes.io/images/docs/architecture/storage.svg)

---

## 6. **Configuration Management**
Kubernetes allows you to manage application configurations using ConfigMaps and Secrets.

**Example:**
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
    name: example-config
data:
    key: value
```
This ConfigMap stores configuration data for an application.

**Diagram:**
![Configuration Management](https://kubernetes.io/images/docs/architecture/configuration.svg)

---

## 7. **Monitoring and Logging**
Kubernetes integrates with monitoring and logging tools like Prometheus and Fluentd to provide insights into cluster performance.

**Example:**
- Use Prometheus to monitor metrics.
- Use Fluentd to aggregate logs.

**Diagram:**
![Monitoring and Logging](https://kubernetes.io/images/docs/architecture/monitoring.svg)

---

These features make Kubernetes a robust platform for managing containerized applications at scale.