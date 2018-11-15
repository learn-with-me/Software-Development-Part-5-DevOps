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
