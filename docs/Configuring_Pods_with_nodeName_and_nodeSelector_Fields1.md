# Kubernetes Task 1

## Steps to be followed

### 1. Create pods with the fields `nodeName` and `nodeSelector`

```yaml
# Pod with nodeName
apiVersion: v1
kind: Pod
metadata:
    name: pod-with-nodename
spec:
    containers:
    - name: nginx
        image: nginx
    nodeName: <node-name>
---
# Pod with nodeSelector
apiVersion: v1
kind: Pod
metadata:
    name: pod-with-nodeselector
spec:
    containers:
    - name: nginx
        image: nginx
    nodeSelector:
        disktype: ssd
```

### 2. Assign label to the nodes

```sh
kubectl label nodes <node-name> disktype=ssd
```

### 3. Create a pod with the `NotIn` operator

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: pod-with-notin
spec:
    containers:
    - name: nginx
        image: nginx
    affinity:
        nodeAffinity:
            requiredDuringSchedulingIgnoredDuringExecution:
                nodeSelectorTerms:
                - matchExpressions:
                    - key: disktype
                        operator: NotIn
                        values:
                        - hdd
```