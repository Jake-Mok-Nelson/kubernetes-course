# Kubernetes Architecture

[Kubernetes APIs](https://kubernetes.io/docs/concepts/overview/kubernetes-api/)
[Architecture](https://kubernetes.io/docs/concepts/architecture/)

## Clusters

![Kubernetes Components](components-of-kubernetes.svg)


## Nodes: Master and Worker

### Master

Control Plane: The master node hosts the Kubernetes control plane components, including the API server, etcd (a distributed key-value store for cluster configuration), the scheduler, and the controller manager. These components collectively manage cluster state and control the desired state of applications.

API Server: The API server is the entry point for interacting with the Kubernetes cluster. It exposes the Kubernetes API, which users, administrators, and automation tools use to communicate with the cluster.

Cluster State Management: The master node maintains the desired state of the cluster, continuously reconciling it with the current state. It schedules and manages container deployments, ensuring that the applications run as specified.

Scaling and Load Balancing: The master node handles scaling decisions, such as replicating pods (groups of containers) across worker nodes. It also manages load balancing for services, distributing incoming traffic to the appropriate pods.

High Availability: In production environments, the master node is often set up in a high-availability configuration to ensure fault tolerance and reliability. This typically involves having multiple master nodes in a clustered setup.


#### Control Plane

API Server: The API server is the entry point for interacting with the Kubernetes cluster. It exposes the Kubernetes API, which users, administrators, and automation tools use to communicate with the cluster. It's a core component of the control plane.

etcd: Etcd is a distributed key-value store that stores the configuration data and the desired state of the cluster. It is a critical component for maintaining cluster state and ensuring consistency.

Scheduler: The scheduler is responsible for placing pods onto worker nodes based on resource requirements, affinity, anti-affinity rules, and other policies. It determines where to run pods in the cluster.

Controller Manager: The controller manager includes various controllers that regulate the state of the cluster. For example, the Replication Controller ensures that the desired number of pod replicas are running, and the Node Controller monitors the health of worker nodes

### Worker Nodes (minions)

- Run application containers (workloads).
- Can scale to meet demands.


### Other Nodes

Other nodes types are available but used only as configured/requried.

Etcd Nodes: Etcd is a distributed key-value store that stores the configuration data and the desired state of the cluster. It is typically deployed on separate machines, often in a clustered setup, to ensure reliability and fault tolerance. These machines running etcd can be thought of as etcd nodes.

Ingress Nodes: These nodes are configured to handle incoming external traffic and manage routing to the appropriate services within the cluster. They are part of the network layer of Kubernetes.

GPU Nodes: In clusters that require GPU acceleration, some nodes may be equipped with GPUs to support workloads that require graphics processing.

Storage Nodes: In clusters with specific storage needs, nodes may be designated for hosting storage-related services, such as distributed file systems or storage providers.

