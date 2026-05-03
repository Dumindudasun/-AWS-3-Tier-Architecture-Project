# AWS 3-Tier Architecture Project

## Overview

Designed and deployed a secure **3-Tier Architecture on AWS** using networking, compute, database, and load balancing services.  
This project demonstrates how real-world applications are structured using separate **Web, Application, and Database tiers** for better security, scalability, and maintainability.

---

# Architecture
<img width="1536" height="1024" alt="ChatGPT Image May 2, 2026, 10_17_06 AM (1)" src="https://github.com/user-attachments/assets/6c16b8ab-8956-4463-8489-f7dd0ee32d71" />


```text
Users
 ↓
ALB
 ↓
Web Tier (Frontend)
 ↓
Application Tier (Backend API)
 ↓
Database Tier (RDS)


                    ┌──────────────────────┐
                    │      Internet User   │
                    └──────────┬───────────┘
                               │ HTTP/HTTPS
                               ▼
                  ┌────────────────────────────┐
                  │ Application Load Balancer  │
                  │         (alb-sg)           │
                  └──────────┬─────────────────┘
                             │
        ┌────────────────────┴────────────────────┐
        │                                         │
        ▼                                         ▼
┌──────────────────┐                     ┌──────────────────┐
│ Public Subnet-A  │                     │ Public Subnet-B  │
│   Web EC2        │                     │   Web EC2        │
│   Nginx/Apache   │                     │   Nginx/Apache   │
│   (web-sg)       │                     │   (web-sg)       │
└────────┬─────────┘                     └────────┬─────────┘
         │ HTTP 3000                                 │
         └──────────────┬────────────────────────────┘
                        ▼
              ┌───────────────────────┐
              │   App Tier EC2        │
              │ Node.js / Python API  │
              │     (app-sg)          │
              └──────────┬────────────┘
                         │ MySQL 3306
                         ▼
              ┌───────────────────────┐
              │   Amazon RDS MySQL    │
              │      (rds-sg)         │
              └───────────────────────┘

```


## Deployment Steps
1.Created custom VPC

2.Created public and private subnets

3.Attached Internet Gateway

4.Configured route tables

5.Created security groups

6.Launched web tier EC2 instance

7.Launched application tier EC2 instance

8.Created RDS MySQL database

9.Configured Application Load Balancer

10.Tested complete traffic flow

## Security Highlights
Internet traffic allowed only to ALB

Backend servers not publicly accessible

Database isolated in private subnet

Controlled tier-to-tier communication

Principle of least privilege applied

## Create VPC
Region: us-east-1

Create VPC:

Name: three-tier-vpc

CIDR: 10.0.0.0/16
<img width="1701" height="557" alt="image" src="https://github.com/user-attachments/assets/b39a54f9-04b9-4bc2-acaf-7d449e246ccb" />

## Network Layout (VPC)
<img width="1865" height="726" alt="image" src="https://github.com/user-attachments/assets/89058e3a-f464-4e2e-a0cd-9ee845eee853" />

VPC: 10.0.0.0/16

Public Subnets:
10.0.1.0/24
10.0.2.0/24
10.0.3.0/24

Private App Subnets:
10.0.11.0/24
10.0.12.0/24

Private DB Subnets:
10.0.21.0/24
10.0.22.0/24

## Security Flow

Internet → ALB (80/443)

ALB → Web EC2 (80)

Web EC2 → App EC2 (3000)

App EC2 → RDS (3306)

Internet ✘ App EC2

Internet ✘ RDS

## Internet Gateway and Route Tables
### Create Internet Gateway:
Name: three-tier-igw

Attach to: three-tier-vpc
<img width="1583" height="212" alt="image" src="https://github.com/user-attachments/assets/3b09f19d-199f-4f54-bf6a-d9adffbcb187" />

### Public route table
Name: public-rt

Route:

0.0.0.0/0 → Internet Gateway

Associate:

Public Subnet 1

Public Subnet 2
<img width="1522" height="207" alt="image" src="https://github.com/user-attachments/assets/18e96138-df6c-4c18-aff9-a75a6195102b" />
<img width="1541" height="365" alt="image" src="https://github.com/user-attachments/assets/2ae41894-a114-4279-9061-115dda0ee1ea" />


### Private route tables:
app-private-rt
<img width="1548" height="345" alt="image" src="https://github.com/user-attachments/assets/f502440f-1020-4450-8528-7cfd881b3829" />

<img width="1532" height="356" alt="image" src="https://github.com/user-attachments/assets/d5f59423-4be8-4c32-8d86-d766f4cef387" />


db-private-rt
<img width="1550" height="342" alt="image" src="https://github.com/user-attachments/assets/7f44dd3c-cd62-4b4f-ba0e-da239f04c745" />
<img width="1532" height="302" alt="image" src="https://github.com/user-attachments/assets/c53de84e-4784-463d-a7e6-35cebc8584bc" />
### all Rote table
<img width="1580" height="410" alt="image" src="https://github.com/user-attachments/assets/4fe49744-4737-446f-8a01-35284620ae66" />




## All Security Grops
<img width="1450" height="460" alt="all security grop" src="https://github.com/user-attachments/assets/e57ca8ad-f060-4ca5-b0b9-71bc1e52582b" />

## 1. ALB SG
HTTP 80 from 0.0.0.0/0
<img width="1578" height="225" alt="sg inbout" src="https://github.com/user-attachments/assets/ff5543cb-571f-4dc0-a798-fd0cf286a98b" />
## 2. Web SG
HTTP 80 from alb-sg

SSH 22 from My IP
<img width="1558" height="268" alt="websg" src="https://github.com/user-attachments/assets/8d0f53a0-7775-4630-9b0d-ed9ba2fd5da5" />

## 3. App SG
TCP 3000 from web-sg

SSH 22 from trusted SG
<img width="1552" height="267" alt="app" src="https://github.com/user-attachments/assets/ad4a1ab8-ed45-4fb9-b54d-6788ee5b7b0b" />

## 4. RDS SG
MySQL 3306 from app-sg
<img width="1555" height="248" alt="RDS" src="https://github.com/user-attachments/assets/ade8e2e3-e627-4ae7-adcc-f776e5ba27a3" />

# Database Tier Deployment (Amazon RDS MySQL)
Deployed a managed Database Tier using Amazon RDS MySQL as part of the AWS 3-tier architecture. The database was placed in private subnets with restricted access to improve security and align with production best practices.

### The database tier is responsible for:

Storing application data

Managing user records

Handling queries from the application tier

Providing persistent storage

Supporting backups and scalability

## RDS Configuration
| Setting         | Value            |
| --------------- | ---------------- |
| Database Engine | MySQL            |
| Template        | Free Tier        |
| DB Identifier   | `three-tier-db`  |
| Master Username | `admin`          |
| Instance Class  | `db.t3.micro`    |
| Public Access   | No               |
| VPC             | `three-tier-vpc` |
| Security Group  | `rds-sg`         |

<img width="1541" height="252" alt="image" src="https://github.com/user-attachments/assets/01ad749e-df4e-4fa6-ae71-ac42cd44f691" />

## Private Network Deployment
The RDS instance was deployed in private database subnets to prevent direct public access.

Internet → Database ❌

App Tier → Database ✅

This ensures only internal application servers can communicate with the database.

## DB Subnet Group
Created a dedicated DB subnet group spanning multiple Availability Zones for resilience and proper RDS deployment.
| Subnet Name         | Purpose                           |
| ------------------- | --------------------------------- |
| DB Private Subnet 1 | Primary private database subnet   |
| DB Private Subnet 2 | Secondary private database subnet |
<img width="1411" height="675" alt="image" src="https://github.com/user-attachments/assets/b215c86c-2700-4ce5-8d8c-fd45a52214c2" />

### Why This Was Required
RDS subnet groups must span at least two Availability Zones to meet AWS deployment requirements and support high availability options.

## Security Design
Security Group (rds-sg)

Inboud roule
| Port | Source   | Purpose                            |
| ---- | -------- | ---------------------------------- |
| 3306 | `app-sg` | MySQL access from application tier |
<img width="1577" height="220" alt="image" src="https://github.com/user-attachments/assets/65f1622f-a698-4c8c-9168-724801df20bc" />


## Access Control
Internet User → RDS ❌

Web Tier → RDS ❌

App Tier → RDS ✅



# Phase 7 — Application Load Balancer (ALB)
Implemented an internet-facing Application Load Balancer (ALB) to provide a secure and scalable public entry point for the AWS 3-tier architecture. The ALB distributes incoming traffic to healthy web tier instances and improves availability across multiple Availability Zones.

## Architecture Role
Internet User → ALB → Web Tier → App Tier → Database Tier

### The ALB is responsible for:

Receiving public HTTP traffic

Routing requests to healthy web servers

Acting as the single public entry point

Improving fault tolerance across multiple subnets

Supporting future horizontal scaling

## Load Balancer Configuration
| Setting         | Value                     |
| --------------- | ------------------------- |
| Name            | `three-tier-alb`          |
| Type            | Application Load Balancer |
| Scheme          | Internet-facing           |
| IP Address Type | IPv4                      |
| VPC             | `three-tier-vpc`          |
| Security Group  | `alb-sg`                  |

<img width="1568" height="267" alt="image" src="https://github.com/user-attachments/assets/0fbf1a74-1d08-4027-bccc-87226b42452b" />
<img width="1566" height="510" alt="image" src="https://github.com/user-attachments/assets/ca03b111-cd81-434c-a7f1-96b2536f181d" />


## Network Mapping
The ALB was deployed across two public subnets for high availability.
| Subnet          | Purpose             |
| --------------- | ------------------- |
| Public Subnet 1 | Availability Zone A |
| Public Subnet 2 | Availability Zone B |

This ensures traffic can continue even if one Availability Zone experiences issues.
<img width="1540" height="658" alt="image" src="https://github.com/user-attachments/assets/a2ec4897-a486-4b17-967b-4d61e017f9ee" />


## Security Design
### ALB Security Group (alb-sg)
Inbound Rules:
| Port | Source      | Purpose            |
| ---- | ----------- | ------------------ |
| 80   | `0.0.0.0/0` | Public HTTP access |
<img width="1527" height="307" alt="image" src="https://github.com/user-attachments/assets/063685f3-be65-4ab6-a48b-c3b7a964e34e" />

## Resource map
<img width="1577" height="522" alt="image" src="https://github.com/user-attachments/assets/a26944ca-a8b3-4d6d-afef-1ccf893f6a96" />



### Access Flow
Internet User → ALB ✅
ALB → Web Tier ✅
Internet User → Web Tier Direct ❌

## Target Group Configuration
Created a target group to route traffic to the web tier instance.
| Setting           | Value              |
| ----------------- | ------------------ |
| Target Group Name | `web-target-group` |
| Target Type       | Instance           |
| Protocol          | HTTP               |
| Port              | 80                 |
| Registered Target | `web-server`       |
<img width="1572" height="718" alt="image" src="https://github.com/user-attachments/assets/1c943da0-2631-4471-9b7d-c10de4d3b9a3" />


## Health Check Configuration
Configured health checks to continuously verify backend availability.
| Setting  | Value                            |
| -------- | -------------------------------- |
| Protocol | HTTP                             |
| Path     | `/`                              |
| Purpose  | Validate Nginx web server health |
If the target becomes unhealthy, the ALB stops sending traffic to it
<img width="1580" height="550" alt="image" src="https://github.com/user-attachments/assets/ba02ac38-4b25-4939-a3a6-d9e7fb93fd40" />
<img width="1540" height="351" alt="image" src="https://github.com/user-attachments/assets/2b188945-1ff6-467e-b69d-ef7f1ee298ac" />




## Listener Configuration
| Protocol | Port | Action                        |
| -------- | ---- | ----------------------------- |
| HTTP     | 80   | Forward to `web-target-group` |
<img width="1516" height="440" alt="image" src="https://github.com/user-attachments/assets/e69c0c6e-13fe-43d3-a095-ea7497beff5b" />


## Public Access
Users access the application using the ALB DNS name.
http://three-tier-alb-1400396043.us-east-1.elb.amazonaws.com

<img width="1462" height="770" alt="image" src="https://github.com/user-attachments/assets/f78da49d-907a-41b7-b830-a616bd73960a" />


## Skills Demonstrated
AWS Load Balancer Configuration

High Availability Design

Target Groups

Health Checks

Traffic Routing

Security Group Hardening

Cloud Networking

Scalable Web Architecture

