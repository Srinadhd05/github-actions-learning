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
 

## Good to know
Below are popular providers of Kubernetes cluster platform as a service:
* Amazon EKS
* Azure AKS
* Google GKE
* Openshift

### CLI's:
* AWS CLI: We can authenticate and interact with AWS resources.
* Kubectl: We can control kubernetes cluster and objects using kubectl.
* eksctl: It can be used to manage EKS AWS resources, like cluster creation or deletion, managing node groups and fargate profiles.

We can interact and configure Kubernetes resources in two different ways: Imperative or Declarative
* Imperative configuration means that to describe the configuration of the resource while executing a command from a terminal’s command prompt.
  	Example: kubctl run ngnix --image=ngnix:latest
* Declarative configuration means that you create a file that describes the configuration for the particular resource and then apply the content of the file to the Kubernetes cluster.

## Kubernetes objects

### Pod: 
Pods contain one or more containers and, within the pod, containers share the same system resources such as storage and networking. Each pod gets a unique private IP address, which can be used communication between pods within cluster using VPC-CNI (container network interface).

![image](https://github.com/user-attachments/assets/81982b2f-5876-4c7d-8314-777b4b40db4d)

* Pod's are not accessible outside of cluster by default.
* Containers within a pod are not isolated. Think of a pod as similar to a virtual machine (VM), with containers similar to applications running on the VM. Pods and groups of pods can be organized by attaching attribute labels to them, such as labeling ‘dev’ or ‘prod’ for the type of environment. 
* We cannot have multiple containers of same kind running a single POD, example: running 2 tomcat pods serving same purpose cannot have in same pod.

### Imperative approch:
kubctl run ngnix --image=ngnix:latest  ---> Command will create a pod with one nginix container running

```
kubectl run --help   					---> helps to explore command options with examples
kubctl run ngnix --image=ngnix:latest --port=80 	---> Creates a pod with name ngnix using ngnix latest tagged image with port 80 mapping 
kubctl get pods   					---> Gets the list of running pods.
kubctl get pods -o wide  				---> Gives additional information about running pods
kubctl logs <<name of the pod>>  			---> Command to check pod logs.
kubectl describe pod <<name of the pod>>  		---> Command to view complete details about pod, like name,node,volumes, events etc..
kubectl delete pod <<name of the pod>>   		---> Command used to delete a pod
kubectl logs <<name of the pod>> -c <<container name>>  ---> A pod can have more than one container, this command will help in getting logs of a specific container from a pod.
```
### Declarative approch with Yaml

Kubernetes objects can be created, updated, and deleted by storing multiple object configuration files in a configuration file and using kubectl apply to recursively create and update those objects as needed.

### Comments & Key Value Pairs
- Space after colon is mandatory to differentiate key and value
```yml
# Defining simple key value pairs
name: Srinadh
age: 29
city: Hyderabad
```

## Dictionary / Map
- Set of properties grouped together after an item
- Equal amount of blank space required for all the items under a dictionary
```yml
person:
  name: Srinadh
  age: 29
  city: Hyderabad
```

## Array / Lists
- Dash indicates an element of an array
```yml
person: # Dictionary
  name: Srinadh
  age: 29
  city: Hyderabad
  hobbies: # List  
    - cycling
    - cooking
  hobbies: [cycling, cooking]   # List with a differnt notation  
```  

## Step-04: Multiple Lists
- Dash indicates an element of an array
```yml
person: # Dictionary
  name: Srinadh
  age: 23
  city: Hyderabad
  hobbies: # List  
    - cycling
    - cooking
  hobbies: [cycling, cooking]   # List with a differnt notation  
  friends: # 
    - name: friend1
      age: 22
    - name: friend2
      age: 25            
```  


## Step-05: Sample Pod Tempalte for Reference
```yml
apiVersion: v1 # String
kind: Pod  # String
metadata: # Dictionary
  name: myfirst-app
  labels: # Dictionary 
    app: myfirst-app        
spec:
  containers: # List
    - name: myapp
      image: nginx:latest
      ports:
        - containerPort: 80
          protocol: "TCP"
        - containerPort: 81
          protocol: "TCP"
```







