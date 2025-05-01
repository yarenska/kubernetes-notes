## 🔹 kubectl run vs kubectl apply -f

Both `kubectl run` and `kubectl apply -f` are used to create Kubernetes resources, but they have different use cases and capabilities.

### 1️⃣ kubectl run

🚀 Creates a single Pod quickly, without using a YAML file.

#### ✅ When to Use kubectl run
* When you need to quickly create a pod for testing.
* When you want to override commands at runtime.
* When you don’t need a full YAML manifest.

#### 📌 Example
```sh
  kubectl run mypod --image=nginx --port=80
 ```

This command:
* Creates a Pod named mypod.
* Uses the nginx image.
* Exposes port 80 inside the container.


### Overriding Command & Arguments

####
```sh
  kubectl run ubuntu-sleeper --image=ubuntu -- sleep 500
 ```

This command:
* Runs the ubuntu container.
* Executes sleep 500 instead of the default command.


### ⚠️ Limitations of kubectl run

* Cannot create Deployments, Services, ConfigMaps, etc. (Only Pods). ❌
* Cannot fully define complex configurations (like volumes, labels, etc.). ❌
* Does not store configuration for easy reusability like YAML files. ❌


### 2️⃣ kubectl run

📄 Applies a YAML file to create, update, or delete Kubernetes resources.

#### ✅ When to Use kubectl apply -f
* When managing resources using declarative configuration.
* When deploying Deployments, Services, ConfigMaps, etc..
* When updating an existing resource without deleting and recreating it.

#### 📌 Example
```sh
  kubectl apply -f my-pod.yaml
```

Where my-pod.yaml contains:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - name: my-container
    image: nginx
    ports:
    - containerPort: 80
```


#### ✅ Key Benefits
- ✔ Allows full resource definitions (Pods, Deployments, Services, etc.).
- ✔ Supports updates without deleting/recreating the resource.
- ✔ Can be stored in Git for version control (Infrastructure as Code).


### 🔹 Key Differences

| Feature                | kubectl run                                       | kubectl apply -f                                                       |
|------------------------|---------------------------------------------------|------------------------------------------------------------------------|
| **Resource Type**      | Creates only a Pod                                | Can create any resource (Pods, Deployments, Services, ConfigMaps, etc.)|
| **Requires YAML?**     | ❌ No                                              | ✅ Yes                                                                 |
| **Recommended For**    | Quick testing                                     | Production deployments, Infrastructure as Code                         |
| **Can Update?**        | ❌ No                                              | ✅ Yes (updates existing resources without deleting them)              |
| **Stores Configuration?** | ❌ No                                          | ✅ Yes (Declarative, stored in YAML)                                   |



