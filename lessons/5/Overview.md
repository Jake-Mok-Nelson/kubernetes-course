Lesson 5: DaemonSets and Specialized Workloads
Objectives:

Explore specialized workloads using DaemonSets.
Understand when and why to use DaemonSets in Kubernetes.
Topics Covered:

Introduction to DaemonSets

1. What are DaemonSets?
- Use cases for DaemonSets
- Managing Node-Level Services

2. Running monitoring agents
- Logging collectors with DaemonSets
- Deploying Network Infrastructure


Activities:

Practical examples of using DaemonSets for node-level services and network infrastructure.


*Tip:* This lesson works across multiple nodes so you may want to create a kind cluster that has multiple nodes (by default there is only one):

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
```

```bash
kind create cluster --config kind-config.yaml
```
