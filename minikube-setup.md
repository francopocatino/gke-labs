# Minikube Setup

Get Kubernetes running locally (free).

## Install Minikube

**macOS:**
```bash
brew install minikube
```

**Verify:**
```bash
minikube version
```

## Install kubectl (if not already installed)

```bash
brew install kubectl
```

## Start Your First Cluster

```bash
# Start Minikube
minikube start

# This creates a local Kubernetes cluster
# Takes 1-2 minutes first time
```

**What it does:**
- Creates a VM (Docker or VirtualBox)
- Installs Kubernetes inside
- Configures kubectl to use it

## Verify It Works

```bash
# Check cluster is running
kubectl cluster-info

# See your node
kubectl get nodes

# Should show:
# NAME       STATUS   ROLE           AGE   VERSION
# minikube   Ready    control-plane  30s   v1.28.x
```

## Basic Commands

```bash
# Start cluster
minikube start

# Stop cluster (keeps data)
minikube stop

# Delete cluster (removes everything)
minikube delete

# Open dashboard (web UI)
minikube dashboard

# SSH into the node
minikube ssh
```

## Test Deployment

```bash
# Create a simple deployment
kubectl create deployment hello --image=nginx

# See it running
kubectl get deployments
kubectl get pods

# Expose it
kubectl expose deployment hello --type=NodePort --port=80

# Access it
minikube service hello
```

## Common Issues

**"minikube start" fails:**
- Make sure Docker Desktop is running
- Try: `minikube start --driver=docker`

**kubectl not working:**
```bash
# Point kubectl to minikube
minikube update-context
```

**Cluster too slow:**
```bash
# Give it more resources
minikube start --cpus=4 --memory=4096
```

## Ready to Learn

Now you have a local Kubernetes cluster!

Next: Deploy lab02-spring to Minikube
