---
title: "AWS CloudWatch"
date: 2025-08-16
draft: false
tags: ["AWS", "CloudWatch", "Monitoring", "Observability"]
---

## AWS CloudWatch

AWS **CloudWatch** is a monitoring and observability service that provides actionable insights for applications, system performance, and infrastructure resources.

---

## 🔹 Key Features
- **Metrics Collection**  
  Collects and tracks metrics from AWS services, applications, and custom sources.

- **Logs Monitoring**  
  Ingests, stores, and analyzes logs from applications, Lambda, EC2, and other services.

- **Alarms**  
  Set thresholds on metrics to trigger notifications or automated actions.

- **Dashboards**  
  Create custom, real-time dashboards for visibility into system performance.

- **Events**  
  Detect and respond to changes in AWS resources using **EventBridge**.

- **Application Insights**  
  Provides automated setup and monitoring for enterprise applications.

---

## 🔹 Common Use Cases
1. Monitor **EC2 instance CPU utilization**.
2. Track **Lambda function errors**.
3. Set **alarms for RDS free storage space**.
4. Analyze **application logs for errors**.
5. Automate responses to infrastructure changes.

---

## 🔹 CloudWatch Components
- **Metrics** → Numeric data points from resources.  
- **Alarms** → Actions triggered when a threshold is breached.  
- **Logs** → Centralized logging for analysis and troubleshooting.  
- **Dashboards** → Visualizations for metrics and logs.  
- **Events (EventBridge)** → Reacts to changes and routes events.

---

## 🔹 Example: Creating an Alarm (CLI)
```bash
aws cloudwatch put-metric-alarm \
  --alarm-name "HighCPUUtilization" \
  --metric-name CPUUtilization \
  --namespace AWS/EC2 \
  --statistic Average \
  --period 300 \
  --threshold 80 \
  --comparison-operator GreaterThanThreshold \
  --dimensions Name=InstanceId,Value=i-0123456789abcdef0 \
  --evaluation-periods 2 \
  --alarm-actions arn:aws:sns:us-east-1:111122223333:NotifyMe
```