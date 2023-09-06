# Introduction to kubectl

Kubectl is a command-line tool for interacting with Kubernetes clusters.

It interacts with the Kubernetes API running on the master node to manage the cluster and its workloads.

## Basic commands (get, create, delete)

Kubectl has a verb first syntax.

Example: `kubectl describe <something>`

```
create:          Create a resource from a file or from stdin
expose:          Take a replication controller, service, deployment or pod and expose it as a new Kubernetes service
run:             Run a particular image on the cluster
set:             Set specific features on objects
explain:         Get documentation for a resource
get:             Display one or many resources
edit:            Edit a resource on the server
delete:          Delete resources by file names, stdin, resources and names, or by resources and label selector
```

## Interacting with Kubernetes resources (pods, services)

*kubectl options:*

--namespace or -n: Specifies the namespace for the operation.
--context: Sets the context to use for the operation, which can be helpful when managing multiple clusters.
--kubeconfig: Specifies the path to the kubeconfig file.
--help or -h: Displays help information for kubectl.

*kubectl create:*

kubectl create is used to create resources in a Kubernetes cluster.

Example: Create a new Deployment from a YAML file.

`kubectl create -f deployment.yaml`

*kubectl delete:*

kubectl delete is used to delete resources from a Kubernetes cluster.

Example: Delete a Pod by name.

`kubectl delete pod my-pod`

*kubectl get:*

kubectl get is used to retrieve information about resources in a Kubernetes cluster.

Example: List all Pods in the default namespace.

`kubectl get pods`

*kubectl describe:*

kubectl describe provides detailed information about a resource, including its current status and configuration.

Example: Describe a Pod named "my-pod."

`kubectl describe pod my-pod`

*kubectl apply:*

kubectl apply is used to create or update resources based on their configuration files.

Example: Apply changes in a YAML file to the cluster.

`kubectl apply -f updated-deployment.yaml`

kubectl edit:*

kubectl edit opens the resource's configuration in the default text editor for manual editing.

Example: Edit a Deployment named "my-deployment."

`kubectl edit deployment my-deployment`

*kubectl exec:*

kubectl exec allows you to run commands inside a running container of a Pod.
Example: Run a shell session in a Pod named "my-pod."

`kubectl exec -it my-pod -- /bin/bash`

### Get vs Describe

Kubectl get is used for quickly listing resources, making it suitable for a high-level overview, while kubectl describe is used for obtaining detailed information about a specific resource, which is useful for troubleshooting and understanding its current state and configuration. 

*Example Output of kubectl get pod my-pod:*

```
NAME     READY   STATUS    RESTARTS   AGE
my-pod   1/1     Running   0          2h
```

NAME: The name of the Pod is "my-pod."

READY: Indicates the number of containers in the Pod that are in the "Running" state out of the total expected.

STATUS: The current status of the Pod, which is "Running."

RESTARTS: The number of times the containers in the Pod have been restarted.

AGE: The age of the Pod, which is 2 hours.


*Example Output of kubectl describe pod my-pod:*

```
Name:         my-pod
Namespace:    default
Priority:     0
Node:         node-1/192.168.1.101
Start Time:   Tue, 14 Sep 2023 10:00:00 +0000
Labels:       app=my-app
              environment=production
Annotations:  <none>
Status:       Running
IP:           10.244.0.2
IPs:
  IP:  10.244.0.2
Containers:
  my-container:
    Container ID:   docker://abcdef1234567890
    Image:          my-image:v1
    Image ID:       docker-pullable://my-image@sha256:1234567890abcdef
    Port:           80/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Tue, 14 Sep 2023 10:00:10 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-abcde (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  default-token-abcde:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-abcde
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  2h    default-scheduler  Successfully assigned default/my-pod to node-1
  Normal  Pulling    2h    kubelet, node-1   Pulling image "my-image:v1"
  Normal  Pulled     2h    kubelet, node-1   Successfully pulled image "my-image:v1"
  Normal  Created    2h    kubelet, node-1   Created container my-container
  Normal  Started    2h    kubelet, node-1   Started container my-container

```

Name: The name of the Pod is "my-pod."

Namespace: The namespace in which the Pod resides (default in this case).

Node: The node where the Pod is scheduled.

Start Time: The time when the Pod was started.

Labels and Annotations: Metadata associated with the Pod.

Status: The overall status of the Pod, which is "Running."

Containers: Information about containers within the Pod, including image details and readiness.

Conditions: The status conditions of the Pod.

Volumes: Volumes associated with the Pod.

Events: A timeline of events related to the Pod's lifecycle.

kubectl describe pod provides a detailed view of the Pod, which can be helpful for troubleshooting and understanding its configuration and status.

### Tips for Kubectl

Always check your namespace. Make sure that you're in the one you intend.

`kubectl config get-context` will show the currently configured clusters, which is set as the currently targeted cluster
and which namespace is being targeted.

Set aliases:

Typing `kubectl X` gets really old, really fast. Configured you ~/.bashrc or ~/.zshrc file with aliases based on your preferences.

`alias k=kubectl`

Setting the namespace is annoying to do if you regularly switch. I shorten it with:

```
setns() {
    kubectl config set-context --current --namespace=$1
}
```

Then switching between namespaces like dev and prod becomes:

```bash
> setns dev
> setns prod
```

