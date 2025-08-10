---
title: "Session 04"
description: "Install Jenkins and Run Docker Compose with Pipeline"
date: 2025-08-02
draft: false
tags: ["jenkins", "docker", "docker-compose", "ci-cd", "devops", "pipeline"]
weight: 5
---

## Agenda
-  [Getting started kubernetes Pods](https://k8s.selfstudy.life/part02/index.html#pod)
-  [Getting started kubernetes Services](https://k8s.selfstudy.life/part03/index.html#service)
-  [Getting started kubernetes Deployments](https://k8s.selfstudy.life/part03/index.html#-deployment)
-  [Getting started kubernetes Secret and configmap](https://k8s.selfstudy.life/part04/index.html)
-  [ Managing Kubernetes using Graphical Tool Rancher ](https://k8s.selfstudy.life/part05/index.html#rancher-kubernetes-management-tool)
## 🚀 CI/CD Explained

CI/CD stands for:

- **CI** → Continuous Integration  
- **CD** → Continuous Delivery / Continuous Deployment

These are essential practices in **DevOps** that help teams deliver software **faster, more reliably, and with confidence**.

---

## 🔧 1. What is CI (Continuous Integration)?

> ✅ CI automates the process of merging and testing code **frequently**.

### Key Concepts:

- Developers push code frequently (e.g., daily)
- Code is automatically **built and tested** via pipelines
- Prevents "integration hell" at release time

### Example Activities:

- Compile/build the app
- Run unit tests
- Lint code
- Package artifacts (e.g., `.jar`, `.tar.gz`)

---

## 📦 2. What is CD?

> CD has two meanings:  
> **Continuous Delivery** or **Continuous Deployment**

Let’s break it down:

---

### ✅ 2A. Continuous Delivery

> Automates delivery of code to **a staging environment** or **manual approval gate**.

- Code is **tested and ready to deploy**
- Human approval is needed for production
- Reduces time between development and release

🧪 Ideal for teams that want **control** and **automated testing**, but **manual production releases**.

---

### 🚀 2B. Continuous Deployment

> Fully automates deployment to **production** with no manual steps.

- Every successful commit is deployed to production
- Requires robust **test automation**
- Zero manual intervention

🔁 Ideal for **high-frequency release teams** (e.g., Netflix, Amazon)

---

## 🔁 CI/CD Pipeline Example Flow

```text
Commit → CI Build → Unit Test → Lint → Integration Test → Package → CD Staging → Manual Approval → CD Production
```

In Continuous Deployment, the **Manual Approval** step is removed.

---

## 🧠 Benefits of CI/CD

| Benefit                | CI                  | CD                        |
|------------------------|---------------------|----------------------------|
| Early bug detection    | ✅                   | ✅                          |
| Faster releases        | ✅                   | ✅                          |
| Automation             | ✅ Build/Test        | ✅ Deploy/Test              |
| Developer confidence   | ✅                   | ✅                          |
| Customer satisfaction  |                     | ✅ Faster feature delivery  |

---

## 🛠️ Tools for CI/CD

| Category          | Popular Tools            |
|-------------------|--------------------------|
| CI Pipelines      | Jenkins, GitLab CI, CircleCI |
| CD Tools          | ArgoCD, Spinnaker, Flux |
| Testing           | JUnit, Pytest, Selenium |
| Containers        | Docker, Podman           |
| Orchestration     | Kubernetes, Nomad        |

---

## 🧩 Summary Table

| Term                   | Description                                      |
|------------------------|--------------------------------------------------|
| **CI**                 | Automate build and test after every commit       |
| **Continuous Delivery**| Ready to deploy with approval                    |
| **Continuous Deployment**| Automatically deploy to production            |


## 🧱 [Part 1: Install Jenkins](https://www.jenkins.io/doc/book/installing/)
## 🐳 Part 2: Install Docker & Docker Compose
### Test in Jenkins (via freestyle or pipeline job):

```groovy
pipeline {
  agent any
  stages {
    stage('Docker Test') {
      steps {
        sh 'docker version'
        sh 'docker info'
      }
    }
  }
}
```

---

## 🚀 Part 3: Sample Jenkins Pipeline to Run Docker Compose App

```groovy
pipeline {
  agent any

  environment {
    COMPOSE_PROJECT_DIR = "${WORKSPACE}/myapp"
  }

  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/nirpendra83/docker-compose.git'
      }
    }

    stage('Build and Deploy') {
      steps {
        dir('myapp') {
          sh 'docker-compose down --volumes || true'
          sh 'docker-compose up -d --build'
        }
      }
    }

    stage('Health Check') {
      steps {
        sh 'sleep 5 && curl -f http://localhost:5000'
      }
    }

    stage('Shutdown') {
      steps {
        dir('myapp') {
          sh 'docker-compose down'
        }
      }
    }
  }

  post {
    always {
      echo '✅ Pipeline complete. Clean up done.'
    }
  }
}
```
-  For Windows Jenkins agent
```groovy
pipeline {
  agent any

  environment {
    COMPOSE_PROJECT_DIR = "${WORKSPACE}/myapp"
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/nirpendra83/docker-compose.git'
      }
    }

    stage('Build and Deploy') {
      steps {
        dir('myapp') {
          bat 'docker-compose down --volumes || true'
          bat 'docker-compose up -d --build'
        }
      }
    }

    stage('Health Check') {
      steps {
        bat 'sleep 5 && curl -f http://localhost:5000'
      }
    }

    stage('Shutdown') {
      steps {
        dir('myapp') {
          bat 'docker-compose down'
        }
      }
    }
  }

  post {
    always {
      echo '✅ Pipeline complete. Clean up done.'
    }
  }
}
```
---

## 💡 Jenkins Declarative Pipeline Guide

The **Declarative Pipeline** in Jenkins is a structured way to define CI/CD workflows using a `Jenkinsfile`. It is easier to read, validate, and maintain compared to Scripted Pipelines.

---

## 🧱 1. Basic Structure

```groovy
pipeline {
  agent any
  stages {
    stage('Example') {
      steps {
        echo 'Hello World'
      }
    }
  }
}
```

---

## 🚀 2. `agent` Block

The `agent` defines **where** the pipeline or a specific stage runs.

### 🔹 Global Agent

```groovy
agent any
```

### 🔹 Label-based Agent

```groovy
agent { label 'docker-node' }
```

### 🔹 Docker Agent

```groovy
agent {
  docker {
    image 'python:3.10'
    args '-p 5000:5000'
  }
}
```

### 🔹 No Agent Globally (define per stage)

```groovy
pipeline {
  agent none
  stages {
    stage('Build') {
      agent any
      steps {
        echo "Building..."
      }
    }
  }
}
```

---

## 🌍 3. `environment` Block

Defines environment variables, globally or per stage.

```groovy
environment {
  MY_ENV = 'production'
  PATH = "/custom/bin:${env.PATH}"
}
```

---

## 🧪 4. `parameters` Block

Used for user input when starting a pipeline manually.

```groovy
parameters {
  string(name: 'VERSION', defaultValue: '1.0.0', description: 'Release version')
  booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Run tests?')
}
```

Use with: `${params.VERSION}`, `${params.RUN_TESTS}`

---

## 🧱 5. `stages` and `steps`

- **Stages** are logical divisions like "Build", "Test", "Deploy"
- **Steps** are shell commands or Jenkins actions

```groovy
stages {
  stage('Build') {
    steps {
      echo "Building version ${params.VERSION}"
      sh 'make build'
    }
  }
}
```

---

## 🧼 6. `post` Block

Defines **actions to run after the pipeline or a stage**, based on outcome.

```groovy
post {
  always {
    echo 'Always runs'
  }
  success {
    echo 'Runs on success'
  }
  failure {
    echo 'Runs on failure'
  }
  unstable {
    echo 'Runs if build is unstable'
  }
  changed {
    echo 'Runs if result changed since last run'
  }
}
```

---

## 🎯 7. `when` Block

Run a stage only if conditions are met.

```groovy
stage('Deploy') {
  when {
    branch 'main'
  }
  steps {
    sh './deploy.sh'
  }
}
```

Other options:
- `expression { return params.RUN_TESTS }`
- `environment name: 'ENV_VAR', value: 'prod'`
- `not`, `anyOf`, `allOf` for combining conditions

---

## 🔀 8. Parallel Stages

```groovy
stage('Test Suite') {
  parallel {
    stage('Unit Tests') {
      steps { sh 'pytest tests/unit' }
    }
    stage('Integration Tests') {
      steps { sh 'pytest tests/integration' }
    }
  }
}
```

---

## 🔧 9. Error Handling

### Explicit failure:

```groovy
steps {
  error("Stopping pipeline due to failure")
}
```

### Retry logic:

```groovy
steps {
  retry(3) {
    sh './sometimes-fails.sh'
  }
}
```

---

## 🧠 10. Built-in Variables

| Variable            | Description                        |
|---------------------|------------------------------------|
| `env.BUILD_ID`      | Unique ID for the current build    |
| `env.BUILD_NUMBER`  | Build number                      |
| `env.JOB_NAME`      | Name of the job                   |
| `params.<name>`     | Access input parameters            |
| `env.WORKSPACE`     | File path of the working directory |

---

## 📦 Example: Full Declarative Pipeline

```groovy
pipeline {
  agent any

  parameters {
    string(name: 'VERSION', defaultValue: '1.0.0')
  }

  environment {
    DEPLOY_ENV = 'staging'
  }

  stages {
    stage('Build') {
      steps {
        echo "Building version ${params.VERSION}"
      }
    }

    stage('Test') {
      steps {
        sh './run_tests.sh'
      }
    }

    stage('Deploy') {
      when {
        branch 'main'
      }
      steps {
        sh './deploy.sh ${params.VERSION}'
      }
    }
  }

  post {
    success {
      echo '✅ Build succeeded.'
    }
    failure {
      echo '❌ Build failed.'
    }
    always {
      echo '🧹 Cleaning up...'
    }
  }
}
```

---

> ✅ **Declarative pipelines** are YAML-like Groovy configurations that make your Jenkins workflows repeatable, testable, and easy to maintain.

