# Kubernetes Services

In Kubernetes, a Service is an abstraction that defines a set of Pods and a policy by which to access them. Services enable you to expose your application's functionality either within the cluster or to external users or systems. This lesson covers how Services work and the different service types.

## Exposing Applications within the Cluster


Kubernetes Services are primarily used to expose applications within the cluster. Here's how it works:

*Pods and Labels:*

First, you deploy your application as Pods, each with a set of labels. Labels are key-value pairs that help identify Pods.

*Service Definition:*

You define a Service using a YAML or JSON manifest. In this manifest, you specify the labels that the Service should target (selector).

*Service Creation:*

When you create the Service, Kubernetes selects Pods with matching labels and creates a stable network endpoint for them. This endpoint is known as the Service's ClusterIP.

*Internal Access:*

Other Pods within the cluster can access the Service using its ClusterIP. This allows for easy communication between different parts of your application.


## Service types (ClusterIP, NodePort, LoadBalancer)


Kubernetes supports several Service types, each designed for different use cases:

### ClusterIP

Purpose: The default Service type.

Function: Exposes the Service on a cluster-internal IP.

Use Case: Ideal for internal services that need to communicate with each other.

Example Service manifest:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
```


### NodePort

Purpose: Exposes the Service on a static port on each node's IP.

Function: Allows external access to the Service by reaching any node on the specified port.

Use Case: Use when you need to expose a Service externally for development or testing purposes.

Example Service manifest:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: NodePort
```

### LoadBalancer

Purpose: Exposes the Service using a cloud provider's load balancer.

Function: Distributes incoming traffic across Pods.

Use Case: Ideal for production environments where you need external access with load balancing and automatic scaling.

Example Service manifest (for cloud providers with LoadBalancer support):

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: LoadBalancer
```

