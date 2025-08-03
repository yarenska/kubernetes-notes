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

Answers:  
The first question's answer is: "selector"
- Pods are identified via selectors.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: microservice-one-service
spec:
  selector:
    app: microservice-one
    .
    .
    .
```

- We specify the selector as key value pairs that identifies the pods to be matched. As you can see above,
we have defined the pod with the selector having the label "microservice-one". In that case, service will see the pod 
and it will forward to request to.  


The second question is if a Pod has multiple ports open, how does Service know which port to forward the request to?
Answer: This is defined target attribute.  

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

First, the service will find the pod that matches via selector given. 
Since the service is a type of load balancer, it will randomly pick a replica of a pod to send the request to.
And it will see the targetPort defined in yaml file.


#### 2. Headless Service

**Properties:**
- Client wants to communicate with 1 specific Pod directly
- Pods want talk directly with specific Pod
- So, not randomly selected
- Use Case: Stateful applications, like databases
- Pod replicas are not identical
- Only master is allowed to write to DB

**Question 1:** How can clients know Pod's IP addresses?  
One option could be API call to Kubernetes API server. But it makes the API too tied to Kubernetes API
It is inefficient.
2nd option is Kubernetes allows clients to discover Pods IP adresses through DNS lookups. Usually, 
when a client performs DNS lookup for a service, the DNS server returns single IP address which belongs to the service.
And this will be the service's ClusterIP adress (internal). If you set the ClusterIP "None", it returns the Pod IP address
instead.


#### 3 Service Type Attributes
1. Cluster IP

```yaml
apiVersion: v1
kind: Service
metadata:
    name: my-service
spec:
    type: ClusterIP
```

* Default, type is not needed
* Internal Service

2. NodePort

```yaml
apiVersion: v1
kind: Service
metadata:
    name: my-service
spec: 
    type: NodePort
```

NodePort basically creates a Service, that is accessible on a static port on each worker node in the cluster.
To compare it with the ClusterIP, ClusterIP is only accessible within the cluster. No external traffic can 
directly access to the Service.  
NodePort service however, makes the external traffic accessible on static, fixed port on each worker node.
So in this case, instead of Ingress, the browser request will send the request directly to the worker node, 
at the port that the Service definition defines as follows:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: ms-service-nodeport
spec:
  type: NodePort
  selector:
    app: microservice-one
  ports:
    - protocol: TCP
      port: 3200
      targetPort: 3000
      nodePort: 30008
```

Here you can see 3 types of port:
1. "port": It is the Service's (ClusterIP) port inside the worker Node.
2. "targetPort": It is the port of the Pod that will be communicated with.
3. "nodePort": It is the port of the Pod that the requests of the external requests will be redirected to.

ClusterIP service is automatically created.
NodePort range must be within 30000-32767.
