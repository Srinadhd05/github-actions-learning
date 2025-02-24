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

# Kubernetes objects

## Pod: 
Pods contain one or more containers and, within the pod, containers share the same system resources such as storage and networking. Each pod gets a unique private IP address, which can be used communication between pods within cluster using VPC-CNI (container network interface).

![image](https://github.com/user-attachments/assets/81982b2f-5876-4c7d-8314-777b4b40db4d)

* Pod's are not accessible outside of cluster by default.
* Containers within a pod are not isolated. Think of a pod as similar to a virtual machine (VM), with containers similar to applications running on the VM. Pods and groups of pods can be organized by attaching attribute labels to them, such as labeling ‘dev’ or ‘prod’ for the type of environment. 
* We cannot have multiple containers of same kind running a single POD, example: running 2 tomcat pods serving same purpose cannot have in same pod.

### Playgrounds for kubernetes

* [Killercoda](https://killercoda.com/playgrounds/scenario/kubernetes)
* [Play with Kubernetes](https://labs.play-with-k8s.com/)

## Imperative & Declarative

We can interact and configure Kubernetes resources in two different ways: Imperative or Declarative
* Imperative configuration means that to describe the configuration of the resource while executing a command from a terminal’s command prompt.
  	Example: kubctl run ngnix --image=ngnix:latest
* Declarative configuration means that you create a file that describes the configuration for the particular resource and then apply the content of the file to the Kubernetes cluster.

## Imperative approch:
kubctl run ngnix --image=ngnix:latest  ---> Command will create a pod with one nginix container running

```bash
kubectl run --help   					---> helps to explore command options with examples
kubctl run ngnix --image=ngnix:latest --port=80 	---> Creates a pod with name ngnix using ngnix latest tagged image with port 80 mapping 
kubctl get pods   					---> Gets the list of running pods.
kubctl get pods -o wide  				---> Gives additional information about running pods
kubctl logs <<name of the pod>>  			---> Command to check pod logs.
kubectl describe pod <<name of the pod>>  		---> Command to view complete details about pod, like name,node,volumes, events etc..
kubectl delete pod <<name of the pod>>   		---> Command used to delete a pod
kubectl logs <<name of the pod>> -c <<container name>>  ---> A pod can have more than one container, this command will help in getting logs of a specific container from a pod.
kubectl exec -it <pod-name> -- /bin/bash   ---> Connect to Nginx Container in a POD
kubectl exec -it <pod-name> ls   ---> Running individual commands in a Container
kubectl get <<kubernetes object>> <<Name of the object>> -o yaml   ---> Get pod definition YAML output   
```
## Declarative approch with Yaml

Kubernetes objects can be created, updated, and deleted by storing multiple object configurations in a file and using kubectl apply to recursively create and update those objects as needed.

Use `kubectl apply -f <<.yaml>>` to create all objects defined by configuration files in a specified directory

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

## Multiple Lists
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


## Sample Pod Tempalte for Reference
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
## ReplicaSet

A ReplicaSet's is used to maintain a stable set of replica Pods running at any given time. It is often used to guarantee the availability of a specified number of identical Pods.
It uses below cofiguration for creating and deleting Pods as needed to reach the desired number.

* selector: specifies how to identify Pods it can acquire
* replicas: indicating how many Pods it should be maintaining
* template: pod template specifying the data of new Pods it should create to meet the number of replicas criteria

```yml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: frontend
  labels:
    app: guestbook
    tier: frontend
spec:
  # modify replicas according to your case
  replicas: 3
  selector:
    matchLabels:
      tier: frontend
  template:
    metadata:
      labels:
        tier: frontend
    spec:
      containers:
      - name: php-redis
        image: us-docker.pkg.dev/google-samples/containers/gke/gb-frontend:v5
```

## Deployments

A Deployment manages a set of Pods to run an application workload, it helps in rollout updates and also performing rollback to earlier version of deployments.
A Deployment provides declarative updates for Pods and ReplicaSets.You describe a desired state in a Deployment, and the Deployment Controller changes the actual state to the desired state at a controlled rate.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 4
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

```bash
kubectl create -f nginx-test.yaml #creates deployment with yaml file.
kubectl get deployment  #Check the deployment.
kubectl get rs  #Cheeck the replicasets.
kubectl get pod  #Check the pods up and running.
```
### Ensure Zero Downtime
We can specify the update strategy. Create under the spec category.

```yaml
minReadySeconds: 5
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxSurge: 1
    maxUnavailable: 1
```
* `minReadySeconds` tells Kubernetes how long it should wait until it creates the next pod. This property ensures that all application pods are in the ready state during the update.
* `maxSurge` specifies the maximum number (or percentage) of pods can be creayed above the specified number of replicas.
   Example: In the above code, the maximum number of pods that can be created will be 5 since 4 replicas are specified in the yaml file.
* `maxUnavailable` declares the maximum number (or percentage) of pods that can go unavailable during the update. If maxSurge is set to 0, this field cannot be 0.

With above configuration kubernetes can start performing rolling updates, but it does not guarantee zero downtime. Because, kubernetes cannot tell when a new pod is ready, usually it eliminates the old pod as soon as the new one is created. It will not be able to validate if pod is ready to accept the requests.

Kubernetes `Readiness Probes` helps in solving the problem. The probes check the state of pods and allow for rolling updates to proceed only when all of the containers in a pod are ready. Pods are considered ready when the readiness probe is successful and after the time specified in `minReadySeconds` has passed.

Create `Readiness Probes` under `spec.template.spec` category in the deployment file.
```yaml
readinessProbe:
  httpGet:
    path: /
    port: 8080
  initialDelaySeconds: 5
  periodSeconds: 5
  successThreshold: 1
```
* `initialDelaySeconds` specifies how long the probe has to wait to start after the container starts.
* `periodSeconds` is the time between two probes. The default is 10 seconds, while the minimal value is 1 second.
* `successThreshold` is the minimum number of consecutive successful probes after a failed one for the entire process to be considered successful. The default and minimal values are both 1.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 4
  selector:
    matchLabels:
      app: nginx
  minReadySeconds: 5
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.0
        ports:
        - containerPort: 80
        readinessProbe:
          httpGet:
            path: /
            port: 8080
          initialDelaySeconds: 5
          periodSeconds: 5
          successThreshold: 1
```

```bash
kubectl apply -f nginx-test.yaml --record
```
### Perform Rolling Update

There are three ways to peform rolling updates.
*  Using command `kubectl set` to perform the action.
   ```bash
   kubectl set image deployment nginx-deployment nginx=nginx:1.14.2 --record
   ```
* Modifying the image version in `spec.templates.spec.containers` section of the yaml file. Then, use kubectl replace to perform the update.
  ```bash
  kubectl replace -f nginx-test.yaml
  ```
* Using `kubectl edit` to edit the deployment directly.
  ```bash
  kubectl edit deployment nginx-deployment --record
  ```
### Check Rollout Status
```bash
kubectl rollout status deployment nginx-deployment
```
### Pause and Resume Rolling Update
```bash
kubectl rollout pause deployment nginx-deployment  # Command to pasue rollout
kubectl rollout resume deployment nginx-deployment # Command tp resume rollout
```
### Rollback Changes
We can rollback changes and revert to a previous version of the app.
```bash
kubectl rollout history deployment nginx-deployment
```
The output lists the available revisions, created by adding the --record flag when performing an update

Choose the revision you want and type the following command to rollback the changes.
```bash
kubectl rollout undo deployment nginx-deployment --to-revision=1 # The command above rolls back to revision 1, specify revision based on deployment history output.
```
## Schedule Pods for Deployment
There are multiple ways to control on which nodes Kubernetes schedules specific pods in your deployment.
* nodeselector
* Affinity and anti Affinity
* [nodename](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#nodename)
* [pod-topology-spread-constraints](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#pod-topology-spread-constraints)
 
### Pod Affinity
There are two types of affinity currently available in Kubernetes:

* `requiredDuringSchedulingIgnoredDuringExecution` The scheduler can't schedule the Pod unless the rule is met. This functions like nodeSelector, but with a more expressive syntax.
* `preferredDuringSchedulingIgnoredDuringExecution` The scheduler tries to find a node that meets the rule. If a matching node is not available, the scheduler still schedules the Pod.

These properties are listed in the PodSpec file.
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: affinity-test
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: kubernetes.io/test-name
            operator: In
            values:
            - test1
            - test2
      preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 1
        preference:
          matchExpressions:
          - key: example-node-label-key
            operator: In
            values:
            - example-node-label-value
  containers:
  - name: affinity-test
    image: k8s.gcr.io/pause:2.0
```
The file above tells Kubernetes to run the pod only on a node with a label whose key is kubernetes.io/test-name and whose value is either test1 or test2. Furthermore, Kubernetes will prefer nodes whose key is example-node-label-key, with the example-node-label-value value.

### Pod Anti-Affinity
Pod anti-affinity is useful if you do not want all the pods to run on the same node. It functions similarly to affinity, with the same two types available - requiredDuringSchedulingIgnoredDuringExecution and preferredDuringSchedulingIgnoredDuringExecution.

The following example specifies an anti-affinity rule that tells Kubernetes to preferably avoid scheduling the "test" app pods to nodes that already have the "test" pods.

```yaml
podAntiAffinity:
  preferredDuringSchedulingIgnoredDuringExecution:
  - weight: 100
    podAffinityTerm:
      labelSelector:
        matchExpressions:
        - key: app
          operator: In
          values:
          - test
      topologyKey: Kubernetes.io/hostname
```
### nodeSelector
nodeSelector is the simplest recommended form of node selection constraint. You can add the nodeSelector field to your Pod specification and specify the node labels you want the target node to have. Kubernetes only schedules the Pod onto nodes that have each of the labels you specify.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
  nodeName: kube-01
```

### Taints and Tolerations

Node affinity is a property of Pods that attracts them to a set of nodes (either as a preference or a hard requirement). Taints are the opposite -- they allow a node to repel a set of pods.

Reference for more details on [Taints and Tolerations](https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/)

```bash
kubectl taint nodes node1 key1=value1:NoSchedule
```
This means that no pod will be able to schedule onto node1 unless it has a matching toleration.

We can specify a toleration for a pod in the PodSpec, and thus a pod with either toleration would be able to schedule onto node1
```yaml
tolerations:
- key: "key1"
  operator: "Equal"
  value: "value1"
  effect: "NoSchedule"

tolerations:
- key: "key1"
  operator: "Exists"
  effect: "NoSchedule"
```

## Volumes
On-disk files in a container are ephemeral, which presents some problems for non-trivial applications when running in containers.
To prevent this data loss and run a stateful application on Kubernetes, we need to adhere to three simple storage requirements:

* Storage must not depend on the pod lifecycle.
* Storage must be available for all pods and nodes in the Kubernetes cluster.
* Storage must be highly available regardless of crashes or application failures.

Kubernetes has several types of storage options available, not all of which are persistent.

### Ephemeral storage: 

Containers can use the temporary filesystem (tmpfs) to read and write files. However, ephemeral storage does not satisfy the three storage requirements. In case of a container crash, the temporary filesystem is lost—the container starts with a clean slate again. Also, multiple containers cannot share a temporary filesystem.

### Ephemeral volumes: 

An ephemeral Kubernetes Volume solves both of the problems faced with ephemeral storage. An ephemeralVolume‘s lifetime is coupled to the Pod. It enables safe container restarts and sharing of data between containers within a Pod. However as soon as the Pod is deleted, the Volume is deleted as well, so it still does not fulfill our three requirements.

![2 -Pods-and-Volumes](https://github.com/user-attachments/assets/789ee58a-a9aa-43a3-89e6-37e7cc5d8233)

### Decoupling pods from the storage: Persistent Volumes 

Kubernetes also supports Persistent Volumes. With Persistent Volumes, data is persisted regardless of lifecycle of the application, container, Pod, Node, or even the cluster itself. Persistent Volumes fulfill the three requirements outlined earlier. 

A Persistent Volume (PV) object represents a storage volume that is used to store persist application data. A PV has its own lifecycle, separate from the lifecycle of Kubernetes Pods. 

A PV essentially consists of two different things: 

* A backend technology called a PersistentVolume 
* An access mode, which tells Kubernetes how the volume should be mounted.

### Backend technology
A PV is an abstract component, and the actual physical storage must come from somewhere. Here are a few examples: 

* csi: Container Storage Interface (CSI) → (for example, Amazon EFS, Amazon EBS, Amazon FSx, google discs , azure files) 
* iscsi: iSCSI (SCSI over IP) storage
* local: Local storage devices mounted on nodes
* nfs: Network File System (NFS) storage

Kubernetes is versatile and supports many different types of PVs. Kubernetes does not care about the underlying storage internals; it just gives us the PV component as an interface to the actual storage. 

There are three major benefits to a PV:

* A PV is not bound to the lifecycle of a Pod: when removing a Pod that is attached to a PV object, the PV will survive. 
* The preceding statement is also valid when a Pod crashes: the PV object will survive the fault and not be removed from the cluster.
* A PV is cluster-wide: it can be attached to any Pod running on any Node in the cluster.
* All different backend storage technologies have their own performance characteristics and tradeoffs. For this reason, we see different types of PVs in production Kubernetes environments that depend on the application.

### Access mode 

The access mode is set during PV creation and tells Kubernetes how the volume should be mounted. Persistent Volumes support three access modes: 

* ReadWriteOnce: Volume allows read/write by only one node at the same time. 
* ReadOnlyMany: Volume allows read-only mode by many nodes at the same time.
* ReadWriteMany: Volume allows read/write by multiple nodes at the same time.

Not all PersistentVolume types support all access modes. 

### Persistent volume claims:

A Persistent Volume (PV) represents an actual storage volume. Kubernetes has an additional layer of abstraction necessary for attaching a PV to a Pod: the PersistentVolumeClaim (PVC). 

Essentially, a Pod cannot mount a PV object directly. It needs to explicitly ask for it. And that asking action is achieved by creating a PVC object and attaching it to the Pod. This is the only reason why this additional layer of abstraction exists. PVCs and PVs have a one-to-one mapping (a PV can only be associated with a single PVC). 

![3 Persistent-Volume-and-Persistent-Volume-Claim](https://github.com/user-attachments/assets/212128d1-ef15-4bf3-93b1-82eb9b94ee46)

### Container Storage Interface (CSI) drivers 

The Container Storage Interface (CSI) is an abstraction designed to facilitate using different storage solutions with Kubernetes. Different storage vendors can develop their own drivers that implement the CSI standards, enabling their storage solutions to work with Kubernetes (regardless of the internals of the underlying storage solution). AWS has CSI plugins for Amazon EBS, Amazon EFS , and Amazon FSx for Lustre.

### Static provisioning 

In what we described in the “Persistent volume claims” section, first the administrator creates one or more `PV`, and then the application developer creates a `PVC`. This is called static provisioning. It is static because you have to manually create the `PV` and the `PVC` in Kubernetes. At scale this can become more and more difficult to manage, especially if you are managing hundreds of `PVs` and `PVCs`.

### Dynamic provisioning 

With dynamic provisioning, you do not have to create a `PV` object. Instead, it will be automatically created under the hood when you create the `PVC`. Kubernetes does so using another object called `Storage Class`. 

A `Storage Class` is an abstraction that defines a class of backend persistent storage (for example, Amazon EFS file storage, Amazon EBS block storage, etc.) used for container applications. 

A `Storage Class` essentially contains two things: 

* Name: This is the name, which uniquely identifies the storage class object.
* Provisioner: This defines the underlying storage technology. For example, provisioner would be `efs.csi.aws.com` for Amazon EFS or `ebs.csi.aws.com` for Amazon EBS. 

The `Storage Class` objects are the reason why Kubernetes is capable of dealing with so many different storage technologies. From a `Pod` perspective, no matter whether it is an EFS volume, EBS volume, NFS drive, or anything else, the Pod will only see a `PVC` object. All the underlying logic dealing with the actual storage technology is implemented by the provisioner the `Storage Class` object uses.

![4 Dynamic-Provisioning](https://github.com/user-attachments/assets/e54a1183-0573-4419-b4d2-303666b88221)

### Static Provisioning Demo: 

* Need to create EFS file system. 
* We create the PV manifest file and provide the FileSystemId of the newly created file system. (The PV status is Available, but it is not yet bound with any PVC. ) 
* We create the persistent volume claim.

The PV status has now changed from Available to Bound, which means that Kubernetes has been able to find a volume match using the PVC, and the volume has been bound. 

Now if we tried to create another PVC, it would fail because we don’t have any more PVs left (a PV can be bound to a single PVC) – that is where dynamic provisioning would come in handy.

![6 Static-Provisioning](https://github.com/user-attachments/assets/9c087f3c-05aa-40c4-b579-5b6a82132621)

### Dynamic provisioning using Amazon EFS 

Amazon EFS CSI driver supports both dynamic provisioning and static provisioning. For EFS, dynamic provisioning creates an access point for each `PV` under the hood. This means you have to create an Amazon EFS file system manually and provide it as an input to the `Storage Class` parameters. 

By default each access point created via dynamic provisioning writes files under a different directory on EFS, and each access point writes files to EFS using a different POSIX uid/gid. This enables multiple applications to use the same EFS volume for persistent storage while providing isolation between applications. 

* let’s create a new Amazon EFS volume (myEFS2) that we will be using for dynamic provisioning.
* We need to create a Storage Class and provide the FileSystemId of the newly created file system.
   * kubectl get sc  ---> verify the Storage Class
* As mentioned in the “Dynamic provisioning” section, we don’t have to create any PV before deploying our application. So, you can go ahead and create a PVC and Pod. 

If you repeat the process and create another application (`efs-app-2`), we don’t have to worry about the `PV` because it will create another access point under the hood.

![9 Dynamic-Provisioning](https://github.com/user-attachments/assets/5584184d-1a1b-48a1-9b1c-c20fa2707130)








