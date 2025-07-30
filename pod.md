### POD

The smallest unit in Kubernetes. A Pod represents one or more tightly coupled containers that share:

1. The same network namespace (IP, ports)

2. The same storage volumes (if defined)

3. The same lifecycle

Here is an example definition of a Pod in Kubernetes:

```pod-definition.yaml
apiVersion: v1
kind: Pod
metadata: 
  name: myapp-pod
  label: 
    app: myapp
    type: front-end
spec:
  containers:
    - name: nginx-container
      image: nginx
```

Note: 
1. You can only define name and label under the metadata section.
2. You can see the kind and version definitions in the following table:

| Kind        | API Version |
|-------------|-------------|
| Pod         | v1          |
| Service     | v1          |
| ReplicaSet  | apps/v1     |
| Deployment  | apps/v1     |


You can deploy the pod with the following command. It creates a new resource if it does not exist or updates the existing resource if it already exists:

```code
kubectl apply -f <pod_name>
```

Here are another useful commands:

Create a pod
```code
kubectl create -f <file_name> 
```

Describe a pod
```code
kubectl describe pod <pod_name> 
```

Get all the pods with their status
```code
kubectl get pods 
```
You can use "-o wide" parameter for more detailed output.