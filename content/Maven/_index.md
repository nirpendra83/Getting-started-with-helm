+++
title = "Maven"
+++

## 🧾 What is Maven?

**Maven** is a **free**, **open-source**, and **build automation tool** primarily used for **Java projects**. It simplifies project build, dependency management, and documentation with a **standardized project structure**.

---

## 🔧 In Simple Terms

Maven helps you **compile code**, **manage dependencies**, **run tests**, and **package applications** — all using a **single configuration file (`pom.xml`)**.

---

## 🧠 Key Features of Maven

| Feature                 | Description                                                                 |
|-------------------------|-----------------------------------------------------------------------------|
| ✅ Dependency Management | Automatically downloads and manages required libraries (JARs).              |
| ⏳ Standardization       | Provides a consistent project structure across teams.                      |
| 🧪 Build Lifecycle       | Defines phases like compile, test, package, install, and deploy.           |
| ⚡ Plugin System         | Uses plugins to extend functionality (testing, reporting, deployment).     |
| 🔗 Integration           | Works well with CI/CD tools like Jenkins, GitLab CI, and GitHub Actions.   |

---

## 📦 Maven vs Gradle

| Maven                                   | Gradle                                     |
|-----------------------------------------|--------------------------------------------|
| XML-based configuration (`pom.xml`)     | Groovy/Kotlin DSL-based configuration       |
| Convention over configuration           | More flexibility, faster builds             |
| Mature, widely adopted                  | Newer, modern, and optimized for performance|

---

## 📂 Real-World Analogy

Imagine **building a house**:

- **Maven** is like the **construction manager** who follows a **blueprint** (`pom.xml`).
- It **fetches materials** (dependencies) automatically when needed.
- It ensures each **phase** (foundation, walls, painting) happens in the **right order**.

---

## 💻 Common Maven Commands

| Command                          | Purpose                                      |
|---------------------------------|----------------------------------------------|
| `mvn clean`                     | Removes previously compiled files            |
| `mvn compile`                   | Compiles the source code                     |
| `mvn test`                      | Runs the test cases                          |
| `mvn package`                   | Packages the code into JAR/WAR file          |
| `mvn install`                   | Installs package into local repository       |
| `mvn deploy`                    | Deploys package to remote repository         |
| `mvn dependency:tree`           | Displays dependency hierarchy                |

---

## 🧑‍🤝‍🧑 Who Uses Maven?

- Java Developers  
- DevOps Engineers  
- QA/Test Engineers  
- Build & Release Engineers  



## 📅 Maven 10-Day Training Plan (DevOps-Oriented)

### 🔹 Day 1: Maven Basics & Setup
- Introduction to Maven in DevOps context
- Install Maven on Linux (RHEL/Ubuntu) & Windows
- Configure environment variables (JAVA_HOME, MAVEN_HOME)
- Hands-on: Create a simple Maven project using archetype

---

### 🔹 Day 2: POM & Build Lifecycle
- Deep dive into `pom.xml`
- GAV (GroupId, ArtifactId, Version) explained
- Maven build lifecycle (validate → compile → test → package → install → deploy)
- Hands-on: Run `mvn clean install` and observe lifecycle phases

---

### 🔹 Day 3: Dependency Management
- Understanding repositories (local, central, remote)
- Dependency scopes (compile, provided, runtime, test)
- Hands-on: Add logging and testing dependencies
- Managing transitive dependencies
- Introduction to **OWASP Dependency-Check Plugin** (security scanning)

---

### 🔹 Day 4: Maven Plugins in Depth
- What are plugins? Why they matter in DevOps
- Common plugins:  
  - Compiler  
  - Surefire (unit testing)  
  - Failsafe (integration testing)  
  - Shade (fat JAR)  
- Hands-on: Configure plugins in `pom.xml`
- Automating builds with plugins

---

### 🔹 Day 5: Nexus Repository Integration
- Introduction to Nexus/Artifactory in CI/CD
- Configuring `settings.xml` for private repository
- Deploying artifacts to Nexus using `mvn deploy`
- Hands-on: Setup Maven to publish to a Nexus repository

---

### 🔹 Day 6: SonarQube Integration
- Why code quality & static analysis matter in DevOps
- Installing/configuring SonarQube
- Sonar Maven plugin
- Hands-on: Analyze a Maven project with SonarQube
- Automating SonarQube scans in CI/CD pipeline

---

### 🔹 Day 7: Tomcat Deployment
- Introduction to application deployment with Tomcat
- Tomcat Maven Plugin overview
- Hands-on: Deploy a WAR file to local Tomcat using Maven
- Automating deployment process with profiles

---

### 🔹 Day 8: Build Profiles & Multi-Module Projects
- Creating environment-specific build profiles (dev, test, prod)
- Activating profiles in CI/CD
- Hands-on: Multi-module Maven project (API + Service + UI)
- Using profiles for environment-specific deployments

---

### 🔹 Day 9: GitLab CI/CD with Maven
- Integrating Maven builds into GitLab pipelines
- Writing `.gitlab-ci.yml` for build → test → package → deploy stages
- Pushing build artifacts to Nexus from GitLab
- Hands-on: End-to-end GitLab CI/CD pipeline for a Maven project

---

### 🔹 Day 10: Advanced DevOps Practices
- Dependency checks & vulnerability management
- Generating SBOM (Software Bill of Materials) with Maven
- Versioning strategies (SNAPSHOT vs Release builds)
- Artifact promotion (Dev → Test → Prod)
- Final Hands-on:  
  - Build → Test → Code Scan (SonarQube + Dependency-Check)  
  - Publish artifact to Nexus  
  - Deploy to Tomcat  
  - Run pipeline in GitLab CI/CD

---

✅ **Outcome (DevOps-Focused)**  
By the end of Day 10, you will be able to:  
- Automate Maven builds in GitLab pipelines  
- Scan dependencies for vulnerabilities (OWASP Dependency-Check)  
- Perform code quality checks with SonarQube  
- Deploy artifacts to Tomcat automatically  
- Manage and promote artifacts using Nexus/Artifactory  
- Build a complete **DevOps-ready Maven CI/CD pipeline**  

