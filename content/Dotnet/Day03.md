---
title: "Session 03"
description: "Getting Started with Kuberntes"
date: 2025-08-02
draft: false
tags: ["jenkins", "docker", "docker-compose", "ci-cd", "devops", "pipeline"]
weight: 5
---

## [Kubernetes Documentation](http://k8s.selfstudy.life/)

## Getting Started with Kubernetes

Kubernetes (K8s) is an open-source system for automating deployment, scaling, and management of containerized applications.

---

## 🚀 Docker vs Kubernetes – Quick Comparison

| Feature              | Docker                         | Kubernetes                    |
|----------------------|--------------------------------|-------------------------------|
| **Purpose**          | Build and run containers        | Container orchestration       |
| **Function**         | Packages applications           | Manages containers            |
| **Architecture**     | Runs on single nodes            | Runs on clusters              |
| **Scaling**          | Manual scaling                  | Automatic scaling             |

---

**Summary:**
- **Docker** is used for creating and running containers.
- **Kubernetes** is used for managing those containers at scale across multiple nodes.

## 🚀 Prerequisites

- Basic knowledge of containers (e.g., Docker)
- A running Kubernetes cluster (Minikube, Kind, or a managed service like GKE, EKS, AKS)
- `kubectl` installed and configured
-  Enable kuberntes from Docker Desktop to get started.

---

## 🛠 Install kubectl command to manage kubernetes 

```bash
# Install kubectl
curl -LO "https://dl.k8s.io/release/$(curl -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl && mv kubectl /usr/local/bin/
```
### For Windows [Download link](https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/)


- [ ] Enable Kubernetes on Docker Desktop
- [ ] Understand the use of kubeconfig file
- [ ] Run some basic commands 


## 🧠 Kubernetes Components (Brief Overview)

Kubernetes consists of a **control plane** and **worker nodes**, each with several key components. Below is a summary of both.

---

## 🛠️ Control Plane Components

These components manage the overall cluster state.

### 1. kube-apiserver
- The **front-end** of the Kubernetes control plane.
- Exposes the Kubernetes API.
- All internal/external communication happens through it.

### 2. etcd
- A **key-value store** for all cluster data.
- Stores configuration, state, and metadata.
- Highly available and consistent.

### 3. kube-scheduler
- Watches for new Pods without assigned nodes.
- Selects the best node to run the Pod based on constraints and policies.

### 4. kube-controller-manager
- Runs various **controllers** (e.g., ReplicationController, NodeController).
- Ensures desired state matches the actual cluster state.

### 5. cloud-controller-manager (optional)
- Manages cloud-specific resources (e.g., load balancers, IPs).
- Used only in cloud environments (e.g., AWS, Azure, GCP).

---

## 🚀 Node Components

These run on **each worker node** and are responsible for running Pods.

### 1. kubelet
- Agent that runs on every node.
- Registers node with the cluster.
- Ensures containers are running in the desired state.

### 2. kube-proxy
- Maintains network rules for Pods.
- Enables communication within the cluster using Services.
- Handles routing and load balancing.

### 3. container runtime
- Software that runs containers (e.g., containerd, CRI-O, Docker).
- Kubernetes uses it to run application containers.

---

## 🔍 Add-ons (Common but not mandatory)

### 1. CoreDNS
- Provides **DNS-based service discovery**.
- Resolves service names to IPs.

### 2. Metrics Server
- Collects resource metrics (CPU/memory).
- Enables autoscaling and monitoring.

### 3. Ingress Controller
- Manages **external HTTP/HTTPS access** to services.
- Works with Ingress resources.

---

## ✅ Summary Table

| Component                | Role                              | Runs On         |
|--------------------------|-----------------------------------|-----------------|
| kube-apiserver           | API access point                  | Control Plane   |
| etcd                     | Cluster data store                | Control Plane   |
| kube-scheduler           | Pod scheduling                    | Control Plane   |
| kube-controller-manager  | Runs controllers                  | Control Plane   |
| cloud-controller-manager | Manages cloud resources           | Control Plane   |
| kubelet                  | Node agent                        | Worker Node     |
| kube-proxy               | Network proxy                     | Worker Node     |
| container runtime        | Runs containers                   | Worker Node     |

---

> 📝 **Tip:** Run `kubectl get componentstatuses` to see health of control plane components.




## AKS Deployment Guide

Steps to deploy Azure Kubernetes Service (AKS) using Azure CLI.

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



## 📘 Kubernetes Pods Assignment

Complete the following tasks to strengthen your understanding of Pods in Kubernetes.

---

## 📝 Tasks 01

- [ ] Create a Pod named `nginx-pod` using the `nginx` image.
- [ ] Describe the `nginx-pod` and review its details.
- [ ] View logs of the `nginx-pod`.
- [ ] Execute an interactive shell inside the `nginx-pod`.
- [ ] Write a YAML manifest to create a Pod named `busybox-pod` running the `busybox` image.
- [ ] Create a Pod that runs for a long time using `sleep` command.
- [ ] Create a Pod with specific resource `requests` and `limits` for CPU and memory.
- [ ] Create a Pod that sets an environment variable `MY_ENV_VAR=HelloK8s`.
- [ ] Create a Pod that mounts a `hostPath` volume to a container.
- [ ] Delete a Pod using `kubectl delete` and confirm it's removed.

---

## 📘 Kubernetes Deployments Assignment

Work through the following tasks to gain practical experience with Deployments in Kubernetes.

---

## 📝 Tasks 02

- [ ] Create a Deployment named `web-deploy` with 2 replicas using the `nginx` image.
- [ ] Verify that the Deployment created the correct number of Pods.
- [ ] Scale the Deployment `web-deploy` to 5 replicas.
- [ ] Update the Deployment image to `nginx:1.21`.
- [ ] Roll back the Deployment to the previous version.
- [ ] Write a YAML manifest to create a Deployment named `httpd-deploy` using the `httpd` image.
- [ ] Add a label `env=prod` to all Pods created by `web-deploy`.
- [ ] Use `kubectl rollout status` to monitor the status of a rolling update.
- [ ] Delete the Deployment and verify that all Pods are also deleted.


---






## 📘 Kubernetes Services Assignment

Complete the following tasks to understand and work with Services in Kubernetes.

---

## 📝 Tasks 03

- [ ] Create a Deployment named `nginx-deploy` with 3 replicas using the `nginx` image.
- [ ] Expose the Deployment with a ClusterIP Service named `nginx-clusterip` on port 80.
- [ ] Verify that the Service correctly routes traffic to all 3 Pods.
- [ ] Use `kubectl get endpoints` to confirm endpoints are assigned to the Service.
- [ ] Create a NodePort Service named `nginx-nodeport` to expose the same Deployment on a node port.
- [ ] Access the application in a browser using `<NodeIP>:<NodePort>`.
- [ ] Create a YAML file to define a LoadBalancer Service (only works in cloud environments).
- [ ] Delete the `nginx-clusterip` and `nginx-nodeport` services.
- [ ] Create a headless Service using `clusterIP: None` and verify DNS resolution inside a Pod.
- [ ] Use `kubectl port-forward` to expose a Pod without a Service and access it locally.

---



