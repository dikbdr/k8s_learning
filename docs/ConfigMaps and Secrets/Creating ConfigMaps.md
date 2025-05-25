## Creating ConfigMaps in Kubernetes

A ConfigMap is an API object used to store non-confidential data in key-value pairs. ConfigMaps allow you to decouple configuration artifacts from image content to keep containerized applications portable.

### Creating a ConfigMap from Literal Values

You can create a ConfigMap directly from the command line using the `kubectl create configmap` command:

```sh
kubectl create configmap my-config --from-literal=key1=value1 --from-literal=key2=value2
```

### Creating a ConfigMap from a File

You can also create a ConfigMap from a file:

```sh
kubectl create configmap my-config --from-file=path/to/config-file.properties
```

### Creating a ConfigMap from a YAML Manifest

Here is an example YAML manifest for a ConfigMap:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
    name: example-config
data:
    APP_ENV: "production"
    LOG_LEVEL: "info"
```

Apply it with:

```sh
kubectl apply -f configmap.yaml
```

### Using a ConfigMap in a Pod

You can reference ConfigMaps in your Pod definitions:

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: configmap-demo
spec:
    containers:
        - name: demo
            image: busybox
            env:
                - name: APP_ENV
                    valueFrom:
                        configMapKeyRef:
                            name: example-config
                            key: APP_ENV
```

This injects the `APP_ENV` value from the ConfigMap into the container's environment.
