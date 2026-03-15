---
title: "AWS Certified Solutions Architect – Associate (SAA-C03) ToC"
description: "Comprehensive Table of Contents for the AWS Solutions Architect Associate certification exam, aligned with the latest SAA-C03 blueprint."
date: 2025-08-03
tags: ["aws", "certification", "solution architect", "saa-c03", "cloud", "exam"]
categories: ["AWS", "Certification"]
draft: false
weight: 10
---

## ✅ AWS Certified Solutions Architect – Associate (SAA-C03) - Table of Contents

### 1. Introduction
- About the SAA-C03 exam
- Target audience and prerequisites
- Exam overview (format, duration, passing score)
- Study resources and preparation tips

---

### 2. Domain 1: Design Secure Architectures (30%)
- IAM (Users, Roles, Groups, Policies)
- IAM Permissions boundaries and session policies
- Resource-based vs Identity-based policies
- AWS Organizations, SCPs, and account structure
- Encryption and Data Protection
  - AWS KMS, S3 encryption, EBS encryption
  - Secrets Manager & Systems Manager Parameter Store
- Secure Network Architecture
  - VPC, Security Groups, NACLs
  - PrivateLink, VPC Peering, VPN, Direct Connect

---

### 3. Domain 2: Design Resilient Architectures (26%)
- High Availability and Fault Tolerance
  - Multi-AZ, Multi-Region design patterns
- Load Balancing
  - ALB, NLB, CLB use cases
- Auto Scaling and Elasticity
- DNS-based Failover (Route 53)
- Backup & Disaster Recovery
  - Backup policies, AWS Backup
  - DR strategies: Pilot light, Warm standby, Active-active

---

### 4. Domain 3: Design High-Performing Architectures (24%)
- Storage Optimization
  - S3 Storage Classes (Standard, IA, Glacier)
  - EBS, EFS, FSx
- Compute Optimization
  - EC2 instance types and performance tuning
  - Lambda, Fargate, ECS, EKS
  - Placement groups
- Database Selection and Performance
  - RDS, Aurora, DynamoDB, Redshift
  - Read Replicas, Global Tables, ElastiCache
- Content Delivery and Caching
  - CloudFront, API Gateway caching

---

### 5. Domain 4: Design Cost-Optimized Architectures (20%)
- Resource Right-Sizing
- Pricing Models
  - On-Demand, Reserved, Spot Instances, Savings Plans
- Cost Management Tools
  - AWS Budgets, Cost Explorer, Trusted Advisor
- Cost-Effective Storage and Data Transfer

---

### 6. AWS Well-Architected Framework
- Pillars of the Well-Architected Framework
  - Operational Excellence
  - Security
  - Reliability
  - Performance Efficiency
  - Cost Optimization
  - Sustainability

---

### 7. Practice and Exam Preparation
- Sample questions and explanations
- Practice exams and mock tests
- Hands-on labs (AWS Free Tier, Qwiklabs, AWS SkillBuilder)
- Final review tips

---

### 8. Appendix
- Summary of Key AWS Services
- AWS CLI and SDK basics
- Architectural Design Patterns (Well-Architected Diagrams)
- Certification Exam FAQs
- Cheat Sheets and Flashcards

---


## AWS Certified Solutions Architect – Professional (SAP-C02) – Table of Contents

## Domain 1: Design Solutions for Organizational Complexity (26%)
- Requirements gathering and analysis  
- Designing multi-account and multi-VPC architectures  
- Hybrid connectivity strategies (on-premises ↔ AWS)  
- Centralized services (security, logging, networking)  
- Governance, compliance, and multi-team environment considerations  

---

## Domain 2: Design for New Solutions (29%)
- Translating business requirements into technical design  
- Selecting appropriate compute, storage, database, and networking services  
- Designing highly available and fault-tolerant architectures  
- Designing secure applications with IAM, KMS, and encryption  
- Choosing appropriate deployment strategies (containers, serverless, microservices)  
- Cost-optimized design patterns  

---

## Domain 3: Continuous Improvement for Existing Solutions (25%)
- Assessing and improving existing architectures  
- Performance optimization (compute, storage, databases, networking)  
- Cost optimization and rightsizing  
- Migrating workloads to modern architectures (monolith → microservices, lift & shift → re-architect)  
- Resiliency and disaster recovery improvements (RTO/RPO strategies)  
- Security enhancements and remediation  

---

## Domain 4: Accelerate Workload Migration and Modernization (20%)
- Migration strategies (6 R’s: Rehost, Replatform, Repurchase, Refactor, Retire, Retain)  
- Designing and implementing migration workflows (AWS Migration Hub, Application Migration Service, Database Migration Service)  
- Selecting modernization approaches (containers, serverless, managed services)  
- Data migration strategies (online/offline, Snowball, Direct Connect, DataSync)  
- Validating migration success and post-migration optimization  

---

## Additional Key Topics Across Domains
- AWS Well-Architected Framework (all pillars)  
- Designing with sustainability in mind  
- Multi-region architectures  
- Edge services (CloudFront, Global Accelerator, Route 53)  
- Observability (CloudWatch, X-Ray, OpenTelemetry, third-party integration)  
- Security best practices (least privilege, SCPs, encryption, monitoring)  





## Projects 

## AWS Solution Architect Professional – Project Ideas

### 1. Highly Available Multi-Tier Application
- Deploy a 3-tier architecture (web, app, DB) using ALB, Auto Scaling Groups, and RDS.  
- Add multi-AZ failover and caching with ElastiCache.  
- Apply IAM roles for least privilege and KMS encryption.  

---

### 2. Hybrid Cloud Connectivity
- Connect on-premises to AWS with VPN and Direct Connect.  
- Use Transit Gateway to connect multiple VPCs and accounts.  
- Demonstrate failover from VPN → Direct Connect.  

---

### 3. Disaster Recovery Architecture
- Build pilot-light and warm-standby strategies.  
- Use Route 53 health checks + failover routing.  
- Replicate RDS/EC2 across regions with RDS cross-region read replicas and S3 replication.  

---

### 4. Microservices on EKS with Service Mesh
- Deploy microservices on Amazon EKS.  
- Use AWS App Mesh or Istio for service-to-service communication.  
- Integrate with CloudWatch Container Insights and X-Ray tracing.  

---

### 5. Secure API Gateway with WAF
- Build a REST API with API Gateway + Lambda backend.  
- Apply AWS WAF rules and Cognito for authentication.  
- Add CloudFront for global content delivery.  

---

### 6. Multi-Region Active-Active Architecture
- Deploy applications across two AWS regions.  
- Use Route 53 latency-based routing with health checks.  
- Implement DynamoDB Global Tables and S3 Cross-Region Replication.  

---

### 7. Centralized Logging and Monitoring
- Aggregate logs from multiple accounts using CloudWatch Logs and S3.  
- Use OpenSearch (Elasticsearch) for log analytics.  
- Add GuardDuty, Security Hub, and AWS Config for compliance monitoring.  

---

### 8. Compliance-Driven Architecture
- Design workloads following AWS Well-Architected Framework (security & compliance focus).  
- Enforce IAM least privilege with Service Control Policies.  
- Use AWS Config rules, Security Hub, and Audit Manager for governance reporting.  

---

### 9. Event-Driven Architecture with Decoupling
- Build a decoupled system using SQS, SNS, and Lambda.  
- Implement retries, dead-letter queues (DLQ), and message filtering.  
- Demonstrate scalability and fault isolation through event-driven design.  

---

### 10. Edge-Optimized Architecture
- Use CloudFront for global content delivery.  
- Add AWS Global Accelerator for latency reduction.  
- Deploy WAF and Shield for DDoS protection at the edge.  

---
