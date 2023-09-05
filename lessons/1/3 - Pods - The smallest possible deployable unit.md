# Pods: The smallest possible deployable unit

A pod contains containers.

It groups containers together as required and handles the lifecycle of those containers.

- Use single container pods by default
- Add secondary containers only to support the primary: referred to as Sidecars.

Purposes/Examples of secondary (sidecar) containers:

- Monitoring
- File transfer
- Initalisers
- Proxies
- Debugging
- Security scanning

## The smallest pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: nginx
```

apiVersion: specifies the Kubernetes API version.

kind: specifies that this is a Pod resource.

metadata: contains metadata about the Pod, including its name.

spec: specifies the desired state of the Pod.

containers: we define a single container named my-container using the Nginx image as an example.

## More involved pods

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
    environment: production
spec:
  containers:
    - name: nginx-container
      image: nginx:latest
      ports:
        - containerPort: 80
          protocol: TCP
      resources:
        requests:
          memory: "64Mi"
          cpu: "250m"
        limits:
          memory: "128Mi"
          cpu: "500m"
      env:
        - name: APP_ENV
          value: "production"
      livenessProbe:
        httpGet:
          path: /
          port: 80
        initialDelaySeconds: 10
        periodSeconds: 5
      readinessProbe:
        httpGet:
          path: /
          port: 80
        initialDelaySeconds: 5
        periodSeconds: 3
```

The spec section contains more details:

- We specify a container named "nginx-container" using the latest Nginx image.
- The container exposes port 80 for incoming traffic.
- Resource requests and limits are set for CPU and memory usage.
- Environment variables are defined using the env field.
- Liveness and readiness probes are configured to check the health of the container.

## Securing pods

[My blog post on this](https://medium.com/digio-australia/securing-your-container-workloads-in-kubernetes-369d18c2a006?source=friends_link&sk=898a290ac0619b8e24a511fb44ed03fe)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
    environment: production
spec:
  securityContext: # Pod securityContext
    fsGroup: 15555
  containers:
  - name: usertest
    image: jakesmnelson/usertest
    securityContext: # Container securityContext
      readOnlyRootFilesystem: true
      runAsUser: 15555
      runAsGroup: 15555
      privileged: false
      allowPrivilegeEscalation: false
```


## Implementations of PODs

Kubernetes provides several built-in workload resources:

Deployment and ReplicaSet: Deployment is a good fit for managing a stateless application workload on your cluster, where any Pod in the Deployment is interchangeable and can be replaced if needed.

StatefulSet: lets you run one or more related Pods that do track state somehow. For example, if your workload records data persistently, you can run a StatefulSet that matches each Pod with a PersistentVolume. Your code, running in the Pods for that StatefulSet, can replicate data to other Pods in the same StatefulSet to improve overall resilience.

DaemonSet: defines Pods that provide facilities that are local to nodes. Every time you add a node to your cluster that matches the specification in a DaemonSet, the control plane schedules a Pod for that DaemonSet onto the new node. Each pod in a DaemonSet performs a job similar to a system daemon on a classic Unix / POSIX server. A DaemonSet might be fundamental to the operation of your cluster, such as a plugin to run cluster networking, it might help you to manage the node, or it could provide optional behavior that enhances the container platform you are running.

Job and CronJob: provide different ways to define tasks that run to completion and then stop. You can use a Job to define a task that runs to completion, just once. You can use a CronJob to run the same Job multiple times according a schedule.

