---
title: "Session 03"
description: "Getting Started with Kuberntes"
date: 2025-08-02
draft: false
tags: ["jenkins", "docker", "docker-compose", "ci-cd", "devops", "pipeline"]
weight: 5
---

## [Kubernetes Documentation](http://k8s.selfstudy.life/)

# Getting Started with Kubernetes

Kubernetes (K8s) is an open-source system for automating deployment, scaling, and management of containerized applications.

---

## 🚀 Prerequisites

- Basic knowledge of containers (e.g., Docker)
- A running Kubernetes cluster (Minikube, Kind, or a managed service like GKE, EKS, AKS)
- `kubectl` installed and configured

---

## 🛠 Install kubectl command to manage kubernetes 

```bash
# Install kubectl
curl -LO "https://dl.k8s.io/release/$(curl -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl && mv kubectl /usr/local/bin/
```

## AKS Deployment Guide

A step-by-step guide to deploy Azure Kubernetes Service (AKS) using Azure CLI.

---

## 📌 Prerequisites

- An active Azure subscription
- Azure CLI installed (`az --version` to check) 
    - [Link  for Az cli installation](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)
- Logged into Azure:
```bash
 az login --use-device-code
```
-  Check your azure subscription
```sh
az account list - table
```
- Check login status
```sh
az account show
```
-  Create a Resource Group
```sh
az group create --name myResourceGroup --location centralindia

```
- Create AKS Cluster
```sh
az aks create \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --location centralindia \
  --node-count 1 \
  --node-vm-size Standard_B2s \
  --generate-ssh-keys

```

- Register the Missing Resource Provider if get an error
```sh
az provider register --namespace Microsoft.OperationalInsights

az provider show --namespace Microsoft.OperationalInsights --query "registrationState"

```

- Register all required add on for AKS
```sh
for ns in Microsoft.ContainerService Microsoft.OperationalInsights Microsoft.Insights Microsoft.Network Microsoft.Compute Microsoft.Storage; do
  az provider register --namespace $ns
done
```
 -  Get AKS Credentials (Kubeconfig)
 ```sh
 az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```
- Verify Cluster Access
```sh
kubectl get nodes
```

## ✅ Jenkins Credential ID for AKS kubeconfig

When you upload your kubeconfig file as a **Secret file** in Jenkins, you choose an **ID** for it. Examples:

- `aks-kubeconfig`
- `kubeconfig`
- `prod-aks`
- (Any custom name you prefer)

This **ID** is used in the Jenkins pipeline to reference the kubeconfig file.

---

## ✅ Example: Upload & Use `aks-kubeconfig`

### 📌 In Jenkins:

1. Navigate to **Manage Jenkins → Credentials**
2. Choose the appropriate **scope** (e.g., Global, Folder, etc.)
3. Click **"Add Credentials"**
4. Choose **"Secret file"**
5. Upload your **`kubeconfig`** file
6. Set the **ID** to: `aks-kubeconfig`

---

### 📋 Jenkins Pipeline (Declarative) Example

```groovy
pipeline {
  agent any

  environment {
    KUBECONFIG = "${WORKSPACE}/kubeconfig"
  }

  stages {
    stage('Use AKS Kubeconfig') {
      steps {
        withCredentials([file(credentialsId: 'aks-kubeconfig', variable: 'KUBECONFIG_FILE')]) {
          sh '''
            cp $KUBECONFIG_FILE $KUBECONFIG
            chmod 600 $KUBECONFIG
            kubectl get nodes
          '''
        }
      }
    }
  }
}
```


## 🛠️ Kompose - Convert Docker Compose to Kubernetes YAML

Kompose is a tool that helps you convert your `docker-compose.yml` into Kubernetes resource manifests.

---

### ✅ What Kompose Does

Kompose automatically converts:

- Docker Compose services ➡️ Kubernetes **Deployments**
- Exposed ports ➡️ Kubernetes **Services**
- Volumes ➡️ **PersistentVolumeClaims**
- Environment variables ➡️ **ConfigMaps** or inline in Pods

---

### 📦 Install Kompose

#### 🔧 On Linux / macOS

```sh
curl -L https://github.com/kubernetes/kompose/releases/download/v1.30.0/kompose-linux-amd64 -o kompose
chmod +x kompose
sudo mv kompose /usr/local/bin/
```

## 🔄 Convert Docker Compose to Kubernetes Manifests
- Navigate to the folder where your docker-compose.yml is located:
- Run:
```sh 
kompose convert
```
- To deploy directly to your cluster:
```sh
kompose up
```
- To remove 
```sh
kompose down
```
> You may have to take care of the labels and ports assigned to svc and pods .
