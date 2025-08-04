---
title: "Session 01"
description: "In-depth chapter on Amazon EC2 for AWS Certified Solutions Architect Associate preparation, including tasks to gain hands-on experience."
date: 2025-08-03
tags: ["aws", "ec2", "compute", "solution architect", "saa-c03"]
categories: ["AWS", "Certification"]
draft: false
weight: 20
---

## 🖥️ Amazon EC2 (Elastic Compute Cloud)

Amazon EC2 provides scalable compute capacity in the AWS cloud. It allows you to launch virtual servers on demand and pay for what you use.

---

## 🔹 Key Concepts

### 1. **EC2 Instance Types**
- General Purpose (e.g., `t3`, `t4g`, `m5`)
- Compute Optimized (`c5`)
- Memory Optimized (`r5`, `x1`)
- Storage Optimized (`i3`, `d2`)
- Accelerated Computing (`p4`, `inf1`)

### 2. **Instance Lifecycle**
- Start → Running → Stop → Terminate
- Instance retirement and status checks

### 3. **Key EC2 Components**
- **AMI (Amazon Machine Image)** – OS and application environment
- **Instance Type** – Hardware configuration
- **Security Group** – Acts as a virtual firewall
- **Key Pair** – SSH access credentials
- **Elastic IP** – Static public IP address
- **User Data** – Shell script to run on first boot (cloud-init)

### 4. **Storage Options**
- **EBS (Elastic Block Store)**
- **Instance Store (ephemeral)**
- **EFS for network-attached storage**

### 5. **Networking**
- Public/Private Subnets
- Elastic IPs
- NAT Gateways
- Security Groups vs NACLs

---

## 🛠️ Hands-On Task List

| Task | Description |
|------|-------------|
| ✅ Launch an EC2 Instance | Use AWS Console or CLI to launch a `t2.micro` instance with Amazon Linux 2023. |
| ✅ Configure Security Group | Allow SSH (port 22) and HTTP (port 80). |
| ✅ SSH into EC2 | Connect using a PEM key from a Linux/Mac terminal or PuTTY for Windows. |
| ✅ Install a Web Server | Use `sudo yum install httpd -y` and start the service. |
| ✅ Add a Custom Index Page | Modify `/var/www/html/index.html` with your own content. |
| ✅ Use EC2 User Data | Launch a new instance that automatically installs a web server using user data. |
| ✅ Create AMI | Create a custom AMI from your configured instance. |
| ✅ Attach/Detach EBS Volume | Attach an additional EBS volume and mount it inside the instance. |
| ✅ Stop, Start, and Terminate | Observe instance state behavior and test persistent storage. |
| ✅ Enable Detailed Monitoring | Turn on CloudWatch detailed monitoring and view CPU graphs. |

---

## 📘 Exam Tips

- Know the difference between **EBS** and **Instance Store**.
- Understand **user data** vs **metadata** (`http://169.254.169.254`).
- Security groups are **stateful**, NACLs are **stateless**.
- EC2 instances by default do **not have public IPs** in private subnets.
- IAM roles are used to **assign permissions to instances** without hardcoding credentials.

---
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instancedata-data-retrieval.html
## Tasks to do
 - ✅ Change the existing ssh public key 
 
