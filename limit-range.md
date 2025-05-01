## 🔹 Limit Range

### ❓🤔 **Question**: 
```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: cpu-resource-constraint
spec:
  limits:
    - type: Container
      default:
        cpu: "500m"
      defaultRequest:
        cpu: "500m"
      max:
        cpu: "1"
      min:
        cpu: "100m"
```

🧩 How can I apply this LimitRange configuration to a pod in Kubernetes?

### ✅ **Answer**: 
To integrate a LimitRange into a pod, you don't directly attach it to the pod YAML. 
Instead, the LimitRange defines default resource requests and limits for containers within a namespace, and it automatically applies to any pod created in that namespace.
Here's how you can integrate it:
1. **Create the** `LimitRange` in the same namespace where your pod is deployed. You've already created the `LimitRange` YAML configuration, which will apply limits for the CPU resources in the specified namespace.
2. **Pod Definition**: When you create a pod within the same namespace as the `LimitRange`, Kubernetes will automatically apply the `LimitRange` to your pod if you don’t explicitly set requests and limits for resources (like CPU). 
If you specify resource requests and limits in your pod YAML, Kubernetes will enforce them along with the `LimitRange` defaults.
3. **Steps**:  
   **3.1. Apply the `LimitRange` resource to the namespace**
   ```bash
   kubectl apply -f cpu-resource-constraint.yaml
   ```
   Where cpu-resource-constraint.yaml is the `LimitRange` configuration you provided.  
   **3.2. Pod definition**
   You don’t need to reference the `LimitRange` in your pod explicitly. However, you should define resource requests and limits for the pod containers. If you don’t set them, the `LimitRange` will automatically apply its default values to the pod.
   Here's an example pod definition where Kubernetes will apply the `LimitRange` defaults:
   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
      name: simple-webapp-color
      labels:
        name: simple-webapp-color
   spec:
      containers:
        - name: simple-webapp-color
          image: simple-webapp-color
          ports:
            - containerPort: 8080
   ```
   **3.3. How `LimitRange` affects the pod**
   In the above example, since you haven't set the CPU request and limit explicitly, Kubernetes will apply the default request of 500m and the default limit of 500m for the container in the pod.
   If you explicitly define requests and limits in the pod definition, Kubernetes will use those values, and the `LimitRange` will enforce them if they are within the defined range (i.e., between 100m and 1 CPU in your `LimitRange`).
   For example:
   ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: simple-webapp-color
      labels:
        name: simple-webapp-color
    spec:
      containers:
      - name: simple-webapp-color
        image: simple-webapp-color
        ports:
          - containerPort: 8080
        resources:
          requests:
            cpu: "200m"
          limits:
            cpu: "500m"
   ```

In this case:  
* The pod will request 200m CPU.
* The pod will have a limit of 500m CPU.
* These values are within the range set by your `LimitRange`, so the pod will be allowed to create without any issues.

### 📌 Summary

* The `LimitRange` applies default resource requests and limits to all pods within the same namespace.
* You **do not** need to reference the `LimitRange` in the pod definition.
* If resource requests and limits are **not specified** in the pod, Kubernetes will apply the `LimitRange` defaults.