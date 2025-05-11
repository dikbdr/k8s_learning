# Features of Kubernetes

Kubernetes is a powerful open-source platform designed to automate the deployment, scaling, and operation of application containers. Below, we explore its key features in depth.

---

## 1. **Automated Scheduling**
Kubernetes efficiently schedules containers across a cluster to optimize resource utilization. It considers factors like CPU, memory, and affinity rules.

### Example: Pod Scheduling
```yaml
apiVersion: v1
kind: Pod
metadata:
    name: nginx-pod
spec:
    containers:
    - name: nginx
        image: nginx
    nodeSelector:
        disktype: ssd
```
This YAML file ensures the pod is scheduled on nodes with the label `disktype=ssd`.

---

## 2. **Self-Healing**
Kubernetes automatically restarts failed containers, replaces unresponsive nodes, and reschedules pods to maintain application health.

### Diagram: Self-Healing Workflow
![Self-Healing Workflow](https://kubernetes.io/images/docs/self-healing.png)

---

## 3. **Horizontal Scaling**
Kubernetes supports both manual and automatic scaling of applications based on resource usage or custom metrics.

### Example: Horizontal Pod Autoscaler
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
    maxReplicas: 10
    metrics:
    - type: Resource
        resource:
            name: cpu
            target:
                type: Utilization
                averageUtilization: 50
```
This configuration scales the deployment based on CPU utilization.

---

## 4. **Load Balancing and Service Discovery**
Kubernetes provides built-in service discovery and load balancing to route traffic to the appropriate pods.

### Diagram: Service Discovery
![Service Discovery](https://kubernetes.io/images/docs/service-discovery.png)

---

## 5. **Storage Orchestration**
Kubernetes allows you to automatically mount storage systems like AWS EBS, GCP Persistent Disks, or NFS.

### Example: Persistent Volume Claim
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
This claim requests 1Gi of storage for a pod.

---

## 6. **Configuration Management**
Kubernetes uses ConfigMaps and Secrets to manage application configuration without rebuilding container images.

### Example: ConfigMap
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
    name: app-config
data:
    APP_ENV: production
    APP_DEBUG: "false"
```

---

## 7. **Rolling Updates and Rollbacks**
Kubernetes ensures zero downtime during updates and allows you to roll back to previous versions if needed.

### Example: Rolling Update
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
    name: nginx-deployment
spec:
    replicas: 3
    strategy:
        type: RollingUpdate
        rollingUpdate:
            maxUnavailable: 1
            maxSurge: 1
    template:
        spec:
            containers:
            - name: nginx
                image: nginx:1.19
```

---

## 8. **Security and Compliance**
Kubernetes provides features like Role-Based Access Control (RBAC), network policies, and secrets management to secure your cluster.

### Example: RBAC Role
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
    namespace: default
    name: pod-reader
rules:
- apiGroups: [""]
    resources: ["pods"]
    verbs: ["get", "watch", "list"]
```

---

## 9. **Extensibility**
Kubernetes supports custom resources and controllers, enabling you to extend its functionality.

### Example: Custom Resource Definition (CRD)
```yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
    name: crontabs.stable.example.com
spec:
    group: stable.example.com
    names:
        kind: CronTab
        listKind: CronTabList
        plural: crontabs
        singular: crontab
    scope: Namespaced
    versions:
    - name: v1
        served: true
        storage: true
```

---

## Conclusion
Kubernetes is a versatile platform with features that simplify container orchestration. Its robust architecture ensures scalability, reliability, and security for modern applications.
