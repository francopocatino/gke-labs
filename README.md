# GKE Labs

Learning Kubernetes on Google Kubernetes Engine.

## What This Is

Hands-on Kubernetes practice deploying the same apps from [gcp-labs](https://github.com/francopocatino/gcp-labs) to GKE.

**Goal:** Understand Kubernetes by comparing it to Cloud Run (serverless).

## Structure

```
gke-labs/
├── KUBERNETES-BASICS.md    # Core concepts explained
├── terraform/              # GKE cluster setup
├── k8s-manifests/          # Kubernetes YAML files
│   ├── lab02-spring/
│   ├── lab06-gcs/
│   └── lab07-pubsub/
└── helm-charts/            # Later: Helm packaging
```

## Prerequisites

- `gcloud` CLI installed and configured
- `kubectl` installed
- GCP project with billing enabled
- Completed [gcp-labs](https://github.com/francopocatino/gcp-labs) (helpful but not required)

## Quick Start

Coming soon - we'll deploy services to GKE step by step.

## Comparison: Cloud Run vs GKE

| Feature | Cloud Run | GKE (Kubernetes) |
|---------|-----------|------------------|
| **Management** | Fully managed | You manage cluster |
| **Scaling** | Automatic (to zero) | Manual config (min replicas) |
| **Cost** | Pay per request | Pay for nodes (always running) |
| **Complexity** | Low | High |
| **Control** | Limited | Full control |
| **Best for** | Stateless APIs | Complex apps, batch jobs, specific needs |

Both are great - just different trade-offs.

## Learning Path

1. Kubernetes basics
2. Deploy first app to GKE
3. Learn core resources (Pods, Deployments, Services)
4. Add ConfigMaps and Secrets
5. CI/CD to GKE
6. Compare with Cloud Run experience

---

**Status:** Just started - building as I learn.
