+++
title = "Session 1: Introduction to Helm"
weight = 1
+++

## 📦 Session 1: Introduction to Helm

Helm is the **package manager for Kubernetes**, designed to simplify deployment, configuration, and lifecycle management of Kubernetes applications using reusable packages called **charts**.

---

## ⚙️ Prerequisites

Before using Helm, ensure the following:

- ✅ A working **Kubernetes cluster**
- ✅ `kubectl` configured to access the cluster
- ✅ Helm CLI installed on your local system

### 🔧 Install Helm

```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

### 🧩 Verify Helm Installation

```bash
helm version
```

---

## 🌐 How Helm Connects to Kubernetes

Helm does **not need a server component** (like Tiller in v2). It uses:

- Your existing `kubeconfig` file (`~/.kube/config`)
- Communicates directly with Kubernetes API server
- Uses standard Kubernetes resources: `Deployments`, `Services`, `Secrets`, etc.
- Releases are tracked in your cluster (by default stored as Kubernetes Secrets)

> In essence, Helm is just another **Kubernetes client**, like `kubectl`, but focused on packages.

---

## 🚀 Getting Started: Using Existing Helm Charts

The fastest way to start with Helm is by **installing applications using public charts**.

### 1️⃣ Add a Chart Repository

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
```

### 2️⃣ Update the Repository Cache

```bash
helm repo update
```

### 3️⃣ Search for a Chart

```bash
helm search repo nginx
```

### 4️⃣ Install a Chart

```bash
helm install my-nginx bitnami/nginx
```

This command installs the **nginx chart** from the Bitnami repo as a release named `my-nginx`.

### 5️⃣ Verify the Installation

```bash
helm list
kubectl get all
```

---

## 🧰 Helm Is Like APT/YUM for Kubernetes

| Platform   | Install Method        | Example                     |
|------------|------------------------|-----------------------------|
| Linux      | `apt install nginx`    | Installs NGINX on system    |
| Kubernetes | `helm install nginx`   | Installs NGINX in cluster   |

---

## 🧱 Helm Components Overview

| Component      | Description                                                                 |
|----------------|-----------------------------------------------------------------------------|
| **Chart**      | Packaged application with Kubernetes templates                             |
| **Repository** | Collection of charts (public or private)                                   |
| **Release**    | Installed instance of a chart in your cluster                              |
| **Helm CLI**   | Interface to install, upgrade, uninstall, or inspect charts                |

---

## 📦 Helm Chart Anatomy

A **Helm chart** is a structured directory containing:

```bash
mychart/
├── Chart.yaml        # Chart metadata
├── values.yaml       # Default config values
├── templates/        # Kubernetes YAML templates
├── charts/           # Subcharts (optional)
└── README.md         # Documentation (optional)
```

### 🛠️ Create a Chart

```bash
helm create myapp
```

---

## 🚢 Helm Repositories

Helm repositories host charts and can be added easily:

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm repo list
```

---

## 🚀 Helm Releases

A **release** is a deployed instance of a chart.

### Common Release Commands:

```bash
# Install
helm install my-nginx bitnami/nginx

# List
helm list

# Upgrade
helm upgrade my-nginx bitnami/nginx

# Roll back
helm rollback my-nginx 1

# Uninstall
helm uninstall my-nginx
```

---

## 🔧 Customizing Installations

### Using `--set` (inline):

```bash
helm install my-nginx bitnami/nginx \
  --set service.type=NodePort \
  --set replicaCount=2
```

### Using `--values` (file):

```bash
helm install my-nginx bitnami/nginx -f custom-values.yaml
```

### Combine both:

```bash
helm install my-nginx bitnami/nginx \
  -f base.yaml \
  --set service.type=LoadBalancer
```

---

## 🔍 Preview and Debug Charts

### Preview manifests without installing:

```bash
helm template my-nginx bitnami/nginx
```

### Dry-run install:

```bash
helm install my-nginx bitnami/nginx --dry-run --debug
```

---

## 📥 Get and Customize Chart Values

### Show default values:

```bash
helm show values bitnami/nginx
```

### Save for customization:

```bash
helm show values bitnami/nginx > custom-values.yaml
```

Then install with:

```bash
helm install my-nginx bitnami/nginx -f custom-values.yaml
```

---

## 📄 Get Values from a Running Release

```bash
helm get values my-nginx -n default --all
```

---

## 🧠 Helm v2 vs v3

| Feature                        | Helm v2                              | Helm v3                                 |
|-------------------------------|--------------------------------------|------------------------------------------|
| Tiller Component              | ✅ Required                          | ❌ Removed                               |
| Security (RBAC)               | Complex                              | Simplified                              |
| CRD Support                   | Via Hooks                            | Native Support                          |
| Release Namespacing           | Global                               | Scoped to Namespace                     |
| Chart Repositories            | Helm Hub only                        | Helm Hub + OCI support                  |
| Release Storage               | ConfigMaps                           | Kubernetes Secrets                      |
| Upgrade Strategy              | Two-step (client + Tiller)           | Single-step (client-only)              |

> Helm v3 is recommended. Use the [2to3 plugin](https://github.com/helm/helm-2to3) to migrate from Helm v2.

---

## 🧰 Common Helm CLI Reference

```bash
# Add repo
helm repo add <name> <url>

# Update repos
helm repo update

# Search
helm search repo <keyword>

# Install
helm install <release-name> <chart>

# Upgrade
helm upgrade <release-name> <chart>

# Rollback
helm rollback <release-name> <revision>

# Uninstall
helm uninstall <release-name>

# Get installed values
helm get values <release-name> -n <namespace> --all

# Show default values
helm show values <chart-name>
```

---

## ✅ Summary

- Helm simplifies app deployment on Kubernetes via reusable charts.
- Works with your kubeconfig—no need for server-side components.
- Helm v3 is modern, secure, and CI/CD-friendly.
- Start by using existing charts, then build and customize your own.
- Mastering Helm is essential for Kubernetes-based DevOps and GitOps workflows.

---
