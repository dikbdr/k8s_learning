# Kubernetes ConfigMaps: Detailed Guide

## What is a ConfigMap?

A **ConfigMap** in Kubernetes is an API object used to store non-confidential configuration data in key-value pairs. ConfigMaps allow you to decouple configuration artifacts from image content, making applications more portable and easier to manage.

## Why Use ConfigMaps?

- **Separation of configuration and code:** Store configuration outside of container images.
- **Dynamic updates:** Change configuration without rebuilding images or redeploying pods.
- **Reusability:** Share configuration across multiple pods or deployments.

## Common Use Cases

- Storing application settings (e.g., URLs, feature flags)
- Managing environment variables
- Providing configuration files to applications

## Step-by-Step Example

### 1. Create a ConfigMap

You can create a ConfigMap from a file, literal values, or a directory.

**From literal values:**
```bash
kubectl create configmap app-config --from-literal=APP_MODE=production --from-literal=APP_DEBUG=false
```

**From a file:**
Suppose you have a file `config.properties`:
```
DB_HOST=localhost
DB_PORT=5432
```
Create ConfigMap:
```bash
kubectl create configmap db-config --from-file=config.properties
```

### 2. View ConfigMaps

```bash
kubectl get configmaps
kubectl describe configmap app-config
```

### 3. Use ConfigMap in a Pod

**As environment variables:**
```yaml
apiVersion: v1
kind: Pod
metadata:
    name: configmap-demo
spec:
    containers:
        - name: demo
            image: busybox
            command: ["sh", "-c", "env"]
            envFrom:
                - configMapRef:
                        name: app-config
```

**As configuration files:**
```yaml
apiVersion: v1
kind: Pod
metadata:
    name: configmap-volume-demo
spec:
    containers:
        - name: demo
            image: busybox
            command: ["cat", "/etc/config/config.properties"]
            volumeMounts:
                - name: config-volume
                    mountPath: /etc/config
    volumes:
        - name: config-volume
            configMap:
                name: db-config
```

### 4. Update a ConfigMap

Edit the ConfigMap:
```bash
kubectl edit configmap app-config
```
Or recreate it with new values.

**Note:** Pods do not automatically reload updated ConfigMaps unless restarted or configured with special mechanisms (e.g., projected volumes with `subPath`).

## Best Practices

- Do not store sensitive data in ConfigMaps (use Secrets for that).
- Use labels and annotations for organization.
- Version your ConfigMaps if needed for rollback.

## Summary

ConfigMaps are essential for managing configuration in Kubernetes, enabling flexibility and separation of concerns. They can be used as environment variables or mounted as files, and are easy to update and reuse.
