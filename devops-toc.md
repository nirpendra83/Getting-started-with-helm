# 🚀 DevOps Engineer Syllabus

**Tech Stack:** Java, Maven, Tomcat, GitLab, Jenkins, Nexus, SonarQube, Docker, Kubernetes, Ansible, Terraform, Vagrant, GCP  

---

## Phase 1 – Linux, Networking & Cloud Basics

### Topics
- Linux filesystem & permissions  
- User & process management  
- SSH, SCP, rsync  
- Systemd & journald  
- Firewall, ports, DNS  
- VM vs Containers  
- Public vs Private Cloud  
- GCP compute basics  

### Hands-on
- Create a VM in GCP  
- Setup SSH key-based login  
- Configure firewall rules  

---

## Phase 2 – Git & GitLab

### Git
- git init, clone, commit, push  
- Branching & merging  
- Tags & releases  

### GitLab
- Repositories  
- Merge Requests  
- GitLab CI/CD  
- Runners  
- Variables & secrets  

### Hands-on
- Create GitLab repo  
- Push Java code  
- Configure GitLab Runner  

---

## Phase 3 – Java, Maven & Tomcat

### Java
- JDK & JVM  
- Web applications  
- WAR packaging  

### Maven
- pom.xml  
- Dependencies  
- Build lifecycle  
- Packaging  

### Tomcat
- Install Tomcat  
- Deploy WAR  
- Configure ports & context  

### Hands-on
- Build Java app using Maven  
- Deploy WAR on Tomcat  

---

## Phase 4 – Code Quality & Artifact Management

### SonarQube
- Static code analysis  
- Bugs & vulnerabilities  
- Quality gates  

### Nexus
- Maven repositories  
- Snapshot & Release  
- Artifact versioning  

### Hands-on
- Scan code using SonarQube  
- Push build artifact to Nexus  

---

## Phase 5 – Jenkins CI/CD

### Jenkins
- Jenkins master & agents  
- Jenkinsfile  
- Pipeline stages  
- Credentials  
- GitLab webhook  

### Pipeline Flow
```
GitLab → Jenkins → SonarQube → Maven → Nexus → Docker
```

### Hands-on
- Create Jenkins pipeline  
- Auto-trigger build from GitLab  

---

## Phase 6 – Docker

### Topics
- Docker images & containers  
- Dockerfile  
- Volumes  
- Networks  

### Hands-on
- Create Docker image of Java app  
- Push image to Docker Registry  

---

## Phase 7 – Kubernetes

### Topics
- Pods  
- Deployments  
- Services  
- Ingress  
- ConfigMaps  
- Secrets  
- HPA  

### Hands-on
- Deploy Java app to Kubernetes  
- Expose via Ingress  
- Perform rolling updates  

---

## Phase 8 – Ansible

### Topics
- Inventory  
- Playbooks  
- Roles  
- SSH based automation  

### Hands-on
- Install Docker using Ansible  
- Install Jenkins on VM  
- Configure Kubernetes nodes  

---

## Phase 9 – Terraform (GCP)

### Topics
- Providers  
- Resources  
- Variables  
- State management  
- Modules  

### Hands-on
- Create GCP VM  
- Create VPC  
- Provision Kubernetes  

---

## Phase 10 – Vagrant

### Topics
- Local VM automation  
- Multi-VM environments  

### Hands-on
- Create Jenkins VM  
- Create GitLab VM  
- Create Docker VM  

---

## Phase 11 – Final Enterprise DevOps Project

### CI/CD Flow
```
Developer → GitLab
            ↓
         Jenkins
            ↓
        SonarQube
            ↓
         Maven
            ↓
         Nexus
            ↓
         Docker
            ↓
       Kubernetes (GCP)
```

### Project Tasks
- Developer pushes Java code  
- Jenkins builds & tests  
- SonarQube checks quality  
- Artifact stored in Nexus  
- Docker image created  
- App deployed to Kubernetes  
- Public URL exposed  
