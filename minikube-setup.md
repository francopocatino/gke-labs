# Minikube Setup

Local Kubernetes cluster. Free, runs on laptop.

## Installation

```bash
# macOS
brew install minikube
brew install kubectl

# Verify
minikube version
kubectl version --client
```

## Core Commands

```bash
# Start cluster (creates VM, installs K8s, configures kubectl)
minikube start

# Stop (keeps data)
minikube stop

# Delete (removes everything)
minikube delete

# Status
kubectl cluster-info
kubectl get nodes

# Dashboard (web UI)
minikube dashboard

# SSH into node
minikube ssh
```

## Quick Test

```bash
# Deploy nginx
kubectl create deployment hello --image=nginx

# Check status
kubectl get deployments
kubectl get pods

# Expose as service
kubectl expose deployment hello --type=NodePort --port=80

# Access in browser
minikube service hello
```

## Resource Allocation

```bash
# Default: 2 CPUs, 2GB RAM
minikube start

# Custom resources
minikube start --cpus=4 --memory=4096
```

## Troubleshooting

**Start fails:**
- Ensure Docker Desktop is running
- Try: `minikube start --driver=docker`

**kubectl can't connect:**
```bash
minikube update-context
```

**Image pull issues:**
```bash
# Use Minikube's Docker daemon
eval $(minikube docker-env)
```

## Notes

- First start takes 1-2 minutes
- Cluster runs in VM (Docker or VirtualBox)
- kubectl auto-configured to use Minikube context
- Stop when not in use (saves battery/resources)
