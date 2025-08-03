---
title: "Session 01"
description: "Learn Docker concepts, architecture, and how to write your first Dockerfile with real examples."
date: 2025-08-02
draft: false
tags: ["docker", "dockerfile", "containers", "devops", "tutorial"]
weight: 2
---


## 📦 What is a Container?

A **container** is a **lightweight, portable, and isolated runtime environment** that bundles:

- Your application code  
- All necessary dependencies (libraries, binaries, configs)  
- Environment variables  
- A minimal runtime  

Containers ensure that your application **runs consistently** across different environments — whether on a developer’s laptop, a test server, or in production.

---

### 🧊 Think of a Container as:

> A portable box that contains everything your app needs to run — no matter where you move it, it will work the same.

---

## 🆚 Container vs Virtual Machine (VM)

| Feature         | Container                      | Virtual Machine (VM)              |
|------------------|--------------------------------|-----------------------------------|
| Startup Time     | Seconds                        | Minutes                           |
| Size             | Lightweight (MBs)              | Heavy (GBs)                       |
| OS Isolation     | Shares host OS kernel          | Has full guest OS                 |
| Performance      | Near-native                    | Slight overhead                   |
| Use Case         | Microservices, cloud-native    | Full OS apps, legacy support      |

---

## 📸 Container Architecture (Simplified)

```
+----------------------------+
|        Host OS             |
|  +----------------------+  |
|  | Container Runtime     |  |
|  |  +------------------+ | |
|  |  |   Container App   | | |
|  |  +------------------+ | |
|  +----------------------+  |
+----------------------------+
```

---

## ✅ Why Containers?

- **Portability**: Run your app anywhere (laptop, server, cloud)  
- **Consistency**: Eliminate "it works on my machine" issues  
- **Isolation**: Keep apps independent and secure  
- **Efficiency**: Use fewer resources than virtual machines  
- **Scalability**: Easily scale containers in/out based on demand  

---

## 📌 Real-world Example

Suppose you're developing a Python app with Flask that needs Python 3.10 and specific libraries.

If you run it on a machine without those exact versions, it may fail.

With a **container**, you bundle Python 3.10, Flask, and your app into one image. That image runs **the same way** on any Docker-enabled system.

---

> 💡 Containers are essential to modern DevOps, CI/CD, and cloud-native application development.


## 🐳 Docker and Dockerfile 

## 📌 Introduction

Docker is a platform that allows you to **build, ship, and run** applications in lightweight, portable **containers**. This guide walks you through:

- What Docker is  
- Why it’s useful  
- Docker architecture  
- Writing your first Dockerfile  
- Building and running a container  

---

## 🔍 What is Docker?

Docker is an open-source tool that helps developers:

- Package applications and dependencies into a **container**
- Run containers **consistently** across environments
- Simplify deployment and scaling

### ✅ Key Benefits

- Environment consistency across dev, test, and prod  
- Lightweight compared to virtual machines  
- Fast startup and shutdown  
- Isolated apps (better security and control)  

---

## ⚙️ Docker Architecture

| Component           | Description                              |
|---------------------|------------------------------------------|
| **Docker Engine**   | Core runtime to build and run containers |
| **Docker Images**   | Read-only templates used to create containers |
| **Docker Containers** | Running instances of Docker images    |
| **Dockerfile**      | A script to define how to build an image |
| **Docker Hub**      | A public registry to host and share images |

## Check the images
```sh
docker images
```
## 🚀 Run Docker Container

```bash
docker run <Image name>
```

## 📋 Check Running and Stopped Containers

```bash
docker ps            # Running containers
docker ps -a         # All containers
```

---

## 📂 Container Logs

```bash
docker logs <container_id>
```

---

## 🔐 Docker Hub Login

```bash
docker login
```

> Provide your Docker Hub username and password.

---

## ☁️ Tag and Push Image to Docker Hub

### Step 1: Tag the Image

```bash
docker tag my-first-docker-app your_dockerhub_username/my-first-docker-app:latest
```

### Step 2: Push the Image

```bash
docker push your_dockerhub_username/my-first-docker-app:latest
```

You can now access it on [https://hub.docker.com](https://hub.docker.com).

---

## 🧹 Docker Cleanup Commands

```bash
docker rm <container_id>          # Remove container
docker rmi <image_id>             # Remove image
docker system prune -f            # Remove unused data
```


## Tasks 01
- Create a mysql db container 
- check the logs if this is failed
- fix and rerun
- try to Access you db
- Convert running container into an image and push to dockerhub.

## Task 02
- Create a container with volumes 
```sh
docker run -d --name test01 -v <hostlocation:Containerlocation> nginx 
```
- Create a container to expose the app to access from browser.
```sh
docker run -d --name test01 -p 80:80 nginx 
```


## 🧱 What is a Dockerfile?

A **Dockerfile** is a text file that contains **step-by-step instructions** on how to create a Docker image.

**Conceptual Flow:**

Dockerfile → Docker Image → Docker Container

### 🔧 Common Instructions

| Instruction | Purpose                            |
|-------------|------------------------------------|
| `FROM`      | Base image to build from           |
| `WORKDIR`   | Set working directory              |
| `COPY`      | Copy files from host to container  |
| `RUN`       | Execute commands during image build|
| `CMD`       | Default command to run on start    |
| `EXPOSE`    | Document container's port          |

---

## 🏗️ Sample Project – Python App in Docker

### 📁 Directory Structure

myapp/  
├── app.py  
└── Dockerfile

### 📝 app.py

```python
print("Hello from Docker!")
```

### 🛠️ Dockerfile

```dockerfile
# Use base image
FROM python:3.10-slim

# Set working directory
WORKDIR /app

# Copy app code
COPY app.py .

# Run the script
CMD ["python", "app.py"]
```

---

## 🧪 Build and Run the Docker Container

### 🔨 Step 1: Build the Image

```bash
docker build -t my-first-docker-app .
```

### 🚀 Step 2: Run the Container

```bash
docker run my-first-docker-app
```

### ✅ Output

Hello from Docker!

---

## 🧹 Docker Cleanup Commands

```bash
docker ps -a               # List all containers
docker rm <container-id>   # Remove a specific container
docker images              # List all images
docker rmi <image-id>      # Remove a specific image
```

---



## 🆚 Docker CMD vs ENTRYPOINT: What’s the Difference?

When defining a Dockerfile, you typically use either **CMD** or **ENTRYPOINT** to specify what command should be run when the container starts.

While they may look similar, **they serve different purposes** and behave differently.

---

### 🔹 CMD

- Provides **default arguments** for the container’s execution.
- **Can be overridden** by command-line arguments passed with `docker run`.
- Only **one CMD instruction** is allowed in a Dockerfile (if multiple are present, only the last is used).

#### ✅ Example

```dockerfile
FROM ubuntu
CMD ["echo", "Hello from CMD"]
```

```bash
docker build -t cmd-example .
docker run cmd-example           # Output: Hello from CMD
docker run cmd-example echo Hi  # Output: Hi
```

---

### 🔸 ENTRYPOINT

- Defines the **main command** that **cannot be overridden** by arguments passed in `docker run`.
- It **always runs**, and CLI arguments are passed as parameters to it.

#### ✅ Example

```dockerfile
FROM ubuntu
ENTRYPOINT ["echo", "Hello from ENTRYPOINT"]
```

```bash
docker build -t entrypoint-example .
docker run entrypoint-example             # Output: Hello from ENTRYPOINT
docker run entrypoint-example User123     # Output: Hello from ENTRYPOINT User123
```

---

### 🔄 Using ENTRYPOINT with CMD

You can use **ENTRYPOINT** to define the main command and **CMD** to provide default parameters.

```dockerfile
FROM ubuntu
ENTRYPOINT ["echo"]
CMD ["Hello from both"]
```

```bash
docker run both-example                # Output: Hello from both
docker run both-example "Custom msg"  # Output: Custom msg
```
### 🧠 Summary

| Feature             | CMD                             | ENTRYPOINT                          |
|---------------------|----------------------------------|--------------------------------------|
| Overridable?        | ✅ Yes                           | 🚫 No (executes always)              |
| Use case            | Default arguments               | Fixed main process                   |
| Shell/Exec support  | Both                            | Both                                 |
| Best used for       | Flexibility                     | Wrapper for legacy tools, enforced run behavior |

> 🔍 Use **ENTRYPOINT** when your container should always run a specific binary, and **CMD** to supply optional default arguments.



## 📂 Dockerfile: COPY vs ADD – What’s the Difference?

Both `COPY` and `ADD` are Dockerfile instructions used to transfer files from your local system into a Docker image during the build process. However, they are not identical.

---

### 🟦 COPY

- **Purpose:** Simple file/directory copy from local context into the image.
- **Behavior:** Only copies files and directories.
- **No extra features** (e.g., does not extract archives or support URLs).

#### ✅ Syntax

```dockerfile
COPY <src> <dest>
```

#### ✅ Example

```dockerfile
COPY app.py /app/
COPY config/ /app/config/
```

---

### 🟨 ADD

- **Purpose:** More powerful than `COPY`, but also more error-prone.
- Supports:
  - **File copying**
  - **Automatic tar archive extraction**
  - **Remote URL downloads**

#### ✅ Syntax

```dockerfile
ADD <src> <dest>
```

#### ✅ Example (with local files)

```dockerfile
ADD app.tar.gz /app/     # Automatically extracts to /app/
```

#### ⚠️ Example (with URL)

```dockerfile
ADD https://example.com/file.txt /app/file.txt
```

---

## ⚖️ COPY vs ADD: Feature Comparison

| Feature                      | COPY     | ADD      |
|-----------------------------|----------|----------|
| Copy local files            | ✅ Yes   | ✅ Yes   |
| Extract tar archives        | ❌ No    | ✅ Yes   |
| Download from URLs          | ❌ No    | ✅ Yes   |
| Simpler & more predictable  | ✅ Yes   | ❌ No    |

---

## 🧠 Best Practice

> 🛡️ **Use `COPY` by default.**
> Only use `ADD` if you need its special features like auto-extraction or remote downloads.

---

## 💡 Example: When to Use What

```dockerfile
# Prefer COPY for clarity
COPY . /app/

# Use ADD only when required
ADD myapp.tar.gz /app/       # Unpacks the archive
ADD https://example.com/config.yaml /app/config.yaml
```


## 🏗️ Docker Multi-Stage Builds

---

## 🔍 What Are Multi-Stage Builds?

Multi-stage builds allow you to use **multiple `FROM` instructions** in a single Dockerfile to:

- Separate the **build environment** from the **runtime image**
- **Minimize final image size**
- Improve **security and performance**

---

## ✅ Why Use Multi-Stage Builds?

In traditional Docker builds:

- Build tools, compilers, and source files get bundled into the final image
- This bloats the image and exposes internal code and tools

**Multi-stage builds solve this** by building your app in one stage, then copying only what’s needed to a second minimal image.

---

## 🛠️ Sample Dockerfile: Go Application

```dockerfile
# Stage 1: Builder
FROM golang:1.21 AS builder
WORKDIR /app
COPY . .
RUN go build -o myapp

# Stage 2: Final image
FROM alpine:3.19
WORKDIR /app
COPY --from=builder /app/myapp .
CMD ["./myapp"]
```

---

## 📋 Step-by-Step Explanation

1. **First stage** (builder):
   - Uses `golang` image to compile the application
   - Output is the binary `myapp`
2. **Second stage**:
   - Uses lightweight `alpine` image
   - Copies only the binary from the builder
   - Runs the binary using `CMD`

---

## ⚖️ Advantages

- 🧹 **Cleaner**: Only final artifacts in runtime image  
- 🔐 **Secure**: No compilers, secrets, or source code left behind  
- 📦 **Lightweight**: Smaller and faster image  

---

## 🔧 Additional Notes

- You can name stages using `AS <name>` and refer to them with `--from=<name>`
- You can have **more than two stages** (e.g., test, build, package)
- Multi-stage builds are supported in **Docker 17.05+**

---

## Maven Build Example
- Clone the Maven sample Project
```sh
git clone https://github.com/khushiramsingh680/studentapp.git
```
- Create a Dockerfile
```sh
# Stage 1: Builder
FROM maven:3.3-jdk-8 AS builder
WORKDIR /app
COPY . .
RUN mvn clean install

FROM tomcat
WORKDIR /usr/local/tomcat/webapps
COPY --from=builder /app/target/studentapp-2.5-SNAPSHOT.war  .
```
-  Now Generate an image
```sh
docker build -t <name> .
```
- Create a container using this image
```sh
docker run -d -p 88:8080 <image name>
```
- Access your app from Browser
```sh
ip:88/studentapp-2.5-SNAPSHOT/
```
## Container UI Tool [Portainer](https://docs.portainer.io/start/install-ce)


