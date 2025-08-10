---
title: "Session 05"
description: "Production-ready starter with Angular, .NET 9, PostgreSQL, and Clean Architecture principles for scalable web apps."
date: 2025-08-03
draft: false
tags: ["clean architecture", "dotnet", "angular", "postgresql", "full-stack", "starter-kit", "devops"]
weight: 6
---
 
## [Project Link](https://github.com/nirpendra83/clean-architecture-docker-dotnet-angular.git)
## ✨ What is this?

A **production-ready full-stack starter kit** built using modern technologies and best practices:

- **Frontend**: Angular 20 with Signals, Material Design, and TailwindCSS  
- **Backend**: .NET 9 API implementing Clean Architecture  
- **Database**: PostgreSQL 16 using Dapper ORM  
- **DevOps**: Docker, GitHub Actions, and NGINX  

> Perfect for developers who want to focus on **business logic** instead of spending time setting up infrastructure.

---

## 🏗️ Why Clean Architecture?

Clean Architecture helps in creating maintainable, testable, and scalable applications by separating concerns and enforcing clear boundaries between layers.

### Benefits:
- Independent of frameworks and UI
- Emphasizes testability and loose coupling
- Simplifies onboarding and feature extension
- Ensures long-term maintainability

---

## 🚀 Application Demo

A **Contact Management Application** with:

- Role-Based Access Control (RBAC)
- Secure API endpoints
- Modular and extensible design

---

## 🔧 Technologies Used

| Layer       | Stack                                      |
|-------------|---------------------------------------------|
| Frontend    | Angular 20, Signals, TailwindCSS, Material |
| Backend     | .NET 9, Clean Architecture, Dapper ORM      |
| Database    | PostgreSQL 16                               |
| DevOps      | Docker, GitHub Actions, NGINX               |

---

## 🚀 Getting Started to deploy to container using docker compose

### 🔃 Clone the Repository

```bash
git clone https://github.com/nirpendra83/clean-architecture-docker-dotnet-angular.git clean-app
cd clean-app
```
- Create .env File
```bash
cp .env.example .env
```
-  Run the below command
```sh
docker-compose up
docker-compose up -d
```

## <span style="color:green">🚀 Deploy the Application to Kubernetes</span>

### <span style="color:green">1. Clone the Repository</span>

```sh
git clone https://github.com/nirpendra83/clean-architecture-docker-dotnet-angular.git clean-app
cd clean-app
```
<span style="color:green">2. Navigate to the Kubernetes Deployment Directory and Apply Resources</span>
```sh
cd kubernetes
kubectl apply -f .
```
<span style="color:green">3. Verify the Deployment</span>
```sh
kubectl get pods
```





## Jenkins Declarative Pipeline for Deploying to Kubernetes

This pipeline performs the following:
- Clones the Git repository (main branch)
- Navigates to the `kubernetes` folder
- Applies all Kubernetes YAML manifests using `kubectl apply -f .`
- Uses a Jenkins secret file credential for kubeconfig (`kubeconfig-cred-id`)

### Jenkinsfile (Declarative Pipeline)

```groovy
pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/nirpendra83/clean-architecture-docker-dotnet-angular.git'
        BRANCH = 'main'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: "${BRANCH}", url: "${REPO_URL}"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([file(credentialsId: 'kubeconfig-cred-id', variable: 'KUBECONFIG')]) {
                    dir('kubernetes') {
                        sh 'kubectl apply -f .'
                    }
                }
            }
        }
    }
}
```

## Configuring Jenkins to Use kubeconfig as a Secret File Credential

To enable your Jenkins pipeline to authenticate with the Kubernetes cluster, follow these steps to store your kubeconfig file securely and use it in your pipeline.

### Step 1: Add kubeconfig as a Secret File in Jenkins

1. Go to **Jenkins Dashboard** → **Manage Jenkins** → **Manage Credentials**
2. Select the appropriate scope (e.g., `(global)` or a specific folder)
3. Click **Add Credentials**
4. Choose **Kind**: `Secret file`
5. Upload your `kubeconfig` file
6. **ID**: Set it as `kubeconfig-cred-id` (or use a custom ID and update the pipeline accordingly)
7. Optionally, add a description
8. Click **OK** to save

---

### Step 2: Use the kubeconfig Secret File in Your Jenkins Pipeline

In your `Jenkinsfile`, reference the kubeconfig credential using the `withCredentials` block:

```groovy
withCredentials([file(credentialsId: 'kubeconfig-cred-id', variable: 'KUBECONFIG')]) {
    sh 'kubectl get pods'
}
```



## GitHub Hook Trigger for GITScm Polling 
### 1️⃣ Install Required Plugins
- In Jenkins, go to **Manage Jenkins → Plugins → Available Plugins**
- Install:
  - **Git**
  - **GitHub Integration** (or **GitHub plugin**)
  - **GitHub Branch Source** (optional but useful for multibranch)

---

### 2️⃣ Configure GitHub in Jenkins
1. Go to **Manage Jenkins → Configure System**
2. Find **GitHub** section.
3. Add a new GitHub server:
   - **Name**: `GitHub`
   - **Credentials**: Add your GitHub Personal Access Token (PAT)  
     _(Permissions: `repo`, `admin:repo_hook`)_
   - Click **Test Connection** to ensure connectivity.

---

### 3️⃣ Configure the Jenkins Job
1. Create or edit your Jenkins pipeline/freestyle job.
2. In **Source Code Management**, select **Git** and:
   - Enter repository URL.
   - Add credentials (if private repo).
3. Under **Build Triggers**, check:
   - ✅ **GitHub hook trigger for GITScm polling**

---

### 4️⃣ Set Up GitHub Webhook
1. Open your GitHub repository in the browser.
2. Go to **Settings → Webhooks → Add webhook**.
3. **Payload URL**:
   ```
   http://<jenkins-server>/github-webhook/
   ```
   Example:
   ```
   http://jenkins.example.com/github-webhook/
   ```
4. **Content type**: `application/json`
5. Select **Just the push event** (or required events).
6. Save the webhook.

---

### 5️⃣ Test the Integration
- Push a commit to the GitHub repository.
- Jenkins should start the job automatically without manual polling.

---

### 📄 Sample `Jenkinsfile` for Testing (Windows-Friendly)

```groovy
pipeline {
    agent any
    triggers {
        githubPush()
    }
    stages {
        stage('Checkout') {
            steps {
                echo "Checking out code..."
                checkout scm
            }
        }
        stage('Build') {
            steps {
                bat 'echo Building the project...'
            }
        }
        stage('Test') {
            steps {
                bat 'echo Running tests...'
            }
        }
        stage('Deploy') {
            steps {
                bat 'echo Deploying application...'
            }
        }
    }
    post {
        always {
            bat 'echo Pipeline finished!'
        }
    }
}
```
