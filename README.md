# AWS 3-Tier Architecture Project

## Overview

Designed and deployed a secure **3-Tier Architecture on AWS** using networking, compute, database, and load balancing services.  
This project demonstrates how real-world applications are structured using separate **Web, Application, and Database tiers** for better security, scalability, and maintainability.

---

# Architecture

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

## Network Layout (VPC)
VPC: 10.0.0.0/16

Public Subnets:
10.0.1.0/24
10.0.2.0/24

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
## 






