# Creating Secrets in Kubernetes

Kubernetes Secrets are used to store and manage sensitive information, such as passwords, OAuth tokens, and ssh keys. Storing such information in a Secret is safer and more flexible than putting it verbatim in a Pod definition or in a container image.

## Types of Secrets

Kubernetes supports several types of Secrets:

- **Opaque**: Default type, used for arbitrary user-defined data.
- **kubernetes.io/dockerconfigjson**: Used for Docker registry credentials.
- **kubernetes.io/basic-auth**: Stores credentials for basic authentication.
- **kubernetes.io/ssh-auth**: Stores data for SSH authentication.
- **kubernetes.io/tls**: Stores a TLS certificate and its associated private key.

## Creating Secrets

### 1. From Literal Values

```sh
kubectl create secret generic my-secret \
    --from-literal=username=admin \
    --from-literal=password=secret123
```

### 2. From a File

```sh
kubectl create secret generic my-ssh-key \
    --from-file=ssh-privatekey=~/.ssh/id_rsa \
    --from-file=ssh-publickey=~/.ssh/id_rsa.pub
```

### 3. From a YAML Manifest

```yaml
apiVersion: v1
kind: Secret
metadata:
    name: my-secret
type: Opaque
data:
    username: YWRtaW4=    # base64 encoded 'admin'
    password: c2VjcmV0MTIz # base64 encoded 'secret123'
```
Apply with:
```sh
kubectl apply -f secret.yaml
```

## Using Secrets

### 1. As Environment Variables

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: secret-env-pod
spec:
    containers:
    - name: mycontainer
        image: busybox
        env:
        - name: USERNAME
            valueFrom:
                secretKeyRef:
                    name: my-secret
                    key: username
        - name: PASSWORD
            valueFrom:
                secretKeyRef:
                    name: my-secret
                    key: password
        command: ["sh", "-c", "echo $USERNAME $PASSWORD"]
```

### 2. As Files in a Volume

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: secret-volume-pod
spec:
    containers:
    - name: mycontainer
        image: busybox
        volumeMounts:
        - name: secret-volume
            mountPath: "/etc/secret"
            readOnly: true
        command: ["cat", "/etc/secret/username", "/etc/secret/password"]
    volumes:
    - name: secret-volume
        secret:
            secretName: my-secret
```

---

**Note:** Secrets are base64-encoded, not encrypted. For stronger security, use additional tools such as Kubernetes secrets encryption at rest and RBAC.