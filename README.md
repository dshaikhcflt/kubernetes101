🏗️ Module 1: Workloads
Focus: Managing application lifecycle and scaling.

Lab 1: Creating a Pod
Create a standalone container instance.

Bash

kubectl apply -f 01-pod.yaml
kubectl get pods
Lab 2: Deployments (Scaling & Updates)
Manage a stateless application.


Create the Deployment:

Bash

kubectl apply -f 02-deployment.yaml
kubectl get deployment lab2-deploy
Scale the Application: Increase the number of replicas to handle more load.

Bash

kubectl scale deployment lab2-deploy --replicas=5
kubectl get pods
Rolling Update: Update the application image version (from 1.14.2 to 1.16.1).


Bash

kubectl set image deployment/lab2-deploy nginx=nginx:1.16.1
kubectl rollout status deployment/lab2-deploy
Rollback: Undo the update and revert to the previous version.

Bash

kubectl rollout undo deployment/lab2-deploy
Labs 3, 4, 5: Specialized Workloads
Deploy specific workload types for databases, agents, and batch tasks.


StatefulSet: For stateful applications like databases.


Bash

kubectl apply -f 03-statefulset.yaml

DaemonSet: For running background agents on every node.


Bash

kubectl apply -f 04-daemonset.yaml

Job & CronJob: For one-time and recurring tasks.


Bash

kubectl apply -f 05-jobs.yaml
kubectl get jobs
kubectl get cronjobs
📦 Module 2: Namespaces
Focus: Resource isolation.

Lab 6: Working with Namespaces
Create an isolated environment called lab6-space.


Bash

kubectl apply -f 06-namespace.yaml
kubectl get namespaces
🔑 Module 3: Configuration & Secrets
Focus: Decoupling configuration from code.

Lab 7: Consuming ConfigMaps & Secrets
This lab deploys a ConfigMap, a Secret, and a specific Deployment that consumes them.


Apply Resources:

Bash

kubectl apply -f 07-config-app.yaml
Verify Consumption: Confirm the variables were injected into the Pod environment.

Bash

# Get the pod name
POD=$(kubectl get pod -l app=configured -o jsonpath="{.items[0].metadata.name}")

# Check ConfigMap injection (Theme)
kubectl exec $POD -- env | grep APP_THEME
# Expected Output: APP_THEME=dark-mode

# Check Secret injection (API Key)
kubectl exec $POD -- env | grep APP_API_KEY
# Expected Output: APP_API_KEY=supersecret
🌐 Module 4: Networking
Focus: Exposing applications.

Lab 8: Services
Expose the application using ClusterIP, NodePort, and LoadBalancer.

Bash

kubectl apply -f 08-services.yaml
Verification:


Internal: kubectl get svc lab8-clusterip (Cluster-internal IP).


External: kubectl get svc lab8-lb (Wait for EXTERNAL-IP).

💾 Module 5: Storage
Focus: Persisting data.

Lab 9: Persistent Storage
Request storage using a Persistent Volume Claim (PVC) and attach it to a Pod.


Apply Resources:

Bash

kubectl apply -f 09-storage.yaml
Verify Status: Ensure the claim is bound to a volume.

Bash

kubectl get pvc lab9-pvc
Status should be Bound.

🧹 Cleanup
Remove all resources to avoid costs or clutter.

Bash

# Delete all resources created by the YAML files
kubectl delete -f .

# (Optional) Delete the GKE Cluster
gcloud container clusters delete lab-cluster --zone=us-central1-a
