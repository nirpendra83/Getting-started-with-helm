
---
title: "Jenkins"
description: "CICD with Jenkins"
date: 2025-08-03
draft: false
tags: ["Jenkins ", "angular", "postgresql", "full-stack", "starter-kit", "devops"]
weight: 8
---



## Jenkins — CI/CD 

Jenkins is an open-source automation server for building, testing, and deploying software.  

The leading open source automation server, Jenkins provides hundreds of plugins to support building, deploying and automating any project.

- [ Jenkins Official Documentation ](https://www.jenkins.io/doc/)
---

## 1. Installation

## [Jenkins Installation Documentation](https://www.jenkins.io/doc/book/installing/)

### Docker
```bash
docker run -d --name jenkins \
  -p 8080:8080 -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  jenkins/jenkins:lts
```

---

## 2. Initial Setup
1. Access Jenkins: `http://<host>:8080`
2. Unlock:
```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
3. Install suggested plugins
4. Create admin user
5. Enable security: Role Strategy Plugin, CSRF protection
6. Disable anonymous read access

---
## Jenkins — Quick Start 

Jenkins is an open-source automation server for building, testing, and deploying software.  
This quick start shows you how to get Jenkins running and create your first pipeline.

---

## [enkins Pipeline Official Documentation](https://www.jenkins.io/doc/book/pipeline/)

## 1. First Pipeline (Hello World)

Once Jenkins is installed and set up, you can create a simple pipeline:

1. In Jenkins, click **New Item** → **Pipeline** → Name it (e.g., `hello-pipeline`) → **OK**.
2. Scroll to **Pipeline** section and paste this:

```groovy
pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Building...'
      }
    }
    stage('Test') {
      steps {
        echo 'Testing...'
      }
    }
    stage('Deploy') {
      steps {
        echo 'Deploying...'
      }
    }
  }
}
```

3. Save and click **Build Now** — watch Jenkins execute the stages.

---

## 10 Jenkins Declarative Pipeline Examples (Windows-Friendly)

---

## 1. Hello World (Basic)
```groovy
pipeline {
  agent any
  stages {
    stage('Hello') {
      steps {
        echo 'Hello World'
      }
    }
  }
}
```

---

## 2. Multiple Stages
```groovy
pipeline {
  agent any
  stages {
    stage('Build') {
      steps { echo 'Building...' }
    }
    stage('Test') {
      steps { echo 'Testing...' }
    }
    stage('Deploy') {
      steps { echo 'Deploying...' }
    }
  }
}
```

---

## 3. Run Windows Commands
```groovy
pipeline {
  agent any
  stages {
    stage('List Files') {
      steps {
        bat 'dir'
      }
    }
  }
}
```

---

## 4. Using Environment Variables
```groovy
pipeline {
  agent any
  environment {
    APP_NAME = 'my-app'
  }
  stages {
    stage('Show Env') {
      steps {
        echo "App Name: ${env.APP_NAME}"
      }
    }
  }
}
```

---

## 5. Checkout from Git
```groovy
pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/example/repo.git'
      }
    }
  }
}
```

---

## 6. Run Tests & Publish Report
```groovy
pipeline {
  agent any
  stages {
    stage('Test') {
      steps {
        bat 'pytest --junitxml=reports\\results.xml'
        junit 'reports/results.xml'
      }
    }
  }
}
```

---

## 7. Use Credentials for Private Repo
```groovy
pipeline {
  agent any
  environment {
    GIT_CREDS = credentials('git-creds')
  }
  stages {
    stage('Checkout') {
      steps {
        git url: "https://${GIT_CREDS_USR}:${GIT_CREDS_PSW}@github.com/org/private-repo.git", branch: 'main'
      }
    }
  }
}
```

---

## 8. Build & Push Docker Image (Windows with Docker Desktop)
```groovy
pipeline {
  agent any
  environment {
    IMAGE_NAME = "myorg/app"
  }
  stages {
    stage('Build & Push') {
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-creds') {
            def img = docker.build("${IMAGE_NAME}:${env.BUILD_NUMBER}")
            img.push()
          }
        }
      }
    }
  }
}
```

---

## 9. Conditional Stage Execution
```groovy
pipeline {
  agent any
  stages {
    stage('Deploy to Prod') {
      when { branch 'main' }
      steps {
        echo 'Deploying to Production...'
      }
    }
  }
}
```

---

## 10. Full CI/CD Example (Windows Commands)
```groovy
pipeline {
  agent any
  environment {
    IMAGE = "myorg/app:${env.BUILD_NUMBER}"
  }
  stages {
    stage('Checkout') {
      steps { checkout scm }
    }
    stage('Build') {
      steps { bat 'make build' }
    }
    stage('Test') {
      steps {
        bat 'make test'
        junit 'reports/**/*.xml'
      }
    }
    stage('Docker Build & Push') {
      when { branch 'main' }
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-creds') {
            def img = docker.build(env.IMAGE)
            img.push()
          }
        }
      }
    }
    stage('Deploy') {
      when { branch 'main' }
      steps {
        bat 'deploy.bat'
      }
    }
  }
  post {
    always {
      archiveArtifacts artifacts: 'dist/**', fingerprint: true
    }
  }
}
```
