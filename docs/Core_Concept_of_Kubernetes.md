# Core Concepts of Kubernetes

Kubernetes is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications. Below are the core concepts of Kubernetes:

---

## 1. **Cluster**
A Kubernetes cluster consists of a set of worker machines, called nodes, that run containerized applications. Every cluster has at least one master node and multiple worker nodes.

![Kubernetes Cluster Architecture](https://kubernetes.io/images/docs/components-of-kubernetes.svg)

---

## 2. **Node**
A node is a physical or virtual machine that runs containerized applications. Each node contains:
- **Kubelet**: Ensures containers are running in a Pod.
- **Kube-proxy**: Handles networking for the Pods.
- **Container Runtime**: Runs the containers (e.g., Docker, containerd).

---

## 3. **Pod**
A Pod is the smallest deployable unit in Kubernetes. It represents a single instance of a running process in your cluster and can contain one or more containers.

![Kubernetes Pod](https://kubernetes.io/images/docs/pod.svg)

---

## 4. **Deployment**
A Deployment ensures that a specified number of Pod replicas are running at any given time. It provides declarative updates for Pods and ReplicaSets.

---

## 5. **Service**
A Service is an abstraction that defines a logical set of Pods and a policy to access them. It provides stable networking and load balancing.

---

## 6. **ConfigMap and Secret**
- **ConfigMap**: Stores configuration data in key-value pairs.
- **Secret**: Stores sensitive information like passwords and API keys.

---

## 7. **Namespace**
Namespaces are used to divide cluster resources between multiple users or teams. They provide a mechanism for isolating resources.

---

## 8. **Ingress**
Ingress manages external access to services in a cluster, typically HTTP and HTTPS traffic.

---

## 9. **Volumes**
Volumes provide persistent storage for Pods. They allow data to persist even if the Pod is deleted or restarted.

---

## 10. **Kube-API Server**
The API server is the front-end for the Kubernetes control plane. It handles RESTful requests and updates the cluster state.

---

For more details, visit the [Kubernetes Documentation](https://kubernetes.io/docs/).
