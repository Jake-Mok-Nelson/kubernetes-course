# Scaling Applications

Kubernetes provides powerful mechanisms for scaling your applications to meet varying levels of demand. Scaling can be done manually or automatically, and it can involve horizontal or vertical scaling. Let's explore these concepts.

## Manual scaling

### Horizontal Scaling

Horizontal scaling, also known as scaling out, involves increasing the number of instances (Pods) running your application to handle increased load.

To manually scale a Deployment, you can use the kubectl scale command:

`kubectl scale deployment/my-deployment --replicas=5`

This command scales the specified Deployment, my-deployment, to have 5 replicas (Pods). You can adjust the replica count as needed.

*Tip:* Remember that this would ideally be defined with the deployment manifest and not run ad-hoc.

### Vertical Scaling

Vertical scaling, also known as scaling up, involves increasing the resources (CPU and memory) allocated to individual Pods.

To manually scale a Deployment by adjusting resource requests and limits, you can update the Deployment manifest with the desired resource specifications and then apply the changes:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 3
  template:
    spec:
      containers:
        - name: my-container
          image: my-image:latest
          resources:
            requests:
              memory: "256Mi"
              cpu: "0.5"
            limits:
              memory: "512Mi"
              cpu: "1"
```

In this example, we've specified resource requests and limits for CPU and memory. Applying this manifest will trigger a rolling update to adjust the resource allocation for each Pod.



## Automatic scaling

### Horizontal Pod Autoscaling (HPA)

Kubernetes supports automatic horizontal scaling using Horizontal Pod Autoscaling (HPA). HPA monitors resource utilization (e.g., CPU or memory) and scales Pods based on defined metrics and thresholds.

To create an HPA resource, you can define an HPA manifest, such as:

```yaml
apiVersion: autoscaling/v2beta2
kind: HorizontalPodAutoscaler
metadata:
  name: my-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-deployment
  minReplicas: 2
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 50

```

In this example, the HPA targets the my-deployment, scales between 2 and 10 replicas, and maintains CPU utilization at 50%.
