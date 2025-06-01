# Storage in Kubernetes

Kubernetes provides several mechanisms to manage storage for containers, enabling both ephemeral and persistent data storage.

## Persistent Storage and Its Capabilities

**Persistent Storage** in Kubernetes allows data to persist beyond the lifecycle of individual pods. This is crucial for stateful applications like databases.

### Capabilities:
- **Data durability:** Data survives pod restarts, rescheduling, and node failures.
- **Dynamic provisioning:** Storage can be provisioned on-demand.
- **Access modes:** Supports ReadWriteOnce, ReadOnlyMany, and ReadWriteMany.
- **Storage classes:** Define different types of storage (e.g., SSD, HDD, network-attached).

## Types of Persistent Storage

1. **Block Storage:** Raw block devices (e.g., AWS EBS, GCE Persistent Disk).
2. **File Storage:** Shared file systems (e.g., NFS, Azure Files).
3. **Object Storage:** Not natively supported as a volume, but accessible via APIs (e.g., S3, GCS).

## Requirements of Persistent Storage

- **Reliability:** Must ensure data is not lost.
- **Performance:** Meets application I/O needs.
- **Scalability:** Can grow with application demand.
- **Accessibility:** Available to pods as needed.
- **Security:** Supports encryption and access controls.

## Volumes in Kubernetes

A **Volume** is a directory accessible to containers in a pod. Volumes solve the problem of ephemeral container storage.

### How Volumes Work

- Defined in the pod spec.
- Mounted into one or more containers.
- Lifecycle tied to the pod (except for persistent volumes).

## Types of Volumes

### 1. `emptyDir`
- Created when a pod is assigned to a node.
- Data is deleted when the pod is removed.
- **Example:**
    ```yaml
    volumes:
        - name: cache-volume
            emptyDir: {}
    ```

### 2. `hostPath`
- Mounts a file or directory from the host node.
- Useful for single-node testing.
- **Example:**
    ```yaml
    volumes:
        - name: host-volume
            hostPath:
                path: /data
    ```

### 3. `persistentVolumeClaim` (PVC)
- Connects a pod to a PersistentVolume (PV).
- Supports dynamic provisioning.
- **Example:**
    ```yaml
    volumes:
        - name: data-volume
            persistentVolumeClaim:
                claimName: my-pvc
    ```

### 4. `configMap` and `secret`
- Injects configuration data or secrets into pods.
- **Example:**
    ```yaml
    volumes:
        - name: config-volume
            configMap:
                name: app-config
    ```

### 5. `nfs`
- Mounts an NFS share.
- **Example:**
    ```yaml
    volumes:
        - name: nfs-volume
            nfs:
                server: nfs.example.com
                path: /exported/path
    ```

### 6. Cloud Provider Volumes
- AWS EBS, GCE Persistent Disk, Azure Disk, etc.
- **Example (AWS EBS):**
    ```yaml
    volumes:
        - name: ebs-volume
            awsElasticBlockStore:
                volumeID: vol-0abcd1234
                fsType: ext4
    ```

## Example: Using a Persistent Volume Claim

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
    name: my-pvc
spec:
    accessModes:
        - ReadWriteOnce
    resources:
        requests:
            storage: 5Gi
    storageClassName: standard
---
apiVersion: v1
kind: Pod
metadata:
    name: app-pod
spec:
    containers:
        - name: app
            image: nginx
            volumeMounts:
                - mountPath: "/usr/share/nginx/html"
                    name: data-volume
    volumes:
        - name: data-volume
            persistentVolumeClaim:
                claimName: my-pvc
```

---

**Summary:**  
Kubernetes supports various storage types and volume mechanisms to meet different application needs, from ephemeral to persistent, and from local to cloud-backed storage.