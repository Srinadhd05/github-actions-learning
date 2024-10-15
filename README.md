# Kubernetes

Kubernetes, also known as K8s, is an open-source system for automating deployment, scaling, and management of containerized applications.
Kubernetes Cluster: A Kubernetes (K8s) cluster is a group of computing nodes, or worker machines, that run containerized applications.

Reference: https://kubernetes.io/docs/concepts/overview/components/

## Kubernetes Architecture
![image](https://github.com/user-attachments/assets/9601f96c-188a-43c1-a711-594b18543e25)

## Core Components:
* Control plane
* Node

## Control plane components:
The control plane's components make global decisions about the cluster (for example, scheduling), as well as detecting and responding to cluster events (for example, starting up a new pod when a Deployment's replicas field is unsatisfied).

### kube-apiserver:
The core component server that exposes the Kubernetes HTTP API. It's the hub for all operational commands in a Kubernetes cluster, it accepts and processes RESTful requests to manage and orchestrate Kubernetes resources like pods, services, and replication controllers.

### etcd:
Consistent and highly-available key value store used as Kubernetes' backing store for all cluster data. Etcd is also useful to set up the desired state for the system.

### kube-scheduler: 
Control plane component that watches for newly created Pods with no assigned node, and selects a node for them to run on.scheduling decisions include: individual and collective resource requirements, hardware/software/policy constraints, affinity and anti-affinity specifications, data locality, inter-workload interference, and deadlines.

### kube-controller-manager:
Control plane component that runs controller processes.Logically, each controller is a separate process, but to reduce complexity, they are all compiled into a single binary and run in a single process.

### cloud-controller-manager: 
A Kubernetes control plane component that embeds cloud-specific control logic. The CCM inherits its functions from components of Kubernetes that are dependent on a cloud provider.

## Node components:
Node components run on every node, maintaining running pods and providing the Kubernetes runtime environment.

### kubelet:
An agent that runs on each node in the cluster. It makes sure that containers are running in a Pod.

### kube-proxy:
kube-proxy is a network proxy that runs on each node in your cluster, kube-proxy maintains network rules on nodes. These network rules allow network communication to your Pods from network sessions inside or outside of your cluster.

### Container runtime:
It is responsible for managing the execution and lifecycle of containers within the Kubernetes environment.
 

## Draft
Below are popular providers of Kubernetes cluster platform as a service.
	• Amazon EKS
	• Azure AKS
	• Google GKE
	• Openshift

Pod: Pods contain one or more containers and, within the pod, containers share the same system resources such as storage and networking. Each pod gets a unique private IP address, which can be used communication between pods within cluster using VPC-CNI (container network interface).

Kubectl: Kubernetes provides a command line tool for communicating with a Kubernetes cluster's control plane, using the Kubernetes API. This tool is named kubectl.





