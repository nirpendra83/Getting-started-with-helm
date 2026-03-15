---
title: "Jenkins Pipeline steps"
date: 2025-08-01
description: "Comprehensive guide to installing, configuring, and integrating SonarQube for continuous code quality inspection"
---



## 🛠️ Prerequisites
- Ensure you have Docker, Jenkins (admin), SonarQube (Community or custom), Git, and required ports (9000) open.
### Details
-  Analyze code quality for different branches using SonarQube Community Edition:
-  Detect bugs, code smells, security issues per branch
-  Ensure quality before merging
-  Generate reports for developers/reviewers

## 🚀 Step 1: Docker & SonarQube
```sh
docker pull sonarqube:community
docker run -d --name sonarqube -p 9000:9000 sonarqube:community
```
- Access: http://localhost:9000, install plugins if needed

## 🔧 Step 2: Jenkins Plugins
- Install via Jenkins: SonarQube Scanner & Eclipse Temurin Installer
- `Path`: Manage Jenkins → Manage Plugins → Available

## ⚙️ Step 3: Configure Tools in Jenkins
- JDK: Manage Jenkins → Global Tool Configuration → JDK → Add Eclipse Temurin
- SonarQube Scanner: Manage Jenkins → Global Tool Configuration → SonarQube Scanner → Add scanner

## 🔑 Step 4: Generate SonarQube Token
- SonarQube → My Account → Security → Generate Token → Copy for Jenkins

## 🌐 Step 5: Configure SonarQube Server in Jenkins
- Manage Jenkins → Configure System → SonarQube Servers → Add:
```sh
 Name: SonarQube
 URL: http://<sonarqube-server>:9000
 Token: Paste token
 Test connection
```

### 📜 Step 6: Jenkins Pipeline
```sh
pipeline {
    agent any
    tools {
        maven 'maven3'
        jdk 'jdk17'
    }
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }
    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'dev', url: 'https://github.com/Ank911007/FullStack-Blogging-App-.git'
            }
        }
        stage('Compile') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'mvn compile'
                    } else {
                        bat 'mvn compile'
                    }
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'mvn test'
                    } else {
                        bat 'mvn test'
                    }
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar-server') {
                    script {
                        if (isUnix()) {
                            sh "${SCANNER_HOME}/bin/sonar-scanner -Dsonar.projectName=bloggingApp -Dsonar.projectKey=bloggingApp -Dsonar.java.binaries=target -Dsonar.branch.name=dev"
                        } else {
                            bat "\"%SCANNER_HOME%\\bin\\sonar-scanner.bat\" -Dsonar.projectName=bloggingApp -Dsonar.projectKey=bloggingApp -Dsonar.java.binaries=target -Dsonar.branch.name=dev"
                        }
                    }
                }
            }
        }
        stage('Quality Gate Check') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: false
                }
            }
        }
    }
}
```

## 🏗️ Step 7: Run Job & Review
- Save and run Jenkins job
### Access SonarQube dashboard: check bugs, code smells, vulnerabilities, branch reports



