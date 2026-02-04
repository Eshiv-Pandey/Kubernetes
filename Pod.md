# Creating a Kubernetes Pod

## What is a Pod in Kubernetes?

A **Pod** is the smallest deployable unit in Kubernetes.

A Pod is a logical wrapper around one or more containers that:
- Run together on the same node
- Share the same network namespace (same IP address and ports)
- Share storage volumes
- Are scheduled and managed as a single unit

In most real-world use cases, a Pod runs **one primary container**.

---

## Difference Between a Container and a Pod

### Container (Docker)
- Imperative by default using `docker run`
- Runs a single application process
- No built-in orchestration
- Manual networking and restarts
- Not self-healing

### Pod (Kubernetes)
- Declarative using YAML manifests
- Can run multiple containers
- Managed by Kubernetes
- Shared networking and storage
- Self-healing(via replica sets) and reschedulable

### Key Notes
- Pods are declarative, meaning the desired state is defined in a YAML file.
- Multiple containers in a Pod are mainly used for sidecar patterns like logging or monitoring.
- Pods are ephemeral and should not be created directly in production.

---

## What is Minikube?

Minikube is a tool that allows you to run a **single-node Kubernetes cluster locally**.

### Use Cases
- Learning Kubernetes
- Local development
- Testing Kubernetes manifests
- Practicing kubectl commands

---

## Kubernetes Cluster Tools Comparison

| Tool | Purpose | Production Usage |
|-----|--------|------------------|
| Minikube | Local Kubernetes cluster | No |
| kind | Kubernetes in Docker | No |
| k3s | Lightweight Kubernetes | Limited |
| kubeadm | Manual cluster bootstrap | Yes |
| kOps | Automated cluster management | Yes |
| EKS | Managed Kubernetes (AWS) | Yes |

### Important Clarification
- kubeadm is open-source and free
- EKS is paid because it is a managed cloud service
- Minikube, kind, and k3s are intended for local or pre-production usage only

---

## Required Tools

To work with Kubernetes locally, you need:
1. kubectl
2. Minikube

---

## Installing kubectl

### Official Documentation
https://kubernetes.io/docs/tasks/tools/

### Install kubectl on Ubuntu

```bash
sudo apt update
sudo apt install -y apt-transport-https ca-certificates curl
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.29/deb/Release.key | sudo tee /etc/apt/keyrings/kubernetes.asc
echo "deb [signed-by=/etc/apt/keyrings/kubernetes.asc] https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /" | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt update
sudo apt install -y kubectl
````

### Verify Installation

```bash
kubectl version --client
```

---

## Installing Minikube

### Official Documentation

[https://minikube.sigs.k8s.io/docs/start/](https://minikube.sigs.k8s.io/docs/start/)

### Install Minikube on Ubuntu

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

### Verify Installation

```bash
minikube version
```

---

## Starting the Kubernetes Cluster

```bash
minikube start
```

### What This Command Does

* Creates a local VM or container
* Installs Kubernetes components
* Configures kubectl context automatically

### Check Cluster Status

```bash
kubectl get nodes
```

You should see one node in `Ready` state.

---

## Creating a Pod

### Step 1: Create pod.yml

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
    - name: nginx
      image: nginx
      ports:
        - containerPort: 80
```

---

### Step 2: Create the Pod

```bash
kubectl create -f pod.yml
```

### Explanation

* Submits the YAML to the Kubernetes API server
* Scheduler assigns the Pod to a node
* kubelet pulls the image and starts the container

---

## Verifying the Pod

### List Pods

```bash
kubectl get pods
```
<img width="400" height="54" alt="image" src="https://github.com/user-attachments/assets/564ccf10-6810-4d7b-962c-51dc61b33b69" />


### Detailed Pod View

```bash
kubectl get pods -o wide
```
<img width="924" height="53" alt="image" src="https://github.com/user-attachments/assets/62ca5510-f53a-424c-bf6d-bae7d24e49e4" />



This command shows:

* Pod IP address
* Node name
* Pod status

---

## Accessing the Pod

Pods are internal by default.

### SSH into Minikube Node

```bash
minikube ssh
```

### Access the Pod

```bash
curl <POD_IP>
```

If nginx is running, you will see the nginx welcome page.

### Exit SSH

```bash
Ctrl + D
```

---

## Inspecting the Pod

### Describe Pod

```bash
kubectl describe pod nginx
```
<img width="1436" height="886" alt="image" src="https://github.com/user-attachments/assets/2dd1552e-00bb-4b88-ba6a-cc01b28efd2c" />

Shows:

* Pod events
* Container state
* Scheduling details

---

### View Pod Logs

```bash
kubectl logs nginx
```

Useful for debugging application output.

---

## Deleting the Pod

```bash
kubectl delete pod nginx
```
<img width="531" height="53" alt="image" src="https://github.com/user-attachments/assets/847b51c5-1a59-433f-97c9-1e714c89a42b" />


### What Happens After Deletion

* Container is stopped
* Pod is permanently removed
* No automatic recreation since it is not managed by a controller

---

## Key Takeaways

* Pods are the smallest Kubernetes unit
* Pods are declarative and ephemeral
* Do not use standalone Pods in production
* Minikube is meant only for local development as it is does not manages the cluster like EKS , KOps does. Also it's mostly used by students and developers for practicing before going to paid options.
* Always use Deployments for real applications.
  Deployments will be discussed later by me only :)

```
