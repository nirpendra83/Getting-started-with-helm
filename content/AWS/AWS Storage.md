---
title: "Aws Storage"
description: "In-depth chapter on Amazon EC2 for AWS Certified Solutions Architect Associate preparation, including tasks to gain hands-on experience."
date: 2025-08-03
tags: ["aws", "ec2", "compute", "solution architect", "saa-c03"]
categories: ["AWS", "Certification"]
draft: false
weight: 20
---
# 🗂️ AWS Storage Overview

## 1. Object Storage
- **Amazon S3 (Simple Storage Service)**
  - Stores data as objects (files + metadata).
  - Virtually unlimited storage.
  - Accessed over HTTP(S).
  - **Use cases**: backups, data lakes, static website hosting, ML data storage.
  - Features: versioning, lifecycle policies, cross-region replication.

- **S3 Glacier / Glacier Deep Archive**
  - Low-cost archival storage.
  - Retrieval options: minutes to hours.
  - **Use cases**: compliance data, long-term archives.

---

## 2. Block Storage
- **Amazon EBS (Elastic Block Store)**
  - Block-level storage for **EC2 instances**.
  - Behaves like a hard drive.
  - Types: General Purpose SSD (gp3), Provisioned IOPS (io2), Throughput optimized HDD (st1), Cold HDD (sc1).
  - **Use cases**: OS volumes, databases, transactional workloads.

- **Instance Store (Ephemeral Storage)**
  - Storage physically attached to the EC2 host.
  - Very fast, but data is lost if the instance stops/terminates.
  - **Use cases**: cache, temporary data, high-performance compute.

---

## 3. File Storage
- **Amazon EFS (Elastic File System)**
  - Managed **NFS (Network File System)**.
  - Shared storage for multiple EC2s, scalable, elastic.
  - **Use cases**: web servers, CMS, analytics workloads.

- **Amazon FSx**
  - Managed file systems for specific workloads:
    - FSx for Windows File Server (SMB protocol, Active Directory integration).
    - FSx for Lustre (HPC, ML, big data).
    - FSx for OpenZFS (low-latency workloads).
    - FSx for NetApp ONTAP (enterprise workloads).

---

## 4. Hybrid & Data Transfer
- **AWS Storage Gateway**
  - Connects on-premises apps with AWS cloud storage.
  - Types: File Gateway, Tape Gateway, Volume Gateway.

- **AWS Snow Family (Snowcone, Snowball, Snowmobile)**
  - Physical devices to move large datasets to AWS.
  - **Use cases**: data migration, edge computing, disconnected environments.

- **AWS DataSync**
  - Automated data transfer between on-premises and AWS storage.

---

## 5. Backup & Disaster Recovery
- **AWS Backup**
  - Centralized backup across AWS services (EBS, RDS, DynamoDB, EFS, FSx).
  - Policy-based, compliant storage.

---

## 🔑 Key Differences

| Storage Type  | Example Services              | Best For |
|---------------|-------------------------------|----------|
| Object        | S3, S3 Glacier                | Data lakes, backups, archives |
| Block         | EBS, Instance Store           | Databases, OS, transactional workloads |
| File          | EFS, FSx                      | Shared file systems, HPC, enterprise apps |
| Hybrid/Transfer | Storage Gateway, Snow Family, DataSync | Migration, hybrid cloud, offline data transfer |
| Backup        | AWS Backup                    | Centralized backup & compliance |
