### Services 

#### 1. Cluster IP
* It is the default type of services. If you didnt define anything, the service get automaticaaly this type.

Assuming we have a Deployment having the definition of type:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: microservice-one
spec:
  replicas: 2
  template:
    metadata:
      labels:
        app: microservice-one
    spec:
      containers:
        - name: ms-one
          image: my-private-repo/ms-one:1.0
          ports:
            - containerPort: 3000
        - name: log-collector
          image: my-private-repo/log-collector:1.0
          ports:
            - containerPort: 9000
            . 
            .
            .
```

Here in the spec part, we have the Pod definition having 2 containers (1 being the side container).

And assuming that we have a Service definition:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: microservice-one-service
spec:
  selector:
    app: microservice-one
  ports:
    - protocol: TCP
      port: 3200
      targetPort: 3000

```
And we have the Ingress definition:

```yaml
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: ms-one-ingress
  annotations:
    kubernetes.io/ingress.class: "nginx"
spec:
  rules:
    - host: microservice-one.com
      http:
        paths:
          - path: /
            backend:
              serviceName: microservice-one-service
              servicePort: 3200
```

Now question may arise:

1. How does Service know which Pod to forward to requests to?
2. In the pod, where to forward to requests to?

