# Ingress in Kubernetes

**Ingress** is a Kubernetes API object that manages external access to services within a cluster, typically HTTP and HTTPS traffic. It provides routing rules to control how requests reach different services based on the request's host, path, or other properties.

## Use Cases

- **Expose multiple services under the same IP/domain**: Route traffic to different services based on URL paths or hostnames.
- **TLS/SSL termination**: Secure traffic with HTTPS at the Ingress level.
- **Load balancing**: Distribute incoming requests across multiple backend pods.
- **Centralized routing and authentication**: Apply authentication, rate limiting, or other policies at the Ingress.

## Example 1: Simple Path-based Routing

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
    name: example-ingress
spec:
    rules:
    - host: example.com
        http:
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

**Explanation:**  
Requests to `example.com/app1` go to `app1-service`, and requests to `example.com/app2` go to `app2-service`.

---

## Example 2: Host-based Routing

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
    name: host-ingress
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

**Explanation:**  
Traffic to `app1.example.com` goes to `app1-service`, and `app2.example.com` goes to `app2-service`.

---

## Example 3: TLS Termination

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

**Explanation:**  
Ingress uses the TLS certificate stored in `tls-secret` to terminate HTTPS for `secure.example.com`.

---

## Summary

Ingress simplifies exposing and managing access to Kubernetes services, supporting advanced routing, security, and scalability features.