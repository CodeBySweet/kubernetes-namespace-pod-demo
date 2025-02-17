
## Kubernetes Namespace and Pod Management
This guide provides step-by-step instructions for managing Kubernetes Namespaces and Pods using both imperative and declarative approaches.

### Table of Contents
1. List Namespaces
2. Create Namespace - Imperative
3. Describe Namespace
4. Delete Namespace
5. Create Namespace - Declarative
6. Create Pod - Declarative

### 1. List Namespaces
**1. List all Namespaces in the Cluster**
To list all Namespaces in the Kubernetes cluster, run the following commands:
```bash
kubectl get namespace
# OR
kubectl get ns
```
### 2. Create Namespace - Imperative
**1. Create a Namespace**
Create a Namespace named app1-dev:
```bash
kubectl create namespace app1-dev
```
Verify the Namespace creation:
```bash
kubectl get namespace
```
**2. Add Labels to the Namespace**
Add the following labels to the app1-dev Namespace:
- app=app1
- env=dev
```bash
kubectl label namespace app1-dev app=app1 env=dev
```
Verify the labels:
```bash
kubectl get namespace app1-dev --show-labels
```
### 3. Describe Namespace
**1. Describe the Namespace**
To view detailed information about the app1-dev Namespace, including its labels, run:
```bash
kubectl describe namespace app1-dev
```
### 4. Delete Namespace
**1. Delete the Namespace**
Delete the app1-dev Namespace:
```bash
kubectl delete namespace app1-dev
```
Verify the deletion:
```bash
kubectl get namespace
```
### 5. Create Namespace - Declarative
**1. Create a Namespace using a YAML File**
Create a file named app1-ns.yaml with the following content:
```yaml
apiVersion: v1  # Kubernetes API version
kind: Namespace  # Type of resource (Namespace)
metadata:  # Metadata about the resource
  name: app1-dev  # Name of the Namespace
  labels:  # Labels for organizing resources
    app: app1  # Label for the application
    env: dev  # Label for the environment (development)
```
Apply the YAML file to create the Namespace:
```bash
kubectl apply -f app1-ns.yaml
```
Verify the Namespace creation:
```bash
kubectl get namespace app1-dev --show-labels
```
### 6. Create Pod - Declarative
**1. Create a Pod using a YAML File**
Create a file named nginx_pod2.yaml with the following content:
```yaml
apiVersion: v1  # Kubernetes API version
kind: Pod  # Type of resource (Pod)
metadata:  # Metadata about the Pod
  name: app1-pod-1  # Name of the Pod
  namespace: app1-dev  # Namespace where the Pod will be created
  labels:  # Labels for organizing resources
    app: app1  # Label for the application
    env: dev  # Label for the environment (development)
spec:  # Specification of the Pod's contents
  containers:  # List of containers in the Pod
    - name: nginx  # Name of the container
      image: nginx:latest  # Docker image to use for the container
      ports:  # Ports to expose from the container
        - containerPort: 80  # Expose port 80 for the container
```
Apply the YAML file to create the Pod:
```bash
kubectl apply -f nginx_pod2.yaml
```
Verify the Pod creation:
```bash
kubectl get pods -n app1-dev
```
Describe the Pod for detailed information:
```bash
kubectl describe pod app1-pod-1 -n app1-dev
```
**2. Clean Up**
Delete the Pod and Namespace:
```bash
kubectl delete pod app1-pod-1 -n app1-dev
kubectl delete namespace app1-dev
```

### Notes
- Ensure you have kubectl configured and access to a Kubernetes cluster.
- Use --dry-run=client to test YAML files without applying them.
- Always verify resource creation and deletion using kubectl get commands.
