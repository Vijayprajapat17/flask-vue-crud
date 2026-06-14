# CI/CD Pipeline for Flask + Vue.js Application

## Overview

This project implements a CI/CD pipeline for the Flask-Vue CRUD application using Jenkins, Docker, Docker Compose, AWS EC2, and AWS Systems Manager (SSM).

The pipeline automates source code checkout, code quality analysis, Docker image creation, image publishing, and application deployment to AWS EC2.

A key requirement of this project is that server access is managed exclusively through AWS Systems Manager (SSM), with SSH access disabled.

---

# Technologies Used

* Jenkins Declarative Pipeline
* Flask
* Vue.js
* Docker
* Docker Compose
* AWS EC2 (Amazon Ubuntu:latest)
* AWS Systems Manager (SSM)
* Docker Hub
* SonarQube

---

# Project Structure

```text
flask-vue-crud/
│
├── client/
│   └── Dockerfile
│
├── server/
│   └── Dockerfile
│
├── docker-compose.yml
├── Jenkinsfile
└── README.md
```

---

# Jenkins Setup

### Required Plugins

* Git Plugin
* Pipeline Plugin
* Docker Pipeline Plugin
* SonarQube Scanner Plugin
* Credentials Binding Plugin

### SonarQube Configuration

Configure SonarQube in Jenkins under:

```text
Manage Jenkins → System → SonarQube Servers
```

Configure Sonar Scanner under:

```text
Manage Jenkins → Global Tool Configuration
```

Tool Name:

```text
sonarqube
```

---

# AWS Setup

An Amazon Ubuntu:latest EC2 instance was used as the deployment server.

### Installed Components

* Docker
* Docker Compose
* AWS SSM Agent

The EC2 instance is registered as a managed instance in AWS Systems Manager.

---

# IAM Configuration

The EC2 instance is attached to an IAM Role with the following policy:

```text
AmazonSSMManagedInstanceCore
```

This allows Systems Manager to manage the instance securely without SSH access.

---

# SSM Configuration

Verify that the SSM Agent is running:

```bash
sudo systemctl status amazon-ssm-agent
```

The instance should appear as **Online** in:

```text
AWS Systems Manager → Managed Instances
```

---

# Docker Configuration

The application consists of two services:

### Backend

* Flask API
* Port 5001

### Frontend

* Vue.js Application
* Port 5173

Docker Compose is used to manage both containers and their communication.

---

# Code Quality Validation

Code quality analysis is performed using **SonarQube** integrated with Jenkins.

The SonarQube stage analyzes the source code and helps identify:

* Bugs
* Code Smells
* Security Vulnerabilities
* Maintainability Issues
* Duplicated Code

The analysis is executed automatically during every pipeline run, providing continuous feedback on code quality before deployment.

---

# Pipeline Stages

The Jenkins pipeline includes the following stages:

### 1. Checkout

Clones the source code from GitHub.

### 2. Code Quality Analysis

Runs SonarQube analysis on the source code.

### 3. Docker Build

Builds Docker images for:

* Backend
* Frontend

### 4. Docker Login

Authenticates with Docker Hub using Jenkins credentials.

### 5. Push Images

Pushes Docker images to Docker Hub.

### 6. Deploy

Deploys the application using Docker Compose.

---

# Running the Pipeline

1. Create a Jenkins Pipeline Job.
2. Connect the GitHub repository.
3. Configure required credentials.
4. Select the Jenkinsfile from SCM.
5. Trigger the build.

The pipeline automatically builds, analyzes, publishes, and deploys the application.

---

# Required Credentials

### Docker Hub Credentials

Type:

```text
Username with Password
```

Credential ID:

```text
dockerhub-cred
```

### SonarQube Token

Configured within Jenkins and associated with the SonarQube server configuration.

---

# Environment Variables

Example environment variables used in the pipeline:

```text
DOCKER_USER=vijay171202
SCANNER_HOME=sonarqube
```

---

# Deployment Details

Deployment is performed on an Amazon Ubuntu:latest EC2 instance.

Docker Compose is used to manage application containers.

Deployment actions include:

* Pulling updated Docker images
* Stopping existing containers
* Starting updated containers
* Ensuring application availability after deployment

---

# Security Considerations

### Why SSH is Disabled

SSH access is disabled to reduce the attack surface and improve security.

Benefits include:

* No exposed port 22
* Centralized access management
* Improved auditability through AWS Systems Manager
* Reduced risk of unauthorized server access

All administrative operations are performed through AWS Systems Manager.

### IAM Permissions Used

The EC2 instance uses:

```text
AmazonSSMManagedInstanceCore
```

This policy enables secure remote management through Systems Manager.

---

# Conclusion

This project demonstrates a CI/CD implementation for a Flask and Vue.js application using Jenkins, Docker, Docker Compose, AWS EC2, and SonarQube. The solution automates the build, analysis, containerization, and deployment process while following secure deployment practices.
