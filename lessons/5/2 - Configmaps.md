# ConfigMaps - Managing Configuration Data and Configuring Pods

## What are ConfigMaps?

ConfigMaps are Kubernetes resources used to store configuration data in a key-value format. In this section, we'll delve into the fundamentals of ConfigMaps and their significance in containerized environments.

The role of ConfigMaps in decoupling configuration from application code.


## Use Cases for ConfigMaps

ConfigMaps are versatile and serve various use cases in Kubernetes. In this section, we'll explore scenarios where ConfigMaps shine and why they are crucial.

Use Cases:

- Separating configuration from application code.
- Managing environment-specific settings.
- Configuring applications dynamically.

## Creating and Managing ConfigMaps

Here's an example configmap from kubernetes.io:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: game-demo
data:
  # property-like keys; each key maps to a simple value
  player_initial_lives: "3"
  ui_properties_file_name: "user-interface.properties"

  # file-like keys
  game.properties: |
    enemy.types=aliens,monsters
    player.maximum-lives=5    
  user-interface.properties: |
    color.good=purple
    color.bad=yellow
    allow.textmode=true  
```

There are four different ways that you can use a ConfigMap to configure a container inside a Pod:

- Inside a container command and args
- Environment variables for a container
- Add a file in read-only volume, for the application to read
- Write code to run inside the Pod that uses the Kubernetes API to read a ConfigMap

*Applying a configmap to a pod:*

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: configmap-demo-pod
spec:
  containers:
    - name: web
      image: nginx:latest
      command: ["sleep", "3600"]
      env:
        # Define the environment variable
        - name: PLAYER_INITIAL_LIVES # Notice that the case is different here
                                     # from the key name in the ConfigMap.
          valueFrom:
            configMapKeyRef:
              name: game-demo           # The ConfigMap this value comes from.
              key: player_initial_lives # The key to fetch.
        - name: UI_PROPERTIES_FILE_NAME
          valueFrom:
            configMapKeyRef:
              name: game-demo
              key: ui_properties_file_name
      volumeMounts:
      - name: config
        mountPath: "/config"
        readOnly: true
  volumes:
  # You set volumes at the Pod level, then mount them into containers inside that Pod
  - name: config
    configMap:
      # Provide the name of the ConfigMap you want to mount.
      name: game-demo
      # An array of keys from the ConfigMap to create as files
      items:
      - key: "game.properties"
        path: "game.properties"
      - key: "user-interface.properties"
        path: "user-interface.properties"
```

## Tips when working with Configmaps

* Updating a configmap doesn't re-deploy the target pod, you'll have to do that manually if it's a pod/configmap configuration.
* Treating configmaps as immutable and deploying new ones with each change is good practice. These can be associated with deployments to handle automatic rollout.
* When using OS ENV Vars vs config files consider the use cases for the target app, how it works, and what is clearer for the engineer maintaining it.
* The point of ConfigMaps is to seperate configuration from Application so ideally avoid baking ConfigMap logic into your application.

## Activities

Try adjusting the values in the configmap and re-applying it. 

Try to get the values to reflect in the pod and verify them.

Try to imagine cases where you might want to use configmaps.
