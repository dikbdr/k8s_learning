## Load Balancing with Ingress in Kubernetes

Kubernetes Ingress is an API object that manages external access to services in a cluster, typically HTTP. One of its key features is load balancing, which distributes incoming traffic across multiple backend pods to ensure reliability and scalability.

### How Ingress Load Balancing Works

When a request reaches the Ingress controller, it uses rules defined in the Ingress resource to route the request to the appropriate backend service. The controller then load balances the traffic among the pods behind that service.

### Example: Simple Ingress Load Balancing

Suppose you have a deployment with two pods running a web application, exposed via a Kubernetes Service. An Ingress resource is used to route external traffic to this service.

**1. Deployment and Service**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
    name: web-app
spec:
    replicas: 2
    selector:
        matchLabels:
            app: web-app
    template:
        metadata:
            labels:
                app: web-app
        spec:
            containers:
            - name: web-app
                image: nginx
                ports:
                - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
    name: web-app-service
spec:
    selector:
        app: web-app
    ports:
        - protocol: TCP
            port: 80
            targetPort: 80
```

**2. Ingress Resource**

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
    name: web-app-ingress
spec:
    rules:
    - host: example.com
        http:
            paths:
            - path: /
                pathType: Prefix
                backend:
                    service:
                        name: web-app-service
                        port:
                            number: 80
```

### What Happens

- The Ingress controller receives HTTP requests for `example.com`.
- It forwards the requests to the `web-app-service`.
- The service load balances the requests across the two pods.

This setup ensures that traffic is distributed evenly, improving availability and performance.
