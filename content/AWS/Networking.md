---
title: "AWS Cloud Networking"
description: "In-depth chapter on Amazon VPC and EC2 networking for AWS Certified Solutions Architect Associate preparation, with hands-on setup steps."
date: 2025-08-03
tags: ["aws", "vpc", "ec2", "networking", "solution architect", "saa-c03"]
categories: ["AWS", "Certification"]
draft: false
weight: 21
---

# AWS VPC Core Terminologies

## 1. VPC (Virtual Private Cloud)
- A logically isolated virtual network in AWS.
- You can launch AWS resources (EC2, RDS, etc.) inside a VPC.
- You define:
  - IP address range (CIDR block)
  - Subnets
  - Routing
  - Security settings

---

## 2. Subnet
- A segment of a VPC's IP address range.
- Two main types:
  - **Public Subnet** → Connected to the Internet via an Internet Gateway (IGW).
  - **Private Subnet** → No direct Internet access (typically uses NAT Gateway or VPN).
- Each subnet is tied to a single **Availability Zone (AZ)**.

---

## 3. Router (Route Table)
- Each VPC has a built-in virtual router.
- **Route tables** determine how traffic is directed:
  - Local routes (within the VPC)
  - External routes (e.g., Internet, VPN, Peering)
- Default route table automatically routes traffic inside the VPC.

---

## 4. Internet Gateway (IGW)
- A highly available, horizontally scaled VPC component.
- Enables communication between VPC resources and the Internet.
- Must be attached to the VPC and referenced in the route table.

---

## 5. NAT Gateway / NAT Instance
- **NAT Gateway**: AWS-managed service that allows private subnet instances to access the Internet (for updates, patches, etc.) without exposing them to inbound traffic.
- **NAT Instance**: Self-managed EC2 instance providing the same function (legacy option, less common today).

---

## 6. Security Components

### a. Network ACL (NACL)
- Stateless firewall at the **subnet level**.
- Controls inbound and outbound traffic using **allow or deny** rules.
- Rules are evaluated in order, from lowest to highest number.
- Applies to all resources in the subnet.

### b. Security Group (SG)
- Stateful firewall at the **instance level**.
- Controls inbound and outbound traffic for specific resources (e.g., EC2).
- Default: **deny all inbound, allow all outbound**.
- Rules only allow (no explicit deny).

---

## 7. DHCP Options Set
- Configurable DHCP settings for a VPC.
- Defines:
  - Domain name (e.g., `ec2.internal`)
  - DNS servers (AmazonProvidedDNS or custom)
  - NTP servers
  - NetBIOS settings (optional)

---

## 8. DNS in VPC
- Amazon provides built-in DNS resolution.
- Key settings:
  - **enableDnsSupport** → Enables DNS resolution within the VPC (default: true).
  - **enableDnsHostnames** → Assigns public DNS hostnames to instances with a public IP (default: false; must be enabled for public DNS).

---

## 9. VPC Peering
- Connects two VPCs privately over the AWS backbone.
- VPCs can communicate as if on the same network.
- **Transitive peering not supported** (A–B, B–C ≠ A–C).

---

## 10. VPC Endpoints
- Provide private connectivity between a VPC and supported AWS services without Internet, NAT, VPN, or Direct Connect.
- Types:
  - **Interface endpoint** → Elastic network interface in a subnet.
  - **Gateway endpoint** → Route table entry for services like S3, DynamoDB.

---

## 11. VPC Flow Logs
- Capture IP traffic information for network interfaces in a VPC.
- Useful for monitoring and troubleshooting.

---

## 12. Key Comparison: Subnet vs NACL

| Feature              | Subnet                                | NACL                               |
|----------------------|---------------------------------------|-------------------------------------|
| Level                | Network segmentation                  | Firewall rules for subnet           |
| Purpose              | Divide VPC into smaller networks      | Control traffic entering/leaving subnet |
| Statefulness         | Not stateful (just IP ranges)         | Stateless (evaluate inbound & outbound separately) |
| Association          | One subnet per AZ                    | Can be associated with multiple subnets |

---
# AWS VPC – ASCII Diagram

+-------------------+               +-------------------+
|   EC2 Instance    |               |   EC2 Instance    |
|   Public IP: Yes  |               |   Public IP: No   |
+-------------------+               +-------------------+
          |                                 |
          v                                 v
+-------------------+               +-------------------+
| Security Group    |               | Security Group    |
| (Stateful rules)  |               | (Stateful rules)  |
+-------------------+               +-------------------+
          |                                 |
          v                                 v
+-------------------+               +-------------------+
| Network ACL       |               | Network ACL       |
| (Stateless rules) |               | (Stateless rules) |
+-------------------+               +-------------------+
          |                                 |
          v                                 v
+-------------------+               +-------------------+
| Route Table       |               | Route Table       |
| Local + 0.0.0.0/0 |               | Local + NAT GW    |
+-------------------+               +-------------------+
          |                                 |
          v                                 v
+-------------------+               +-------------------+
| Internet Gateway  |               | NAT Gateway       |
| (IGW in Public    |               | (in Public Subnet)|
| Subnet)           |               |                   |
+-------------------+               +-------------------+
          |                                 |
          +---------------+-----------------+
                          v
                    +-----------+
                    |  Internet |
                    +-----------+

## Key Notes
- **IGW** → Required for Internet access (public subnets).
- **NAT GW** → Enables private subnet instances to access the Internet.
- **SG** → Stateful firewall at instance level.
- **NACL** → Stateless firewall at subnet level.
- **Route Tables** → Define traffic direction (local, IGW, NAT, peering, etc.).
- **DHCP/DNS** → Control hostname resolution and domain naming.

---

## VPC Setup on AWS (Step-by-Step)

### 1. Create a VPC
1. Open the [AWS Console](https://aws.amazon.com/console/).
2. Go to **VPC → Create VPC**.
3. Enter details:
   - Name: `MyVPC`
   - CIDR block: `10.0.0.0/16`
   - IPv6: Optional
   - Tenancy: Default
4. Click **Create VPC**.

### 2. Create Subnets
1. Go to **Subnets → Create subnet**.
2. Name: `Public-Subnet` → CIDR: `10.0.1.0/24` (AZ `us-east-1a`).
3. Name: `Private-Subnet` → CIDR: `10.0.2.0/24` (AZ `us-east-1b`).

### 3. Create an Internet Gateway
1. Go to **Internet Gateways → Create IGW**.
2. Attach it to your VPC.

### 4. Configure Route Tables
1. Create `PublicRouteTable` and associate with Public Subnet.
2. Add route `0.0.0.0/0 → IGW`.
3. For Private Subnet, add route `0.0.0.0/0 → NAT Gateway`.

### 5. Configure Security Groups
- Allow **SSH (22)** from your IP.
- Allow **HTTP (80)** and **HTTPS (443)**.
- Default: allow all outbound.

### 6. Launch an EC2 Instance
- Select AMI (Amazon Linux/Ubuntu).
- Place in `Public-Subnet` with SG attached.
- Assign a key pair and launch.

### 7. Test Connectivity
```bash
ssh -i your-key.pem ec2-user@<public-ip>
```


## Task
- [ ] Create a vpc
- [ ] Create 2  subnets
- [ ] Create IGW and attach to vpc
- [ ] Create a route and linked to subnet
- [ ] Create a vm in private subnet 
    - [ ] Check if you are able to connect vm 
-   [ ] Create a vm in public subnet subnet 
    - [ ] Check if you are able to connect vm 
- [ ] create a NAT Gateway and attach to a subnet
- [ ] Create two vpc and enable communication b/w them using vpc peering 
    - check across account as well
    -  check across region as well