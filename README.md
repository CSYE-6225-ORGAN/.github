# Cloud-Native Web Application on AWS

## Overview

This project implements a **secure, scalable, and highly available cloud-native web application** on AWS using **Infrastructure as Code**, **immutable infrastructure**, and **automated CI/CD pipelines**.
It combines a Node.js REST API with AWS-managed services to demonstrate real-world **DevOps, Cloud, and Site Reliability Engineering practices**.

The architecture emphasizes **fault tolerance, security, automation, observability, and zero-downtime deployments**.

---

## High-Level Architecture

* **Custom AMIs** built with **Packer**
* **EC2 Auto Scaling Groups** across multiple Availability Zones
* **Application Load Balancer (ALB)** with HTTPS
* **AWS CodeDeploy** for blue/green-style rolling deployments
* **PostgreSQL (RDS)** for relational data
* **DynamoDB** for token storage with TTL
* **S3** for artifacts and profile image storage
* **SNS + Lambda** for asynchronous email verification
* **CloudWatch + StatsD** for monitoring and metrics
* **IAM with least privilege** and **KMS encryption**

---

## Technology Stack

### Cloud & Infrastructure

* AWS EC2, ALB, Auto Scaling Groups
* AWS RDS (PostgreSQL, Multi-AZ)
* DynamoDB (TTL-based token expiry)
* AWS S3 (artifact & image storage)
* AWS SNS, Lambda, SES
* AWS IAM, KMS, CloudWatch
* CloudFormation
* Packer (AMI creation)

### Application & Runtime

* Node.js (Express.js)
* PostgreSQL
* DynamoDB
* JWT-based authentication
* Bcrypt password hashing

### CI/CD & Automation

* AWS CodeDeploy
* GitHub Actions
* Jenkins (Groovy + Python jobs)
* Bash scripting
* Systemd services

### Observability & Logging

* Winston logging
* StatsD metrics
* CloudWatch Agent
* Log rotation & log segregation
* Fluent Bit (containerized log pipeline)

---

## Application Features

* User registration with email verification
* Secure authentication using Basic Auth + JWT
* Profile management (CRUD)
* Profile image upload/download via S3
* Token-based email verification with TTL
* Health check endpoint (`/healthz`)
* Automated scaling and self-healing infrastructure

---

## Infrastructure Workflow

### 1. AMI Creation (Packer)

* Builds a custom Amazon Linux AMI
* Pre-installs:

  * Node.js
  * PostgreSQL
  * CloudWatch Agent
  * CodeDeploy Agent
* Ensures **immutable and consistent EC2 instances**

### 2. Infrastructure Provisioning

* CloudFormation provisions:

  * VPC, subnets, route tables
  * Internet Gateway
  * Security Groups
  * ALB and Auto Scaling Groups
  * RDS, DynamoDB, IAM roles

### 3. Application Deployment

* CodeDeploy pulls artifacts from S3
* Lifecycle hooks:

  * `BeforeInstall`: stop app, clean dependencies
  * `AfterInstall`: install dependencies, configure CloudWatch
  * `ApplicationStart`: restart Node.js service
* Zero-downtime rolling deployments

---

## Node.js Service Management

* Application runs as a **systemd service**
* Automatically restarts on failure
* Starts on system boot
* Centralized logging via syslog and CloudWatch

---

## Monitoring & Logging

* CloudWatch Agent collects system and application metrics
* StatsD captures custom application metrics
* Structured logs using Winston
* Automated log rotation
* Keyword-based log segregation (ERROR, CRITICAL)
* Fluent Bit forwards logs to centralized streams

---

## Security Best Practices

* IAM roles with least privilege
* KMS encryption for EBS, RDS, and S3
* Private subnets for databases
* Token expiry using DynamoDB TTL
* Password hashing with bcrypt
* No credentials hardcoded in production workflows

---

## CI/CD Pipeline

* Code pushed to GitHub triggers GitHub Actions
* Artifacts uploaded to S3
* CodeDeploy deploys to Auto Scaling Group
* Health checks ensure successful rollout
* Automatic rollback on failure

---

## How to Run (High-Level)

1. Build AMI using Packer
2. Deploy infrastructure using CloudFormation
3. Push application code to GitHub
4. GitHub Actions triggers CodeDeploy
5. Application becomes available via ALB DNS

---

## Key Learning Outcomes

* Designing production-grade AWS architectures
* Implementing immutable infrastructure
* Automating deployments with zero downtime
* Building secure, event-driven microservices
* End-to-end observability and monitoring
* Real-world DevOps and SRE practices

---

## Author

**Aravind Polepeddi**
DevOps / Cloud Engineer
5+ years of experience in AWS, CI/CD, Automation, and Distributed Systems
