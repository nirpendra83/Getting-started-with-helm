+++
title = "Helm Components"
weight = 5
+++

- **Helm Chart**
-  **Helm Repository**  
-  **Helm CLI**  
-  **Helm Release** 

---

## 🧱 Helm Charts

A **Helm chart** is a packaged collection of Kubernetes resource definitions required to deploy applications. Think of it like a `.deb` or `.rpm` package in Linux — reusable, shareable, and version-controlled.

### 📦 Chart Structure

```bash
mychart/
├── Chart.yaml        # Metadata about the chart (name, version, description)
├── values.yaml       # Default config values for templates
├── charts/           # Optional: subcharts (dependencies)
├── templates/        # Kubernetes manifest templates
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── _helpers.tpl  # Reusable template snippets
│   └── ...
└── README.md         # Optional: chart documentation
```

### 📄 Key Files

- **Chart.yaml** – Metadata about the chart (name, version, description)
- **values.yaml** – Default configuration values; can be overridden at install/upgrade
- **templates/** – Kubernetes manifests rendered with Go templating
- **charts/** – Subcharts or dependencies
- **README.md** – Optional documentation

### 🛠️ Create a New Chart

```bash
helm create myapp
```

This generates a scaffolded chart directory under `myapp/`.

### 🚀 Install a Chart

```bash
helm install my-nginx bitnami/nginx
```

---

## 📚 Helm Repositories

A **Helm repository** is a remote or local source that hosts one or more Helm charts, similar to APT/YUM repos in Linux.

### ➕ Add a Repository

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
```

### 📋 List Configured Repositories

```bash
helm repo list
```

### 🔄 Update Repository Cache

```bash
helm repo update
```

Repositories enable you to distribute and manage versioned charts in a structured manner.

---

## 🚢 Helm Releases

A **Helm release** is a deployed instance of a chart in your Kubernetes cluster. Each release is uniquely named and tracks the chart version, configuration values, and revision history.

### 🚀 Install / Create a Release

```bash
helm install myapp ./myapp
```

### 📜 List All Releases

```bash
helm list
```

### 🔧 Upgrade a Release

```bash
helm upgrade myapp ./myapp
```

### ⏪ Roll Back to a Previous Revision

```bash
helm rollback myapp 1
```

### ❌ Delete a Release

```bash
helm uninstall myapp
```

Releases provide full control over versioning, rollback, and lifecycle management of your Kubernetes apps.

---

## 🧰 Common Helm CLI Commands

```bash
# Add a repository
helm repo add <name> <url>

# Update repository cache
helm repo update

# Search for a chart
helm search repo <keyword>

# Install a chart
helm install <release-name> <chart-name>

# List installed releases
helm list

# Upgrade a release
helm upgrade <release-name> <chart-name>

# Roll back a release
helm rollback <release-name> <revision-number>

# Uninstall a release
helm uninstall <release-name>
```

---

## 🧠 Summary

- **Helm Charts**: Packaged applications with templated Kubernetes manifests.
- **Helm Repositories**: Storage locations for Helm charts (public or private).
- **Helm Releases**: Deployed and versioned instances of a chart within a cluster.
- **Helm CLI**: Command-line interface for managing charts, releases, and repos.

Helm streamlines application deployment on Kubernetes by bringing structure, reusability, and lifecycle control to complex workloads.
