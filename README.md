# GKE Labs

Learning Kubernetes - starting locally with Minikube, then deploying to GKE.

## Learning Path

**Phase 1: Local (FREE)**
- Practice Kubernetes with Minikube
- Deploy apps locally
- Learn core concepts without cloud costs

**Phase 2: Cloud (Later)**
- Create GKE cluster with Terraform
- Deploy same apps to GKE
- Add production features
- Clean up to avoid costs

## Structure

```
gke-labs/
├── KUBERNETES-BASICS.md    # Core concepts
├── k8s-manifests/          # YAML files (work on both Minikube & GKE)
├── terraform/              # GKE cluster setup (use later)
└── minikube-setup.md       # Local setup guide
```

## Prerequisites

- Docker Desktop installed
- `kubectl` installed
- `minikube` installed

## Quick Start (Minikube)

```bash
# Start local cluster
minikube start

# Verify
kubectl get nodes

# Deploy first app
kubectl apply -f k8s-manifests/lab02-spring/

# Access it
minikube service lab02-spring
```

## Comparison: Minikube vs GKE

| Feature | Minikube | GKE |
|---------|----------|-----|
| **Cost** | FREE | ~$7-30/month |
| **Setup** | Local machine | Cloud cluster |
| **Kubernetes** | Same concepts | Same concepts |
| **YAML files** | Identical | Identical |
| **Use for** | Learning, dev | Production, portfolio |

---

**Current status:** Starting with Minikube. Will add GKE deployment later.
