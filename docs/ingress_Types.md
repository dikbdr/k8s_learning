# Types of Ingress

Kubernetes Ingress allows external access to services within a cluster. There are several types of Ingress configurations, each serving different use cases:

## 1. Single Service Ingress

Routes all traffic to a single backend service.

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
    name: single-service-ingress
spec:
    rules:
    - http:
            paths:
            - path: /
                pathType: Prefix
                backend:
                    service:
                        name: my-service
                        port:
                            number: 80
```

**Example:** All requests to the Ingress are forwarded to `my-service` on port 80.

---

## 2. Name-based (Host-based) Ingress

Routes traffic based on the requested host.

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
    name: host-based-ingress
spec:
    rules:
    - host: app1.example.com
        http:
            paths:
            - path: /
                pathType: Prefix
                backend:
                    service:
                        name: app1-service
                        port:
                            number: 80
    - host: app2.example.com
        http:
            paths:
            - path: /
                pathType: Prefix
                backend:
                    service:
                        name: app2-service
                        port:
                            number: 80
```

**Example:** Requests to `app1.example.com` go to `app1-service`, and requests to `app2.example.com` go to `app2-service`.

---

## 3. Path-based Ingress

Routes traffic based on the URL path.

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
    name: path-based-ingress
spec:
    rules:
    - http:
            paths:
            - path: /app1
                pathType: Prefix
                backend:
                    service:
                        name: app1-service
                        port:
                            number: 80
            - path: /app2
                pathType: Prefix
                backend:
                    service:
                        name: app2-service
                        port:
                            number: 80
```

**Example:** Requests to `/app1` go to `app1-service`, and `/app2` to `app2-service`.

---

## 4. TLS (HTTPS) Ingress

Secures Ingress traffic using TLS certificates.

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
    name: tls-ingress
spec:
    tls:
    - hosts:
        - secure.example.com
        secretName: tls-secret
    rules:
    - host: secure.example.com
        http:
            paths:
            - path: /
                pathType: Prefix
                backend:
                    service:
                        name: secure-service
                        port:
                            number: 443
```

**Example:** Requests to `https://secure.example.com` are routed securely to `secure-service`.

---

## 5. Default Backend Ingress

Handles requests that do not match any rule.

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
    name: default-backend-ingress
spec:
    defaultBackend:
        service:
            name: default-backend
            port:
                number: 80
```

**Example:** Any unmatched request is sent to `default-backend`.

---

Choose the Ingress type based on your application's routing and security requirements.