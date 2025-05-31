## Ingress Path Types

Kubernetes Ingress supports different path types to define how incoming URLs are matched against the rules in an Ingress resource. The three main path types are:

### 1. `Prefix`

- Matches if the request path starts with the specified path.
- Example:
    ```yaml
    - path: /foo
        pathType: Prefix
        backend:
            service:
                name: foo-service
                port:
                    number: 80
    ```
    - Matches `/foo`, `/foo/`, `/foo/bar`, etc.

### 2. `Exact`

- Matches only if the request path exactly matches the specified path.
- Example:
    ```yaml
    - path: /foo
        pathType: Exact
        backend:
            service:
                name: foo-service
                port:
                    number: 80
    ```
    - Matches only `/foo`, **not** `/foo/` or `/foo/bar`.

### 3. `ImplementationSpecific`

- Matching behavior depends on the Ingress controller implementation.
- Example:
    ```yaml
    - path: /foo
        pathType: ImplementationSpecific
        backend:
            service:
                name: foo-service
                port:
                    number: 80
    ```
    - Use with caution, as behavior may vary.

**Note:** Prefer `Prefix` or `Exact` for predictable behavior across different Ingress controllers.