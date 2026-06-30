# 🚀 Todo Application Deployment on AWS using ECR, ECS (Fargate) and CloudWatch

## Project Overview

This project demonstrates how to deploy a containerized Node.js Todo Application on AWS using Docker, Amazon ECR, Amazon ECS (Fargate), and Amazon CloudWatch.

The application source code is stored on an EC2 instance, containerized using Docker, pushed to Amazon ECR, and deployed on Amazon ECS using the Fargate launch type. Application logs are collected and monitored using Amazon CloudWatch.

---

# Project Scenario

A company wants to deploy a Todo Application on AWS without managing servers.

Requirements:

- Containerize the application.
- Store container images securely.
- Run containers without managing EC2 instances.
- Monitor application logs.
- Provide public access to the application.

To achieve this, we use:

```text
GitHub
   |
EC2 Instance
   |
Docker
   |
Amazon ECR
   |
Amazon ECS (Fargate)
   |
CloudWatch Logs
   |
End Users
```

---

# Tools and Technologies Used

| Tool / Service | Purpose |
|---------------|---------|
| Git | Clone the application source code |
| Docker | Containerize the application |
| AWS CLI | Interact with AWS services |
| Amazon EC2 | Build server for Docker image |
| AWS IAM | User authentication and permissions |
| Amazon ECR Public | Store Docker images |
| Amazon ECS | Run containerized applications |
| AWS Fargate | Serverless compute for ECS |
| Amazon CloudWatch | Monitor and collect logs |

---

# Architecture Diagram

```text
GitHub Repository
        |
        v
EC2 Instance
        |
        v
Docker Build
        |
        v
Amazon ECR
        |
        v
Amazon ECS (Fargate)
        |
        v
CloudWatch Logs
        |
        v
Users
```

---

# Deployment Phases

## Phase 1: Launch EC2 Instance and Automated Environment Setup

In this phase, we create an Ubuntu EC2 instance and use a User Data script to automatically install all required software and clone the application repository.

### Services Used

```text
Amazon EC2
Git
Docker
AWS CLI
```

### Create EC2 Instance

Navigate:

```text
Amazon EC2
  ↓
Launch Instance
```

### Instance Details

```text
Name: todo-app-server
AMI: Ubuntu Server
Instance Type: t2.micro
```

### Security Group

Allow the following ports:

```text
SSH (22)
HTTP (80)
Custom TCP (8000)
```

### User Data Script

Paste the following script into the User Data section while creating the EC2 instance:

```bash
#!/bin/bash

# Update packages
apt update -y

# Install Git and Unzip
apt install git unzip -y

# Install Docker
apt install docker.io -y
systemctl start docker
systemctl enable docker

# Add ubuntu user to docker group
usermod -aG docker ubuntu

# Install AWS CLI v2
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "/tmp/awscliv2.zip"

cd /tmp

unzip awscliv2.zip

./aws/install

# Clone Todo Application Repository
cd /home/ubuntu

git clone https://github.com/master-ankitt/todo-app-via-aws-services.git

# Set Ownership
chown -R ubuntu:ubuntu /home/ubuntu/todo-app-via-aws-services

# Success Log
echo "User data execution completed" > /var/log/userdata-success.log
```

### Verify Setup

```bash
docker --version
```

```bash
aws --version
```

```bash
git --version
```

```bash
cd ~/todo-app-via-aws-services
```

```bash
ls
```

---

## Phase 2: Create IAM User

...

## Phase 3: Configure AWS CLI

...

## Phase 4: Create Public ECR Repository

...

## Phase 5: Push Docker Image to ECR

...

## Phase 6: Create ECS Cluster

...

## Phase 7: Create Task Definition

...

## Phase 8: Run ECS Task

...

## Phase 9: Verify Application

...

## Phase 10: Monitor Logs using CloudWatch

...
