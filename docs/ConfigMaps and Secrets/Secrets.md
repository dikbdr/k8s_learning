# Kubernetes Secrets

## What is a Secret in Kubernetes?

A **Secret** in Kubernetes is an object used to store sensitive information, such as passwords, OAuth tokens, SSH keys, or any confidential data. Storing such information in a Secret is safer and more flexible than putting it directly in a Pod definition or a container image.

Secrets are encoded (not encrypted by default) and can be mounted as files or exposed as environment variables in Pods. Kubernetes also supports integrating with external secret management systems.

## Why Use Secrets?

- **Security**: Keeps sensitive data separate from application code.
- **Flexibility**: Easily update secrets without rebuilding images or redeploying applications.
- **Access Control**: Use RBAC to restrict who can access secrets.

## Creating a Secret

### 1. From Literal Values

```sh
kubectl create secret generic my-secret \
    --from-literal=username=admin \
    --from-literal=password=secret123
```

### 2. From a File

```sh
kubectl create secret generic my-tls-secret \
    --from-file=cert.crt \
    --from-file=key.key
```

### 3. From a YAML Manifest

```yaml
apiVersion: v1
kind: Secret
metadata:
    name: my-secret
type: Opaque
data:
    username: YWRtaW4=        # base64 encoded 'admin'
    password: c2VjcmV0MTIz    # base64 encoded 'secret123'
```
Apply with:
```sh
kubectl apply -f my-secret.yaml
```

## Using Secrets in Pods

### As Environment Variables

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: secret-env-pod
spec:
    containers:
    - name: mycontainer
        image: nginx
        env:
        - name: USERNAME
            valueFrom:
                secretKeyRef:
                    name: my-secret
                    key: username
```

### As Mounted Files

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: secret-volume-pod
spec:
    containers:
    - name: mycontainer
        image: nginx
        volumeMounts:
        - name: secret-volume
            mountPath: "/etc/secret"
            readOnly: true
    volumes:
    - name: secret-volume
        secret:
            secretName: my-secret
```

## Use Cases

- Storing database credentials
- Managing API keys or tokens
- TLS certificates for HTTPS
- SSH keys for Git operations

> **Note:** By default, Secrets are only base64-encoded, not encrypted. For stronger security, enable encryption at rest and restrict access via RBAC.
