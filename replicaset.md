#### Replica Set

A ReplicaSet in Kubernetes is a controller that ensures a specified number of identical 
Pod replicas are running at any given time.

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-replicaset
  labels:
    app: myapp
    type: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      type: frontend
  template:
    # here comes the pod-definition.yml
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: frontend
    spec:
      containers:
        - name: nginx-container
          image: nginx
```

Now, this ReplicaSet helps to produce the same 3 pods of the same kind.
