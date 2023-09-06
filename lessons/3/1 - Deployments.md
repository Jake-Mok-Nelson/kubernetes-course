# Deployments

In Kubernetes, a Deployment is a resource that allows you to define, create, and manage a set of identical Pods. Deployments are essential for maintaining the desired state of your applications and managing their updates.

## Creating Deployments

To create a Deployment in Kubernetes, you need to define its configuration using a YAML or JSON manifest file. 

Below is an example of a simple Deployment manifest:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: my-container
          image: my-image:latest
          ports:
            - containerPort: 80
```
Breaking down the configuration above we are using:

apiVersion: Specifies the API version for the Deployment.

kind: Indicates that this is a Deployment resource.

metadata: Provides metadata such as the name of the Deployment.

spec (deployment): Defines the desired state of the Deployment.

replicas: Specifies the desired number of replicas (Pods).

selector: Specifies how the Deployment selects Pods to manage.

template: Defines the Pod template for creating replicas.

metadata: Labels for Pods created by this template.

spec (pod): Defines the containers to run in the Pods.

---

To create the Deployment from this manifest, you can use the kubectl apply command:

`kubectl apply -f deployment.yaml`

This will create the Deployment, and the specified number of Pods (replicas) will be created.

## Rolling Updates and Rollbacks
Deployments provide a powerful way to manage rolling updates and rollbacks of your application.

### Rolling Updates
When you need to update your application, such as deploying a new version of your container image, you can do so without downtime using a rolling update. To update a Deployment, you can change the image version in the Deployment manifest or other relevant configuration changes. Then, apply the updated manifest:

`kubectl apply -f updated-deployment.yaml`

Kubernetes will automatically perform a rolling update, creating new Pods with the updated configuration and gradually replacing the old Pods. You can monitor the update progress using kubectl get pods.

### Rollbacks


In case an update introduces issues or errors, you can perform a rollback to a previous version of the Deployment. Kubernetes tracks the revision history of Deployments, allowing you to revert to a specific revision.

To initiate a rollback, use the kubectl rollout undo command:

`kubectl rollout undo deployment/my-deployment`

This command will rollback the Deployment to the previous revision, effectively reverting the changes made during the update.

Remember to keep your Deployment manifests and configuration under version control to facilitate tracking and managing updates and rollbacks effectively.


You could also revert changes to the deployment manifest and re-apply it but is not strictly speaking a rollback:

`kubectl apply -f updated-deployment.yaml`
