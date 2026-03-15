
+++
title = "Day 01"
+++


# 🗓️ Day 01 – Maven Basics & Setup

## 📘 Introduction to Maven in DevOps Context
- Maven is a build automation and dependency management tool widely used in DevOps pipelines.  
- Helps standardize project builds, manage dependencies, and integrate with tools like Nexus, SonarQube, Jenkins/GitLab CI, and Tomcat.  
- In DevOps, Maven ensures repeatable builds and easy integration with CI/CD pipelines.  

---

## ⚙️ Installing Maven

### On Linux (RHEL/Ubuntu)
1. Install Java (required by Maven):  
   sudo apt install openjdk-11-jdk -y    # Ubuntu/Debian  
   sudo yum install java-11-openjdk -y   # RHEL/CentOS  

2. Download Maven:  
   wget https://downloads.apache.org/maven/maven-3/3.9.9/binaries/apache-maven-3.9.9-bin.tar.gz  

3. Extract and move:  
   tar -xvzf apache-maven-3.9.9-bin.tar.gz  
   sudo mv apache-maven-3.9.9 /opt/maven  

### On Windows
1. Download from https://maven.apache.org/download.cgi  
2. Extract into `C:\Program Files\Maven`  
3. Add `MAVEN_HOME` and update `PATH` in Environment Variables  

---

## 🔧 Configure Environment Variables

### On Linux
Add to `~/.bashrc` or `~/.zshrc`:  
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64  
export M2_HOME=/opt/maven  
export MAVEN_HOME=/opt/maven  
export PATH=$M2_HOME/bin:$PATH  

Reload shell:  
source ~/.bashrc  

### On Windows
- JAVA_HOME = C:\Program Files\Java\jdk-11  
- MAVEN_HOME = C:\Program Files\Maven  
- Add `%MAVEN_HOME%\bin` to PATH  

---

## ✅ Verify Installation
Run:  
mvn -v  

Expected output:  
Apache Maven 3.9.9 (...)  
Java version: 11.0.x  

---

## 🛠️ Hands-On: Create First Maven Project
Generate a project:  
mvn archetype:generate -DgroupId=com.demo -DartifactId=myapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false  

Project structure:  
myapp/  
 ├── src/main/java  
 ├── src/test/java  
 └── pom.xml  

Build and install:  
cd myapp  
mvn clean install  

---

## 🎯 Assignment
- Install Maven on your system (Linux or Windows).  
- Configure JAVA_HOME and MAVEN_HOME.  
- Create your first Maven project and run `mvn clean install`.  
- Explore the `target/` folder to see generated artifacts.  
