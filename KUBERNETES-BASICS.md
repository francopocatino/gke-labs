# Kubernetes Notes

Personal reference for understanding Kubernetes concepts.

## What Kubernetes Does

Container orchestrator - manages containers across multiple machines. Handles scheduling, scaling, restarts, networking, and config management.

---

## Core Concepts

### Cluster
Group of machines (nodes) running Kubernetes. GKE = Google manages the cluster infrastructure.

### Node
One VM in the cluster. Runs containers, has CPU/RAM/disk. Types:
- Control plane (manages cluster) - GKE handles
- Worker nodes (run apps) - I configure

### Pod
Smallest unit in K8s. Usually 1 pod = 1 container. Pods are temporary - they die and get recreated with new IPs.

### Deployment
Manages multiple identical pods (replicas). Handles:
- Keeping N pods running
- Rolling updates
- Rollbacks
- Self-healing

### Service
Stable endpoint to access pods. Provides fixed IP/DNS since pod IPs change.

Types:
- ClusterIP (internal only)
- LoadBalancer (external, like Cloud Run URL)
- NodePort (exposes on node IP:port)

### Ingress
HTTP(S) load balancer. Routes URLs to services.

### ConfigMap
Config data (env vars, files). Non-sensitive.

### Secret
Sensitive data (passwords, keys). Encrypted at rest.

### Namespace
Virtual clusters within a cluster. Isolate resources (dev/staging/prod).

---

## Cloud Run vs Kubernetes

| Cloud Run | Kubernetes |
|-----------|-----------|
| Zero config | Create cluster, configure everything |
| Auto-scales (to zero) | Manual scaling (min replicas > 0) |
| $0 when idle | ~$50-100/month minimum |
| Simple (one URL) | Complex (services, ingress, networking) |
| Limited control | Full control |

**When to use K8s:** Need specific orchestration, batch jobs, stateful apps, cloud portability, or hiring requirements.

---

## Deployment Flow

```
Write YAML → kubectl apply → K8s creates pods → Service routes traffic
```

vs Cloud Run:
```
gcloud run deploy → done
```

---

## Common Commands

```bash
# View resources
kubectl get pods
kubectl get deployments
kubectl get services
kubectl get all

# Details
kubectl describe pod POD_NAME

# Logs
kubectl logs POD_NAME
kubectl logs -f POD_NAME

# Apply config
kubectl apply -f file.yaml

# Delete
kubectl delete -f file.yaml

# Shell into pod
kubectl exec -it POD_NAME -- /bin/bash

# Port forward (local testing)
kubectl port-forward pod/POD_NAME 8080:8080

# Restart deployment
kubectl rollout restart deployment/NAME
```

---

## YAML Structure (Deployment)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app-name
spec:
  replicas: 3
  selector:
    matchLabels:
      app: app-name
  template:
    metadata:
      labels:
        app: app-name
    spec:
      containers:
      - name: app
        image: gcr.io/project/image:tag
        ports:
        - containerPort: 8080
        env:
        - name: VAR_NAME
          value: "value"
```

---

## YAML Structure (Service)

```yaml
apiVersion: v1
kind: Service
metadata:
  name: app-service
spec:
  type: LoadBalancer
  selector:
    app: app-name
  ports:
  - port: 80
    targetPort: 8080
```

---

## Key Differences from Cloud Run

**Scaling:**
- Cloud Run: automatic, to zero
- K8s: set replicas, doesn't scale to zero

**Updates:**
- Cloud Run: new revision, automatic rollout
- K8s: rolling update, manual control

**Networking:**
- Cloud Run: one URL per service
- K8s: service + ingress configuration

**Cost:**
- Cloud Run: pay per request
- K8s: pay for nodes (always on)

---

## Minikube Commands

```bash
# Start/stop
minikube start
minikube stop
minikube delete

# Dashboard
minikube dashboard

# Access service
minikube service SERVICE_NAME

# SSH into node
minikube ssh

# Give more resources
minikube start --cpus=4 --memory=4096
```

---

## What I'm Learning

1. Start with Minikube (local, free)
2. Deploy apps from gcp-labs
3. Understand K8s concepts hands-on
4. Later: move to GKE (optional)

Same apps, same containers, different orchestration.
