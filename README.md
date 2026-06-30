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

## 1. Git

Used to clone the application source code from GitHub.

### Purpose

```text
Source Code Management
Version Control
Repository Cloning
```

### Command Used

```bash
git clone https://github.com/master-ankitt/todo-app-via-aws-services.git
```

---

## 2. Docker

Docker is used to package the application and its dependencies into a container image.

### Purpose

```text
Containerization
Portable Deployments
Application Packaging
```

### Commands Used

```bash
docker build -t todo-app .
```

```bash
docker images
```

---

## 3. AWS CLI

AWS CLI is used to interact with AWS services directly from the EC2 instance.

### Purpose

```text
Authenticate with AWS
Push Images to ECR
Manage AWS Resources
```

### Commands Used

```bash
aws configure
```

```bash
aws sts get-caller-identity
```

---

## 4. Amazon EC2

Amazon EC2 provides a virtual machine where we build and manage the Docker image.

### Purpose

```text
Docker Host
AWS CLI Host
Build Environment
```

---

## 5. AWS IAM

IAM provides authentication and authorization for AWS services.

### Purpose

```text
User Management
Access Control
Permissions
```

### IAM User Created

```text
ankit
```

### Permissions Assigned

```text
AmazonElasticContainerRegistryPublicFullAccess
AmazonElasticContainerRegistryPublicPowerUser
AmazonElasticContainerRegistryPublicReadOnly
AmazonEC2FullAccess
```

---

## 6. Amazon ECR (Elastic Container Registry)

Amazon ECR stores Docker images.

### Purpose

```text
Container Image Repository
Image Version Management
Secure Storage
```

### Workflow

```text
Docker Image
      |
      v
Amazon ECR
```

---

## 7. Amazon ECS (Elastic Container Service)

Amazon ECS runs Docker containers.

### Purpose

```text
Container Orchestration
Container Management
Application Deployment
```

### Launch Type Used

```text
AWS Fargate
```

Reason:

```text
No Server Management
Automatic Scaling
Serverless Container Execution
```

---

## 8. AWS Fargate

Fargate is the compute engine used by ECS.

### Purpose

```text
Run Containers Without EC2
Serverless Compute
```

---

## 9. Amazon CloudWatch

CloudWatch collects application and container logs.

### Purpose

```text
Log Collection
Monitoring
Troubleshooting
```

### Example

```text
Container Logs
      |
      v
CloudWatch Logs
```

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

## Phase 1: Launch EC2 Instance

...
