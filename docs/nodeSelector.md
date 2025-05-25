# `nodeSelector` in Kubernetes

## Definition

`nodeSelector` is a simple way to constrain pods to run on particular nodes in a Kubernetes cluster. It is a field in a pod specification that matches the pod to nodes with specific labels.

## Explanation

When you use `nodeSelector`, Kubernetes schedules the pod only on nodes that have the specified label(s). This is useful for targeting workloads to nodes with certain hardware, geographic location, or other custom attributes.

## Example Usage

### 1. Label a Node

```sh
kubectl label nodes node-1 disktype=ssd
```

### 2. Pod Spec with `nodeSelector`

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: mypod
spec:
    nodeSelector:
        disktype: ssd
    containers:
    - name: mycontainer
        image: nginx
```

This pod will only be scheduled on nodes labeled with `disktype=ssd`.

### 3. Multiple Labels

```sh
kubectl label nodes node-2 region=us-west
```

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: region-pod
spec:
    nodeSelector:
        disktype: ssd
        region: us-west
    containers:
    - name: mycontainer
        image: nginx
```

This pod will only run on nodes with both `disktype=ssd` and `region=us-west`.

## Notes

- `nodeSelector` is simple but limited to exact matches.
- For more complex scheduling, use `nodeAffinity`.
