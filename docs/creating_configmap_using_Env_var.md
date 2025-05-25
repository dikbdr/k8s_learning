## Creating a ConfigMap from Environment Variables

You can create a ConfigMap directly from environment variables defined in your shell. This is useful when you want to quickly import existing environment settings into Kubernetes.

### Example: Creating a ConfigMap from Environment Variables

Suppose you have the following environment variables set in your shell:

```bash
export APP_MODE=production
export APP_DEBUG=false
```

You can create a ConfigMap using these variables with the following command:

```bash
kubectl create configmap my-env-config --from-env-file=<path-to-env-file>
```

#### Step-by-Step

1. **Create an environment file** (e.g., `app.env`):

    ```env
    APP_MODE=production
    APP_DEBUG=false
    ```

2. **Create the ConfigMap from the file:**

    ```bash
    kubectl create configmap my-env-config --from-env-file=app.env
    ```

This will create a ConfigMap named `my-env-config` with keys and values taken from the environment file.

### Using the ConfigMap in a Pod

You can reference the ConfigMap in your Pod definition to inject these values as environment variables:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: env-example-pod
spec:
  containers:
    - name: env-example-container
      image: nginx
      envFrom:
        - configMapRef:
            name: my-env-config
```

**Explanation:**
- The `envFrom` field imports all key-value pairs from the specified ConfigMap as environment variables in the container.
- This approach is convenient when you want to inject multiple configuration values at once.
