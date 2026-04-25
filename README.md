# AWS 3-Tier Architecture Project

## Overview

Designed and deployed a secure **3-Tier Architecture on AWS** using networking, compute, database, and load balancing services.  
This project demonstrates how real-world applications are structured using separate **Web, Application, and Database tiers** for better security, scalability, and maintainability.

---

# Architecture

```text
Internet User
      ↓
Application Load Balancer
      ↓
Web Tier (EC2 - Nginx)
      ↓
Application Tier (EC2 - Node.js / Backend API)
      ↓
Database Tier (Amazon RDS MySQL)
