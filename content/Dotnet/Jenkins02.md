
---
title: "Jenkins 02"
description: "CICD with Jenkins"
date: 2025-08-03
draft: false
tags: ["Jenkins ", "angular", "postgresql", "full-stack", "starter-kit", "devops"]
weight: 8
---



- Docker pipeline plugins has to be there 
- Configure Docker as an agent
- Test docker intergration
```sh
pipeline {
  agent {
    docker { image 'node:16-alpine' }
  }
  stages {
    stage('Test') {
      steps {
        sh 'node --version'
      }
    }
  }
}
```

-  Multi Agent pipeline 
```sh
pipeline {
  agent none
  stages {
    stage('Back-end') {
      agent {
        docker { image 'maven:3.8.1-adoptopenjdk-11' }
      }
      steps {
        sh 'mvn --version'
      }
    }
    stage('Front-end') {
      agent {
        docker { image 'node:16-alpine' }
      }
      steps {
        sh 'node --version'
      }
    }
  }
}
```