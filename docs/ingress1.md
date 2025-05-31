## Ingress Terminologies and Resources

### What is Ingress?
Ingress is a Kubernetes API object that manages external access to services within a cluster, typically HTTP/HTTPS traffic. It provides routing rules to control how requests reach your services.

### Key Terminologies

- **Ingress Controller**: A pod or deployment that implements the Ingress resource, watching for changes and configuring a load balancer or proxy (e.g., NGINX, Traefik).
- **Ingress Resource**: The YAML configuration that defines routing rules for external traffic.
- **Backend**: The Kubernetes Service that receives traffic routed by the Ingress.
- **Host**: The domain name (e.g., `example.com`) used to match incoming requests.
- **Path**: The URL path (e.g., `/app`) used to route requests to specific services.
- **TLS**: Configuration for securing Ingress traffic with SSL/TLS certificates.

### Ingress Resource Fields

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
    name: example-ingress
    annotations:
        # Optional: custom configuration for the ingress controller
spec:
    rules:
        - host: example.com
            http:
                paths:
                    - path: /app
                        pathType: Prefix
                        backend:
                            service:
                                name: app-service
                                port:
                                    number: 80
    tls:
        - hosts:
                - example.com
            secretName: example-tls
```

#### Field Explanations

- **apiVersion**: API version for the Ingress resource.
- **kind**: Resource type, always `Ingress`.
- **metadata**: Metadata about the Ingress (name, annotations, labels).
- **spec**: Specification of the desired behavior.
    - **rules**: List of routing rules.
        - **host**: Domain name to match.
        - **http**: HTTP-specific routing.
            - **paths**: List of path-based routing rules.
                - **path**: URL path to match.
                - **pathType**: How the path is matched (`Prefix`, `Exact`, or `ImplementationSpecific`).
                - **backend**: The service to route traffic to.
                    - **service.name**: Name of the Kubernetes Service.
                    - **service.port.number**: Service port to forward traffic.
    - **tls**: (Optional) TLS configuration.
        - **hosts**: Domains covered by the certificate.
        - **secretName**: Name of the Kubernetes Secret containing the TLS certificate.

### Summary Table

| Field                | Description                                      |
|----------------------|--------------------------------------------------|
| apiVersion           | API version of Ingress                           |
| kind                 | Always `Ingress`                                 |
| metadata             | Name, annotations, labels                        |
| spec.rules           | Routing rules for hosts and paths                |
| spec.rules.host      | Domain name to match                             |
| spec.rules.http      | HTTP routing configuration                       |
| spec.rules.http.paths| Path-based routing rules                         |
| spec.tls             | TLS/SSL configuration                            |
