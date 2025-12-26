# Kubernetes Basics

Simple explanations of core Kubernetes concepts.

## What Is Kubernetes?

**Container orchestrator** - manages containers across multiple servers.

Think of it as:
- Docker → runs one container on one machine
- Kubernetes → runs many containers across many machines

**What it does:**
- Schedules containers to run on available nodes
- Restarts failed containers
- Scales up/down based on load
- Routes traffic to containers
- Manages configuration and secrets

---

## Why Kubernetes? (vs Cloud Run)

**Cloud Run:**
- Google manages everything
- Just deploy container → done
- Auto-scales (including to zero)
- Simple but less control

**Kubernetes:**
- You manage the cluster
- More configuration needed
- Doesn't scale to zero (costs more)
- Complex but full control

**Use Kubernetes when:**
- Need specific networking setup
- Running batch jobs/cron jobs
- Stateful applications (databases)
- Want cloud portability (K8s runs everywhere)
- Need very specific resource control

---

## Core Concepts (ELI5)

### 1. Cluster
**What:** Group of machines running Kubernetes

```
Cluster
├── Node 1 (server/VM)
├── Node 2 (server/VM)
└── Node 3 (server/VM)
```

**GKE = Google Kubernetes Engine** - Google manages the nodes for you

---

### 2. Node
**What:** One machine (VM) in your cluster

Each node:
- Runs containers
- Has CPU, RAM, disk
- Talks to other nodes

**Types:**
- Control plane nodes (manage cluster) - GKE handles this
- Worker nodes (run your apps) - you configure these

---

### 3. Pod
**What:** Smallest deployable unit in Kubernetes (one or more containers)

Usually: 1 pod = 1 container

```
Pod
└── Container (your app)
```

**Key point:** Pods are ephemeral (temporary)
- They die and get recreated
- Get new IP addresses
- Don't store data permanently

Like Cloud Run instances that come and go.

---

### 4. Deployment
**What:** Manages multiple identical Pods

```yaml
Deployment: "lab02-spring"
├── Pod 1 (replica)
├── Pod 2 (replica)
└── Pod 3 (replica)
```

**What it does:**
- Ensures X number of Pods are running (replicas)
- Rolling updates (deploy new version gradually)
- Rollback if something breaks
- Self-healing (restarts failed Pods)

**Similar to:** Cloud Run service (but you control replica count)

---

### 5. Service
**What:** Stable endpoint to access Pods

**Problem:** Pods have changing IPs

**Solution:** Service provides fixed IP/DNS

```
Service "lab02-service"
  └── Routes to → Pod 1, Pod 2, Pod 3
```

**Types:**
- **ClusterIP** - Internal only (default)
- **LoadBalancer** - External (like Cloud Run URL)
- **NodePort** - Exposes on node IP:port

---

### 6. Ingress
**What:** HTTP(S) load balancer routing

Routes URLs to Services:
```
https://myapp.com/api  → Service A
https://myapp.com/web  → Service B
```

Like Cloud Run routing but more flexible.

---

### 7. ConfigMap
**What:** Configuration stored separately from code

```yaml
ConfigMap:
  APP_NAME: "My App"
  REGION: "us-central1"
```

Pods read these values as environment variables.

**Similar to:** Cloud Run `--set-env-vars`

---

### 8. Secret
**What:** Sensitive data (passwords, API keys)

Like ConfigMap but encrypted at rest.

```yaml
Secret:
  DB_PASSWORD: "encrypted-value"
```

**Similar to:** Cloud Run `--set-secrets` from Secret Manager

---

### 9. Namespace
**What:** Virtual cluster within cluster

Organize resources:
```
Cluster
├── Namespace: dev
│   └── Deployment: lab02
├── Namespace: prod
│   └── Deployment: lab02
└── Namespace: default
```

Isolates resources (dev can't see prod).

---

## How It All Works Together

**Example: Deploying lab02-spring**

```
You write YAML:
  Deployment: lab02-spring (3 replicas)
  Service: lab02-service (LoadBalancer)
  ConfigMap: environment variables

You apply:
  kubectl apply -f lab02.yaml

Kubernetes does:
  1. Pulls container image
  2. Creates 3 Pods on available Nodes
  3. Creates Service with stable IP
  4. Creates LoadBalancer (external IP)
  5. Routes traffic to Pods (load balancing)
  
User accesses:
  http://EXTERNAL-IP → Service → Pod 1, 2, or 3
```

---

## Kubernetes vs Cloud Run

### Deploying a Service

**Cloud Run:**
```bash
gcloud run deploy lab02 --source .
```
Done. Google handles everything.

**Kubernetes:**
```yaml
# 1. Write deployment.yaml
# 2. Write service.yaml
# 3. Apply both
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

More work, but more control.

---

### Scaling

**Cloud Run:**
- Automatic (CPU/memory based)
- Scales to zero
- No config needed

**Kubernetes:**
```yaml
replicas: 3  # Fixed number

# Or auto-scaling:
minReplicas: 2
maxReplicas: 10
targetCPUUtilizationPercentage: 80
```

Doesn't scale to zero (minimum replicas always running).

---

### Updates

**Cloud Run:**
```bash
gcloud run deploy lab02 --source .
```

New revision created, traffic shifted automatically.

**Kubernetes:**
```bash
kubectl set image deployment/lab02 app=gcr.io/project/lab02:v2
```

Rolling update (old Pods gradually replaced).

---

## Key Differences Summary

| Aspect | Cloud Run | Kubernetes |
|--------|-----------|------------|
| **Setup** | Zero config | Create cluster, configure |
| **Scaling** | Automatic | Manual (HPA for auto) |
| **Minimum cost** | $0 (scale to zero) | ~$50-100/month (nodes always on) |
| **Networking** | Simple (one URL) | Complex (Services, Ingress) |
| **Updates** | Automatic rollout | Control rolling updates |
| **Monitoring** | Built-in | Need to configure |
| **Learning curve** | Easy | Steep |

---

## When to Use What

**Use Cloud Run when:**
- ✅ Stateless HTTP services
- ✅ Want simple deployment
- ✅ Cost-sensitive (pay per request)
- ✅ Don't need custom networking

**Use Kubernetes when:**
- ✅ Need specific orchestration
- ✅ Running batch jobs
- ✅ Complex microservices
- ✅ Want cloud portability
- ✅ Need full control

**Real talk:** Most apps don't need Kubernetes complexity. But knowing it makes you more hireable.

---

## kubectl Commands (Cheat Sheet)

```bash
# Get resources
kubectl get pods
kubectl get deployments
kubectl get services

# Describe (detailed info)
kubectl describe pod POD_NAME

# Logs
kubectl logs POD_NAME
kubectl logs -f POD_NAME  # Follow (tail -f)

# Apply configuration
kubectl apply -f file.yaml

# Delete resources
kubectl delete -f file.yaml
kubectl delete pod POD_NAME

# Execute command in pod
kubectl exec -it POD_NAME -- /bin/bash

# Port forward (local testing)
kubectl port-forward pod/POD_NAME 8080:8080
```

---

## Next Steps

Now that you understand the basics, we'll:
1. Create a GKE cluster with Terraform
2. Deploy lab02-spring to Kubernetes
3. Compare the experience with Cloud Run
4. Add more complex scenarios