# Kubernetes Deployment Basics

## What is a Deployment ?

A **Deployment in Kubernetes** is a higher-level object that manages Pods and ensures the desired number of replicas are always running.  
It provides **scaling, self-healing, rolling updates, and rollbacks** automatically.  
Deployments are used in production to safely manage **stateless applications** instead of handling Pods directly.

---

## Difference btw Container , Pods & Deployments ?

### Container
* Container is the smallest runnable unit in Docker.
* It packages application code and its dependencies.

### Pod
* Pods are wrappers over containers and can run **one or more containers**.
* All containers in a Pod share the **same network namespace and storage**.
* Pods are **ephemeral in nature**, just like containers.
* Pods are **declarative**, meaning everything about a Pod can be defined in a YAML file.

### Deployment
* Deployment is the main reason why Kubernetes is used over plain Docker.
* It removes the need to **manually manage containers and Pods**.
* Deployment ensures the desired state of the application using controllers.

---

## What is a ReplicaSet ?

* ReplicaSet is a **Kubernetes controller** that ensures a specified number of Pods are always running.
* It is responsible for **auto-healing and maintaining replicas**.
* Deployment **creates and manages ReplicaSets**, and ReplicaSets create Pods.
* ReplicaSet is implemented internally using **Go** as part of Kubernetes control plane.

Deployment  
↓  
ReplicaSet  
↓  
Pods

---

## Why ReplicaSets are important

If a Pod gets deleted by mistake or crashes, the application would normally go down.

* ReplicaSet detects the missing Pod
* It **creates a new identical Pod automatically**
* This provides **self-healing**

For zero downtime updates:
* Deployment creates a **new ReplicaSet**
* New Pods are started **before old Pods are terminated**
* This ensures **rolling updates with minimal or zero downtime**

Hence, in production environments, **Deployments are used instead of standalone Pods**.

---

## Creating a Deployment

### Start the cluster
```bash
minikube start
```
### Verify nothing exists yet
```bash

kubectl get pods
kubectl get deploy
kubectl get rs
```

### Create the Deployment
```bash
kubectl create -f deploy.yml
```

### Verify created resources
```bash

kubectl get pods
kubectl get deploy
kubectl get rs
```

## Testing Self-Healing
### Delete a Pod manually
```bash

kubectl delete pod <pod-name>
```

### Watch live behavior
```bash

kubectl get pods -w
```

### What happens

* ReplicaSet detects Pod deletion

* A new Pod is created automatically

* New Pod has a different name and IP

* All this behavior is achieved because of ReplicaSets managed by Deployments.

Now you might be wondering how can someone still access the application if the ip address of the new pod is different ? Again this question will be answered by me only but in the very next file ;)
Stay Tuned & Hydrated!!
