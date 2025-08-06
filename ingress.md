#### Ingress

Ingress is an API object in Kubernetes that manages external access to
services within a cluster, typically over HTTP or HTTPS.

Here is the simple definition of an Ingress:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
spec:
  rules:
    - host: example.com
      http:
        paths:
          - path: /app
            pathType: Prefix
            backend:
              service:
                name: my-service
                port:
                  number: 80
```