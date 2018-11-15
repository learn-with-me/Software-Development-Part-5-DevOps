# Kubernetes (K8s)
Definition: Open source platform designed to automate deploying, scaling and operating application containers. 
Goal: to foster an ecosystem of components and tools that relieve the burden of running applications in public and private clouds.

```
A platform to schedule and run containers on clusters of virtual machines.
It runs on bare metal, virtual machines, private datacenter and public cloud.
```

### Features
  - Multi-host container scheduling
    - Done by kube-scheduler
    - Assigns pods to nodes/host at runtime
    - Accounts for resources, quality of service, policies and user specifications before scheduling
  - Scalability and Availability
    - K8s master can be deployed in a highly available configuration
    - Multi-region deployments are available
    - Architectire in v1.8 supports upto 5000 node clusters and can run 150,000 pods
    - Pods can be horizontally scaled via an API
    - Scalability features
        - Registration: New worker nodes can seamlessly register themselves with master node
        - Service discovery: Allows automatic detection of new services and endpoints via DNS or environment variables
  - Flexibility and Modularization
    - Plug-and-play architecture
    - Extend architecture when needed
    - Add-ons: Network drivers, service discovery, container runtime, visualization and command
  - Persistent Storage
    - Pods can use persistent volumes to store data
    - Data retained across pod restarts and crashes
  - Application upgrades and Application rollbacks
  - Maintenance: turn off/on host during maintenance
  - Logging and monitoring
    -  Monitoring is built-in
    -  TCP, HTTP, container execution health check
    -  Node health check with failures monitored by node controller
    -  Kubernetes status can be monitored by Heapster and cAdvisor
    -  Use existing logging frameworks like ELK
  - Secret Management
    - Mounted as data volumes or environment variables
    - Specific to namespace

### Architecture
  - Master node - responsible for overall management of Kubernetes cluster
    - API server (FE) - Communication
    - Scheduler - Scheduling
    - Cluster/Controller Manager - Controllers
  - etcd - Simple distributed key-value store, K8s uses it as its database and stores all cluster data here. Data like job scheduling info, pod details, state information, etc.
  - kubectl - command-line application to interact with Master node
    - config file: kubeconfig
        - server information
        - authentication information to access api server
  - Worker node - nodes where applications operate
    - kubelet - process that handles communication to master node. Agent that communicates with the API server.
    - Docker - run containers on the node
        - pod - containers of an application are tightly coupled in a pod. Pod is a smallest unit that can be scheduled for deployment with Kubernetes. The group of containers within pod share storage, linux namespace, IP addresses, etc
    - kube-proxy - process for network proxy and load balancer. Also handles network routing for TCP and UDP packets.


##### Node
Serves as a worker machine in K8s cluster. 
Kubernetes in production recommends atleast a three-node cluster

###### Requirements
- A kubelet running
- Container tooling like Docker
- A kube-proxy process running
- a process like supervisord

##### Minikube
Tool to run K8s locally. A light-weight K8s implementation that creates a VM on your local machine and deploys a simple cluster containing only one node.

##### Pods
Simples unit that you can interact with. You can create, deploy and delete pods. It represents one running process on your cluster. If a pod dies for some reason, it will not be rescheduled. Always use higher-level constructs to manage and add stability to pods called controllers.

`Don't use a pod directly. Use a controller instead like a deployment.`

###### what's in here?
- Docker application container
- Storage resources
- Unique network IP
- Options that govern that how a container should run

###### States
- Pending - Pod has been accepted by the K8s system but the container has not been created yet.
- Running - Pod has been scheduled on a node and all of its containers are created and atleast one container is in a running state.
- Succeeded - All containers in a pod have exited with an exit status of 0. This indicates successful execution and will not be restarted.
- Failed - All the container in the pod have exited and atleast one container has failed and returned a non-zero exit status.
- CrashLoopBackOff - Container fails to start for some reason and K8s tries over and over and over again to start the pod.


##### Controllers

###### Benefits
- Application reliability - Multiple instances of an application running, prevent problem if one or more instances fail
- Scale - When pod experience high volume of requests, K8s allow us to scale up the pods
- Load balancing - Multiple versions of the pod running allow traffic to flow through different pods and doesn't overload one single pod or node.

###### Type of Controllers
- ReplicaSets - Ensures that a specified number of replicas for a pod are running at all times. If the number of pods are less that ReplicaSets expect, it'll start up a new pod.
- Deployments - provides declarative updates for pods and ReplicaSets. Internally Deployment manages ReplicaSets, which internally manages Pod. Hence deployments can automatically support rollback mechanism.
    - Use Cases
        - Pod management: Running a ReplicaSet allows us to deploy a number of pods, and check their status as a single unit.
        - Scaling a ReplicaSets scales out the pods, and allows for the deployment to handle more traffic.
- DaemonSets - Ensures that all the nodes run a copy of specific pod. As nodes are added or removed from the cluster, a DaemonSet will add or remove the required pods. Deleting the DaemonSets will cleanpup all the pods it created.
- Jobs - Supervisor process for pods carrying out batch jobs. Used to run individual processes that run once and complete successfully, example Cron job.
- Services - Provides network connectivity to one or more pods in your cluster. Allow the communication between one set of deployments with another. Each service is assigned a unique IP address that never changes through the lifetime of the service. Pods are then configured to talk to the service and then rely on the service IP for any request that might be sent to the pod. Use a service to get two pods in two deployments to talk to each other. Using a pod IP to communicate is a bad choice since they can change at anytime.
    - Internal Services - IP is only reachable only within the cluster
    - External Services - endpoint available through node ip:port (NodePort)
    - Load balancer - Exposes application to the interent

