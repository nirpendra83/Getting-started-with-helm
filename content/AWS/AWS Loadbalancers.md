---
title: "AWS Load Balancers"
date: 2025-08-16
draft: false
---

## AWS Load Balancers

AWS provides **Elastic Load Balancing (ELB)** services to automatically distribute incoming application traffic across multiple targets, such as Amazon EC2 instances, containers, IP addresses, and Lambda functions.  

Load balancers help improve **fault tolerance, scalability, and availability**.

---

## Types of AWS Load Balancers

### 1. **Application Load Balancer (ALB)**
- Operates at **Layer 7 (HTTP/HTTPS)**.
- Best for **web applications**.
- Supports advanced routing:  
  - Path-based (e.g., `/images` → one service, `/api` → another service).  
  - Host-based (e.g., `api.example.com` vs `app.example.com`).  
- Integrates with **AWS WAF** and **CloudWatch**.  

---

### 2. **Network Load Balancer (NLB)**
- Operates at **Layer 4 (TCP, UDP, TLS)**.
- Handles **millions of requests per second** with ultra-low latency.
- Best for:
  - Real-time gaming.
  - IoT applications.
  - High-performance workloads.

---

### 3. **Gateway Load Balancer (GWLB)**
- Operates at **Layer 3 (Network Layer)**.
- Deploys, scales, and manages **third-party virtual appliances** (e.g., firewalls, intrusion detection, deep packet inspection).
- Routes traffic using **Geneve protocol**.

---

### 4. **Classic Load Balancer (CLB)** *(Legacy)*
- Works at both **Layer 4 and Layer 7**.
- Limited features, mainly for older applications.
- Recommended to migrate to **ALB** or **NLB**.

---

## Key Features

- **High Availability** – Automatically distributes traffic across multiple AZs.  
- **Health Checks** – Routes traffic only to healthy targets.  
- **SSL/TLS Termination** – Offloads encryption from application servers.  
- **Integration** – Works with **Auto Scaling, CloudWatch, Route 53, and WAF**.  
- **Security** – Supports **Security Groups** and **IAM policies**.

---

## Use Cases

- **ALB** → Microservices, APIs, web apps.  
- **NLB** → Gaming, streaming, IoT, financial apps.  
- **GWLB** → Security appliances (firewalls, packet inspection).  
- **CLB** → Legacy workloads (not recommended for new apps).  

---

## Example: Architecture Diagram

```plaintext
   Clients
      |
   [AWS Route 53]
      |
   [Elastic Load Balancer]
    /           \
 [EC2-1]      [EC2-2]
```