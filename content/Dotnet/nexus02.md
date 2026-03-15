---
title: "nexus-docker-setup"
date: 2025-08-25
description: "Complete guide to install Nexus on Docker and use it for Maven and Docker artifact management with CI/CD integration"
---

## Nexus Repository Manager on Docker: Installation and Usage with Maven, Docker, and CI/CD

## Prerequisites
Docker installed, optional Docker Compose, Maven installed, Docker client installed.

## Pull and Run Nexus Docker Image
docker pull sonatype/nexus3:latest  
docker run -d -p 8081:8081 --name nexus -v nexus-data:/nexus-data sonatype/nexus3:latest  
docker logs -f nexus

## Access Nexus UI
Navigate to http://localhost:8081  
Default admin credentials: username `admin`, password from:  
docker exec nexus cat /nexus-data/admin.password

## Create Repositories
Maven: Maven (hosted) for your own artifacts, Maven (proxy) to cache external dependencies  
Docker: Docker (hosted) to push images, Docker (proxy) to cache Docker Hub images

## Configure Maven to Use Nexus
Edit ~/.m2/settings.xml:
<settings>
  <mirrors>
    <mirror>
      <id>nexus</id>
      <mirrorOf>*</mirrorOf>
      <url>http://localhost:8081/repository/maven-public/</url>
    </mirror>
  </mirrors>
  <servers>
    <server>
      <id>nexus-releases</id>
      <username>admin</username>
      <password>YOUR_PASSWORD</password>
    </server>
    <server>
      <id>nexus-snapshots</id>
      <username>admin</username>
      <password>YOUR_PASSWORD</password>
    </server>
  </servers>
</settings>

Deploy Maven artifacts:  
mvn deploy -DaltDeploymentRepository=nexus-releases::default::http://localhost:8081/repository/maven-releases/

## Configure Docker to Use Nexus
docker login localhost:5000  
docker tag my-app:1.0 localhost:5000/my-app:1.0  
docker push localhost:5000/my-app:1.0  
docker pull localhost:5000/my-app:1.0

## Integration with CI/CD

### Maven Artifacts Deployment
**Jenkins Pipeline Example:**
```yaml
pipeline {
    agent any
    tools {
        maven 'maven3'
        jdk 'jdk17'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/your-repo/project.git'
            }
        }
        stage('Build & Test') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Deploy to Nexus') {
            steps {
                sh 'mvn deploy -DaltDeploymentRepository=nexus-releases::default::http://localhost:8081/repository/maven-releases/'
            }
        }
    }
}

```
**GitLab CI Example (`.gitlab-ci.yml`):**
```yaml
stages:
  - build
  - deploy

build_job:
  stage: build
  image: maven:3.9-jdk-17
  script:
    - mvn clean install

deploy_job:
  stage: deploy
  image: maven:3.9-jdk-17
  script:
    - mvn deploy -DaltDeploymentRepository=nexus-releases::default::http://localhost:8081/repository/maven-releases/
  only:
    - main
```
### Docker Images Deployment
**Jenkins Pipeline Example:**
```sh
pipeline {
    agent any
    environment {
        DOCKER_REGISTRY = 'localhost:5000'
    }
    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-app:1.0 .'
            }
        }
        stage('Push to Nexus') {
            steps {
                sh 'docker tag my-app:1.0 $DOCKER_REGISTRY/my-app:1.0'
                sh 'docker login $DOCKER_REGISTRY -u admin -p YOUR_PASSWORD'
                sh 'docker push $DOCKER_REGISTRY/my-app:1.0'
            }
        }
    }
}
```
**GitLab CI Example (`.gitlab-ci.yml`):**
```sh
stages:
  - docker-build
  - docker-push

docker_build:
  stage: docker-build
  image: docker:24.0.2
  services:
    - docker:dind
  script:
    - docker build -t my-app:1.0 .

docker_push:
  stage: docker-push
  image: docker:24.0.2
  services:
    - docker:dind
  script:
    - docker login localhost:5000 -u admin -p YOUR_PASSWORD
    - docker tag my-app:1.0 localhost:5000/my-app:1.0
    - docker push localhost:5000/my-app:1.0
  only:
    - main
```
### Key Points
- Store Nexus credentials securely in Jenkins credentials or GitLab CI variables.  
- Use version tags for Docker images to avoid overwriting previous builds.  
- CI/CD pipelines centralize and version all artifacts and container images for consistent deployments.

## Notes
- Use persistent volume `-v nexus-data:/nexus-data` to avoid data loss.  
- Open ports: 8081 for Nexus UI, 5000 for Docker registry.  
- Enable HTTPS in production for security.
