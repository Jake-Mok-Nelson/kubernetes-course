# What are DaemonSets?

*Trivia:* Daemon was coined in 1963 by programmers as a background process that would work tirelessly to complete chores but the word originates from ancient greek where it referred to a supernatural being of great power, sometimes refferred to as a Guardian spirit.

DaemonSets are a Kubernetes resource that ensures that a specific Pod runs on all (or a subset of) nodes in a cluster. Unlike other controllers, they operate at the node level rather than the application level.

*Key Characteristics:*

A DaemonSet ensures that one copy of a Pod runs on each node in the cluster.

It automatically adds or removes Pods as nodes are added or removed from the cluster.

DaemonSets are useful for tasks that need to run on every node, such as monitoring agents, log collectors, or networking proxies.


```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: nginx-daemonset
spec:
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
        image: nginx:latest
```

apiVersion: Specifies the API version for the DaemonSet resource.

kind: Indicates that this is a DaemonSet.

metadata: Provides metadata for the DaemonSet, including its name.

spec: Defines the desired state of the DaemonSet.

selector: Specifies the labels used to select nodes where the DaemonSet's Pods should be scheduled. In this case, it selects nodes with the label app: nginx.

template: Defines the Pod template used to create Pods on each node.

metadata: Labels the Pods with app: nginx.

spec: Specifies the configuration for the Pods.

containers: Defines the container to run within the Pods, using the Nginx image.


## Use Cases for DaemonSets

*Monitoring and Logging:*

DaemonSets can deploy monitoring and logging agents to every node, ensuring that you can collect data from all parts of the cluster.

*Networking:*

Networking proxies or load balancers can be deployed as DaemonSets to handle traffic routing and load balancing at the node level.

*Security and Compliance:*

Security agents and compliance tools can be deployed as DaemonSets to enforce policies and monitor node-level security.



## Managing Node-Level Services

*Deploying Node-Level Services:*

DaemonSets are an effective way to deploy services that need to run on each node, such as service meshes or node-local databases.

*Ensuring Redundancy:*

DaemonSets help ensure that critical services are redundant across nodes, enhancing the availability and reliability of your cluster.

*Updates and Rollbacks:*

Learn how to update and rollback DaemonSets to manage changes or issues in node-level services.
Activities
Hands-on activities in this lesson include:

Creating a DaemonSet to deploy a monitoring agent on all nodes.
