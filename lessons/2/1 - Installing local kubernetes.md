# Minikube/Kind

## Installation

1. Install Brew: `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
1. Install Docker Desktop
1. Install kubectl `curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/arm64/kubectl"`
1. Ensure kubectl is in $PATH and working: `kubectl version`
1. Install Kind: `brew install kind`
1. Ensure the binary is present and working: `kind version`

## Local Kubernetes for development


*Create a Kind Cluster:*

You can create a new Kind cluster using the kind create cluster command. This will set up a Kubernetes cluster using Docker containers as nodes.

Example: `kind create cluster`


*Configure Kubeconfig:*

To interact with the Kind cluster using kubectl, you need to configure your kubeconfig file. Kind makes this easy with the following command:

Example: `kind get kubeconfig-path`

*Use the Cluster:*

Once your Kind cluster is up and running, you can use it just like any other Kubernetes cluster. Deploy applications, create services, and manage resources using kubectl.

Example: `kind load docker-image myapp:v1`

Example: `kubectl apply -f myapp.yaml`

Example: `kubectl get pods`

*Delete the Cluster:*

When you're done with your local development, you can delete the Kind cluster to release resources and clean up Docker containers.

Example: `kind delete cluster`


*Testing and Debugging:*

Kind is particularly useful for testing and debugging applications in a Kubernetes environment. You can iterate quickly on your code and configurations without affecting production systems.

## Minikube/Kind vs. production clusters

- Local clusters run nodes as Docker containers rather seperate compute, VMs, etc.
- Local clusters are usually fast to POC something or understand a concept.
- Local clusters have mo RBAC restrictions by default whilst Production clusters should.
- Local clusters have complete isolation from production.
