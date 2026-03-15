---
title: "nexus"
date: 2025-08-01
description: "A detailed guide to installing, configuring, and integrating Nexus Repository Manager for artifact management and CI/CD pipelines"
---

# Nexus Repository Manager

## Overview
**Nexus Repository Manager**, developed by Sonatype, is a **centralized repository manager** for storing, managing, and distributing software artifacts and dependencies. It is widely used in modern software development to ensure reliable and consistent access to build outputs.

---

## Key Features
1. **Artifact Management**  
   - Stores a variety of artifacts such as JARs, WARs, Docker images, npm packages, etc.  
   - Supports multiple package formats including **Maven, Gradle, npm, NuGet, Docker, PyPI**, and more.

2. **Repository Proxying**  
   - Caches external repositories (e.g., Maven Central, npm registry) locally to improve build performance and reliability.

3. **Version Control for Artifacts**  
   - Maintains different versions of libraries and components for easy retrieval and dependency management.

4. **Security & Access Control**  
   - Implements role-based access to manage read/write/delete permissions for users and teams.

5. **CI/CD Integration**  
   - Seamlessly integrates with Jenkins, GitLab CI, Bamboo, and other pipeline tools for storing and retrieving build artifacts.

---

## Typical Usage in CI/CD
1. **Build Stage**  
   - Compiles code and generates artifacts (e.g., `my-app-1.0.0.jar`).

2. **Upload to Nexus**  
   - Pushes build artifacts to Nexus repositories such as `releases` or `snapshots`.

3. **Dependency Resolution**  
   - Future builds retrieve dependencies from Nexus instead of downloading them from external sources.

4. **Docker Image Management**  
   - Acts as a private Docker registry for storing and distributing container images.

---

## Common Nexus Repositories

| Repository Type | Purpose                  |
|-----------------|--------------------------|
| Maven/Gradle    | Java artifacts           |
| npm             | Node.js packages         |
| NuGet           | .NET packages            |
| Docker          | Docker images            |
| PyPI            | Python packages          |

---

## Example: Maven Deployment to Nexus
```xml
<distributionManagement>
  <repository>
    <id>nexus-releases</id>
    <url>http://nexus.example.com/repository/maven-releases/</url>
  </repository>
  <snapshotRepository>
    <id>nexus-snapshots</id>
    <url>http://nexus.example.com/repository/maven-snapshots/</url>
  </snapshotRepository>
</distributionManagement>
```