## Using ConfigMaps with Mounted Volumes in Kubernetes

ConfigMaps in Kubernetes allow you to decouple configuration artifacts from image content, making your applications more portable and easier to manage. One common way to use ConfigMaps is by mounting them as volumes into your pods, so your applications can consume configuration files directly from the filesystem.

### Real-World Use Case

**Scenario:**  
Suppose you have a web application (e.g., Nginx or Apache) that requires a custom configuration file (`nginx.conf`). Instead of baking this file into your container image, you can store it in a ConfigMap and mount it into the pod at runtime.

### Step-by-Step Example

#### 1. Create a ConfigMap from a Configuration File

```bash
kubectl create configmap nginx-config --from-file=nginx.conf
```

#### 2. Define a Pod Spec that Mounts the ConfigMap

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: nginx-pod
spec:
    containers:
        - name: nginx
            image: nginx:latest
            volumeMounts:
                - name: config-volume
                    mountPath: /etc/nginx/nginx.conf
                    subPath: nginx.conf
    volumes:
        - name: config-volume
            configMap:
                name: nginx-config
```

**Explanation:**
- The `nginx.conf` file from the ConfigMap is mounted directly to `/etc/nginx/nginx.conf` inside the container.
- Any updates to the ConfigMap can be reflected in the pod (with some caveats regarding live reload).

### Other Real-World Examples

- **Application Properties:** Mounting `application.properties` for Java Spring Boot apps.
- **Environment Files:** Providing `.env` files for Node.js or Python applications.
- **Custom Scripts:** Supplying initialization or maintenance scripts to containers.

### Benefits

- **Separation of Concerns:** Keeps configuration separate from application code.
- **Easy Updates:** Update configuration without rebuilding images.
- **Reusability:** Share the same configuration across multiple pods or deployments.
