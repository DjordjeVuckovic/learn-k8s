# Learn Kubernetes # 

# Kubernetes overall architecture #

## Master node (Control plane)

- runs multiple processes to manage the cluster
- API server(api) -> entry point for all the REST, UI, CLI commands used to control the cluster
- Controller manager(c-m) -> responsible for running controllers(keep track of the state of the cluster)
- Scheduler(sched) -> ensures that the workload is running on the right node (decides which node to run the workload on)
- etcd -> key-value store to store the cluster state(record of all the objects in the cluster used by the API server)

##

## Virtual Network

- creates one unified network for all the pods to communicate with each other
- CNI(Container Network Interface) -> used to provide networking between the pods
- Flannel, Calico, Weave, etc.

## Worker node

- runs the pods
    - kubelet -> agent that runs on each node and communicates with the master node
    - kube-proxy -> maintains network rules on the nodes
    - container runtime -> software that runs the containers ( Docker, containerd, CRI-O, etc.)

# Main components of Kubernetes #

- Pod -> smallest unit of deployment in Kubernetes
- Service -> provides a way to expose an application running on a set of pods as a network service
- Ingress -> provides a way to expose HTTP and HTTPS routes from outside the cluster to services within the cluster
- Volume -> provides a way to store data in the pods
- Namespace -> provides a way to divide cluster resources between multiple users
- Deployment -> provides a way to define the desired state of the application
- StatefulSet -> provides a way to manage stateful applications
- DaemonSet -> provides a way to run a copy of a pod on all the nodes
- Job -> provides a way to run a task to completion
- CronJob -> provides a way to run a task on a time-based schedule
- ConfigMap -> provides a way to store configuration data
- Secret -> provides a way to store sensitive data
- Role-based access control (RBAC) -> provides a way to control access to the cluster
- ServiceAccount -> provides a way to authenticate to the API server
- PersistentVolume -> provides a way to store data in the cluster
- PersistentVolumeClaim -> provides a way to request storage in the cluster
- StorageClass -> provides a way to define different classes of storage
- CustomResourceDefinition (CRD) -> provides a way to extend the Kubernetes API
- Operator -> provides a way to automate tasks in the cluster
- Helm -> provides a way to package, distribute, and manage applications in the cluster
- kubectl -> provides a way to interact with the cluster

### Pod

- smallest unit of deployment in Kubernetes
- can contain one or more containers(usually one but can be more if they are tightly coupled and need to share
  resources)
- In most cases, a pod contains only one container
- So we can say that a pod is an abstraction around containers
- Very rarely, a pod can contain multiple containers that are tightly coupled and need to share resources(good example
  is a sidecar container(kerbros sidecar container))
- Containers in a pod share the same network namespace, IP address, and port space
- k8s provides a network namespace for each pod
- pods are ephemeral, they can be created, destroyed, and replaced at any time
- pods are not self-healing, if a pod crashes, it will not be restarted automatically
- pods are not exposed to the outside world, they are isolated from the network
- on recreations, pods get a new IP address
- pods are not meant to be long-lived, they are meant to be disposable
- pods need to be stateless, they should not store any data

### Service

- provides a way to expose an application running on a set of pods as a network service
- !! basically, persistent IP address for the pods and load balancing for the pods
- services are used to expose the pods to the outside world
- services are used to provide a permanent IP address for the pods
- services are used to provide load balancing for the pods
- default load balancing algorithm is round-robin
- services are used to provide service discovery for the pods
- default service type is ClusterIP
- External services can be accessed using the NodePort or LoadBalancer service types
- Internal services can be accessed using the ClusterIP service type

#### k8s service types

- ClusterIP -> exposes the service on a cluster-internal IP address
- NodePort -> exposes the service on each node's IP address at a static port
- LoadBalancer -> exposes the service externally using a cloud provider's load balancer
- ExternalName -> maps the service to a DNS name

### Ingress

- route external traffic to the services in the cluster
- provides a way to expose HTTP and HTTPS routes from outside the cluster to services within the cluster
- Ingress is an API object that manages external access to services in a cluster
- Ingress is not a service, it is a collection of rules that allow inbound connections to reach the services

### ConfigMap and Secret

- ConfigMap -> cm is external configuration data for the pods (plain text format)
- Secret -> secret is sensitive configuration data for the pods (very similar to ConfigMap but stored in base64 encoded
  format)
- Base64 encoding is not encryption, it is just an encoding format and can be easily decoded
- You need to use encryption if you want to store sensitive data utilizing external tools like HashiCorp Vault, AWS KMS,
  etc.
- Basically, ConfigMap and Secret are used to store configuration data that can be consumed by the pods

### Volume

- provides a way to store data in the pods
- attached to the pod and shared between the containers in the pod
- volumes are used to store data that needs to be shared between the containers in the pod
- k8s does not provide a way to store data in the pods, you need to use volumes to store data in the pods
- you need to take care of the data stored in the volumes, as the data is not replicated across the nodes

### Namespace

- provides a way to divide cluster resources between multiple environments or users

### Deployment

- blueprint for the pods (stateless applications)
- provides a way to define the desired state of the application
- set up replication and self-healing for the pods
- abstracts from pods
- provides a way to update the application without downtime
- deployments are used only for stateless applications

### StatefulSet

- blueprint for the pods (stateful applications)
- provides a way to manage stateful applications
- apps like databases, caches,elastic etc.
- take of pods in a specific order where the read and write operations are synchronized
- need to be managed in a specific order
- it is not easy to scale up and down the stateful applications
- usually we should use cloud resources for the stateful applications and it is really hard to manage the stateful
  applications in the k8s

### ReplicaSet

- provides a way to ensure that a specified number of pod replicas are running at all times
- ReplicaSet is a higher-level abstraction around pods
- Self-healing mechanism for the pods (if a pod crashes, it will be restarted automatically)

# Kubernetes configuration #

- All the configuration in k8s is gone using the API server(part of the master node/control plane)
- kubernetes clients(kubectl,ui,api) interact with the API server
- k8s uses the declarative approach to manage the configuration(yml or json configuration files)
- we define the desired state of the application in the configuration files( deployment, service, etc.)

### Configuration files

- 3 ways to create the configuration files:
  - Imperative way -> kubectl create command
  - Imperative way -> kubectl run command
  - Declarative way -> configuration files(yml or json)


- Each configuration file contains parts:
    - apiVersion -> version of the k8s API
    - kind -> type of the object
    - metadata -> data about the object
    - specification (spec) -> desired state of the object
    - status -> current state of the object

- status is not included in the configuration files, it is managed by the k8s
- k8s uses the status to compare the desired state and the current state of the object.If the current state is different
  from the desired state, k8s will try to fix the state. State is managed by the controllers in the control plane and because of that k8s is self-healing.
- etcd holding current state of the objects

### minikube

- minikube is a tool that makes it easy to run Kubernetes locally
- minikube runs a single-node Kubernetes cluster inside a virtual machine on your local machine
- control plane and worker node are running on the same machine

### Communication between the components

- 3 ways to communicate with the k8s API server(part of the master node/control plane):
  - kubectl -> command line tool
  - k8s API -> REST API
  - k8s UI -> web-based UI


### kubectl

- kubectl is a command-line tool that is used to interact with the Kubernetes cluster
- kubectl is the main way to interact with the Kubernetes cluster
- kubectl is used to create, update, delete, and get information about the objects in the cluster
- most powerful tool to interact with the k8s

### Creating manifests

- manifests are configuration files that define the desired state of the application

#### Labels
- each k8s component can have a label
- Labels are key-value pairs that are attached to the objects in the cluster
- Labels are additional identifying information that can be used to group and select objects
- Why we need labels?
- Each pod has unique name, but we have multiple pods with the same label. This ensures that you can identify all replicas of the same pod bu the matching label.
- We are defining labels for the pods in template section of the deployment configuration file.
- Selector is used to select the pods based on the labels. With matchingLabels we know which pods are part of witch deployment.