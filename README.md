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

In this phase, we create an IAM user that will be used to authenticate with AWS CLI and interact with Amazon ECR.

### Create IAM User

Navigate:

```text
AWS Console
  ↓
IAM
  ↓
Users
  ↓
Create User
```

### User Details

```text
User Name: ankit
```

### Assign Permissions

Attach the following permissions:

```text
AmazonElasticContainerRegistryPublicFullAccess
```

```text
AmazonElasticContainerRegistryPublicPowerUser
```

```text
AmazonElasticContainerRegistryPublicReadOnly
```

```text
AmazonEC2FullAccess
```

Click: Create User

---

## Phase 3: Configure AWS CLI

In this phase, we generate an Access Key and Secret Key for the IAM user and configure AWS CLI on the EC2 instance.

### Generate Access Key

Navigate:
```text
IAM
  ↓
Users
  ↓
ankit
  ↓
Security Credentials
  ↓
Create Access Key
```

Copy: Access Key ID and Secret Access Key
```

### Configure AWS CLI

Run:

```bash
aws configure
```


Provide:

```text
AWS Access Key ID
AWS Secret Access Key
ap-south-1
```

### Verify Configuration

```bash
aws sts get-caller-identity
```

Expected Output:

```text
AWS Account Details
IAM User Information
```

---

## Phase 4: Create Public ECR Repository

In this phase, we create a Public Amazon ECR repository that will store our Docker image.

### Create Repository

Navigate:

```text
Amazon ECR
  ↓
Public Registry
  ↓
Repositories
  ↓
Create Repository
```

### Repository Details

```
Repository Name: todo-app
Operating System: Linux
Architecture: x86-64
Click: Create Repository
```

---

## Phase 5: Build and Push Docker Image to ECR

In this phase, we build the Docker image and push it to Amazon ECR Public.

### Navigate to Application Directory

```bash
cd ~/todo-app-via-aws-services
```

### Open ECR Push Commands

Navigate:

```text
Amazon ECR
  ↓
Public Repository
  ↓
todo-app
  ↓
View Push Commands
```

### Login to ECR

```bash
sudo aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/q2k6c3h5
```

### Build Docker Image

```bash
sudo docker build -t todo-app .
```

### Verify Image

```bash
docker images
```

### Tag Docker Image

```bash
sudo docker tag todo-app:latest public.ecr.aws/q2k6c3h5/todo-app:latest
```

### Push Docker Image

```bash
sudo docker push public.ecr.aws/q2k6c3h5/todo-app:latest
```

### Verify Image Upload

Navigate:

```text
Amazon ECR
  ↓
Public Repositories
  ↓
todo-app
```

Verify:

```text
Image Successfully Uploaded
```

---

## Phase 6: Create ECS Cluster

In this phase, we create an ECS cluster using AWS Fargate.

### Create Cluster

Navigate:

```text
Amazon ECS
  ↓
Clusters
  ↓
Create Cluster
```

### Cluster Configuration

```text
Cluster Name: todo-app-cluster
Compute Capacity: Fargate Only
```

### Monitoring

```text
Container Insights with Enhanced Observability
```

Click: Create

---

## Phase 7: Create ECS Task Definition

In this phase, we define how our container should run inside ECS.

### Create Task Definition

Navigate:

```text
Amazon ECS
  ↓
Task Definitions
  ↓
Create Task Definition
```

### General Configuration

```text
Task Definition Name: todo-task-1
Launch Type: AWS Fargate
Operating System: Linux
CPU Architecture: x86-64
```

### Container Configuration

Container Name: todo-app
Image URI: public.ecr.aws/q2k6c3h5/todo-app:latest


### Port Mapping
```text
Container Port: 8000
Protocol: TCP
```

### Log Collection
```text
Amazon CloudWatch Logs
```

Click: Create
---

## Phase 8: Run ECS Task

--> In this phase, we launch our application container using the ECS Task Definition.

### Run New Task

Navigate:

```text
Amazon ECS
  ↓
Clusters
  ↓
todo-app-cluster
```

Scroll Down: Tasks
Click: Run New Task

### Select Task Definition

```text
todo-task-1
```

### Networking Configuration
Public IP: Enabled
Security Group: Allow TCP Port 8000
Click: Create

---

## Phase 9: Verify Application

--> In this phase, we verify that the application is running successfully.

### Get Public IP

Navigate:

```text
Amazon ECS
  ↓
Clusters
  ↓
todo-app-cluster
  ↓
Tasks
  ↓
Running Task
```

Copy:

```text
Public IP Address
```

### Access Application

Open Browser:

```text
http://<PUBLIC-IP>:8000
```

Example:

```text
http://13.232.xxx.xxx:8000
```

Expected Result:

```text
Todo Application Home Page
```

---

## Phase 10: Monitor Logs Using CloudWatch

--> In this phase, we verify application logs generated by ECS.

### View Logs

Navigate:

```text
Amazon CloudWatch
  ↓
Log Groups
```

Select:

```text
ECS Log Group
```

Verify:
Container Startup Logs
Application Logs
Error Logs

---

# Project Outcome

✅ EC2 Instance Provisioned

✅ Docker Installed Using User Data

✅ AWS CLI Configured

✅ IAM User Created

✅ Docker Image Built

✅ Docker Image Stored in Amazon ECR

✅ ECS Cluster Created

✅ ECS Task Definition Created

✅ Application Running on AWS Fargate

✅ Logs Available in CloudWatch

✅ Todo Application Accessible Through Public IP
---

