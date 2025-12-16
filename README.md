## 🏗️ Module 1: Workloads*Focus: Application lifecycle, scaling, and self-healing.*

Lab 1: Creating a Pod 

Create a standalone container instance.

```bash
kubectl apply -f 01-pod.yaml
kubectl get pods

```

Lab 2: Deployments (Scaling & Updates) 

Manage a stateless application.

1. **Create the Deployment:**
```bash
kubectl apply -f 02-deployment.yaml
kubectl get deployment lab2-deploy

```


2. 
**Scale (Imperative):** 
Increase replicas to handle load.


```bash
kubectl scale deployment lab2-deploy --replicas=5
kubectl get pods

```


3. 
**Rolling Update:** 
Update the image version.


```bash
kubectl set image deployment/lab2-deploy nginx=nginx:1.16.1
kubectl rollout status deployment/lab2-deploy

```


4. 
**Rollback:** 
Undo the update.


```bash
kubectl rollout undo deployment/lab2-deploy

```



###Lab 3, 4, 5: Specialized WorkloadsDeploy stateful apps, agents, and batch tasks. 

```bash
# Database Workload
kubectl apply -f 03-statefulset.yaml

# Node Agent (DaemonSet)
kubectl apply -f 04-daemonset.yaml

# Batch Jobs
kubectl apply -f 05-jobs.yaml

```

---

##📦 Module 2: Namespaces*Focus: Resource isolation.*

Lab 6: Working with Namespaces 

Create an isolated environment.

```bash
kubectl apply -f 06-namespace.yaml
kubectl get namespaces

```

---

##🔑 Module 3: Configuration*Focus: Decoupling config from code.*

Lab 7: Consuming ConfigMaps & Secrets 

This lab creates configuration data AND a specific deployment that consumes it.

1. **Apply Resources:**
```bash
kubectl apply -f 07-config-app.yaml

```


2. **Verify Consumption:**
Inspect the Pod to prove variables were injected.
```bash
# Get the Pod name
POD=$(kubectl get pod -l app=configured -o jsonpath="{.items[0].metadata.name}")

# Check ConfigMap (Theme)
kubectl exec $POD -- env | grep APP_THEME
# Output: APP_THEME=dark-mode

# Check Secret (API Key)
kubectl exec $POD -- env | grep APP_API_KEY
# Output: APP_API_KEY=supersecret

```



---

##🌐 Module 4: Networking*Focus: Exposing applications.*

Lab 8: Services 

Expose the app from Lab 2 using three different methods.

```bash
kubectl apply -f 08-services.yaml

```

**Verification:**

* **ClusterIP:** `kubectl get svc lab8-clusterip` (Internal IP only)
* **NodePort:** `kubectl get svc lab8-nodeport` (Port 30008 on nodes)
* **LoadBalancer:** `kubectl get svc lab8-lb` (Wait for EXTERNAL-IP)

---

##💾 Module 5: Storage*Focus: Persistence.*

Lab 9: Persistent Storage 

Request storage (PVC) and attach it to a Pod.

```bash
kubectl apply -f 09-storage.yaml

```

**Verification:**
Ensure the claim is bound.

```bash
kubectl get pvc lab9-pvc

```

*Status should be `Bound`.*

---

##🧹 Cleanup```bash
# Delete all lab resources
kubectl delete -f .

# (Optional) Delete GKE Cluster
gcloud container clusters delete lab-cluster --zone=us-central1-a

```

```

```
