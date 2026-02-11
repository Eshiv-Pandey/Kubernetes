# What are Services in Kubernetes?

A Service in Kubernetes is an abstraction that provides a stable network identity to access a set of Pods.

## Pods are ephemeral (IP changes on restart), but a Service gives:

* **A stable IP / DNS name**

* **Load balancing across Pods**

* **Service discovery** : Service discovery in Kubernetes means **Pods find each other using service names instead of IP addresses**.
In k8s, each service uses labels to tag pods. The service defines selectors the match these labels.K8s automatically track all pods who's labels matches with the selectors. Traffic sent to the service is load balanced to these matching pods, enabling service discovery without hardcoded IPs.

  
* **Expose to external world** : Cluster IP, NodePort Mode IP, Load Balancer

In short:

Pods come and go, Services stay.

## Why Services are needed

Without a Service:

* Every Pod has a dynamic IP

* Clients would break when Pods restart

With a Service:

* Traffic is routed to healthy Pods automatically

* Clients talk to the Service, not directly to Pods

## Types of Services

### ClusterIP

Default

Exposes service inside the cluster only

### NodePort

Exposes service inside your org. That means whosoever has access to the worker node can access the application/service as well.

### LoadBalancer

Uses cloud provider LB (AWS ELB, GCP LB, etc.). When a service is created as type LoadBalancer, the CCM(Cloud Control Manager) creates an external load balancer IP using the underlying cloud logic in the CCM.
Users/Clients can access apps/services via this external Ip.

### Headless Service

No load balancing

Direct Pod DNS resolution (used in StatefulSets)


## Offtopic --> How auto-scaling happens in Kubernetes (under the hood)

Auto-scaling in Kubernetes is NOT done by Services.
Services only route traffic.

Scaling is handled by controllers.

## Popular Interview Questions -->
How to add Auto-Scaling and Auto-Healing Capabilities ? 
-We use Deployments

What is a namespace in Kubernetes ?
-A namespace in kubernetes is a logical seperation of resources, networking policies and RBAC so that multiple project teams can work on the same k8s cluster without interupting the work of each other .

Role of kube-proxy ?
-Ensures services can communicate with each other.

Difference btw Docker Swarm & Kubernetes ?
-Docker Swarm : simple easy to use, suits small-medium sized apps, has basic orchestration and offers limited tools
-Kubernetes : complex but powerful, handles large sized apps easily, offers auto-scaling and auto-healing along with huge number of tools and excellent community support.

## What's Next??
We will deep dive into k8s, learn Ingress, RBAC, k8s custom resources etc all will be taught by me only :)
