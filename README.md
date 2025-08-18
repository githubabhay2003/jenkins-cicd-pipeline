# Project Title: End-to-End Automated CI/CD Pipeline for Web Application Deployment

**Date:** August 18, 2025  
**Version:** 1.0  

---

## 1. Project Overview / Summary

### Problem Statement
The traditional process for deploying web applications is often manual, slow, and prone to human error. This leads to inconsistent releases, increased downtime, and a slow feedback loop for developers. There is a critical need for a standardized, automated process to build, test, and deploy applications reliably and efficiently.

### Goals & Objectives
The primary goal of this project is to establish a robust, fully automated Continuous Integration and Continuous Deployment (CI/CD) pipeline.

**Key Objectives:**
- **Automate the Build Process:** Automatically compile source code and package the application into a portable container upon every code change.  
- **Standardize Environments:** Use containerization to ensure consistency between development, build, and production environments.  
- **Implement Continuous Integration (CI):** Integrate source code changes into a central repository, where automated builds are triggered.  
- **Implement Continuous Deployment (CD):** Automatically deploy every successful build to production without manual intervention.  
- **Reduce Deployment Time & Errors:** Decrease the time from code commit to production release and minimize failures caused by manual configuration mistakes.  

---

## 2. Scope of the Project

### In-Scope
- Setup and configuration of a Jenkins master-slave architecture on cloud infrastructure.  
- Development of a simple static web application.  
- Creation of a Dockerfile to containerize the web application.  
- Implementation of a CI pipeline job to:  
  - Pull code from GitHub.  
  - Build a Docker image.  
  - Push the image to Docker Hub.  
- Implementation of a CD pipeline job to:  
  - Pull the new Docker image.  
  - Deploy the image as a running container in production.  
- Full automation via GitHub webhook integration.  

### Out-of-Scope
- Automated testing stages (unit, integration, end-to-end).  
- Security scanning of code or container images (DevSecOps).  
- Infrastructure as Code (Terraform, Ansible).  
- Advanced deployment strategies (Blue-Green, Canary, A/B).  
- Monitoring, logging, and alerting.  
- High-availability/disaster recovery setups for Jenkins.  

### Target Users
- **Development Team:** Pushes code to GitHub to trigger automated deployments.  
- **DevOps Team:** Manages, maintains, and monitors the CI/CD pipeline.  

---

## 3. Technical Requirements

### Functional Requirements
- Automatically trigger builds on GitHub push to the main branch.  
- Build Docker image from repository Dockerfile.  
- Version Docker images (e.g., Jenkins build number) and push to Docker Hub.  
- Deploy the most recent Docker image to production automatically.  
- Ensure deployed app is accessible publicly via IP and port.  

### Non-Functional Requirements
- **Scalability:** Support distributed builds using master-slave Jenkins architecture.  
- **Reliability:** Pipeline must be idempotent with clear logs for troubleshooting.  
- **Security:** Manage credentials securely in Jenkins Credentials Manager.  
- **Maintainability:** Pipeline must be well-documented and easy to modify.  

---

## 4. Architecture & Design

### System Architecture
1. Developer pushes code to GitHub.  
2. GitHub Webhook notifies Jenkins Master.  
3. Jenkins Master triggers Build-Job on Build Slave.  
4. Build Slave checks out code, builds Docker image, pushes to Docker Hub.  
5. Jenkins Master triggers Deploy-Job on Production Slave.  
6. Production Slave pulls image and runs container → Live application available.  

### Technology Stack
- **CI/CD Orchestration:** Jenkins  
- **Containerization:** Docker  
- **SCM:** Git, GitHub  
- **Container Registry:** Docker Hub  
- **Cloud Provider:** AWS  
- **VMs:** EC2 Instances  
- **OS:** Amazon Linux 2  

---

## 5. Infrastructure & Deployment

### Cloud Setup
- **EC2 Instances:**  
  - 1x Jenkins Master  
  - 1x Build Slave  
  - 1x Production Slave  

- **Networking (AWS Security Groups):**  
  - Port 22 (SSH) → Admin access  
  - Port 8080 → Jenkins UI & webhook  
  - Port 8081 → Application traffic  

### CI/CD Pipeline
- Two Jenkins Freestyle jobs:  
  - **Build-Job:** Triggered by GitHub webhook → builds & pushes Docker image.  
  - **Deploy-Job:** Triggered downstream → pulls image & deploys container.  

---

## 6. Data & Security Considerations

- **Data Storage:**  
  - Code → GitHub  
  - Images → Docker Hub  
  - No persistent database  

- **Authentication & Access Control:**  
  - Jenkins master ↔ slave secured with SSH keys.  
  - Docker Hub credentials stored securely in Jenkins Credentials Manager.  

- **Compliance:**  
  - Educational project, no sensitive data, no compliance requirements.  

---

## 7. Project Timeline & Milestones

- **Day 1:** Infrastructure setup (EC2 + Jenkins master).  
- **Day 2:** Node integration (SSH between master & slaves).  
- **Day 3:** CI pipeline (Build-Job → Docker Hub).  
- **Day 4:** CD pipeline (Deploy-Job → production).  
- **Day 5:** Full automation (GitHub webhook integration).  

---

## 8. Risks & Mitigation Strategies

- **Risk:** Jenkins master unavailable.  
  - *Mitigation:* Regular backups to S3.  
- **Risk:** Vulnerable base Docker image.  
  - *Mitigation:* Future → integrate container scanning (Trivy, Snyk).  
- **Risk:** Build slave runs out of disk space.  
  - *Mitigation:* Add `docker system prune -f` post-build step.  

---

## 9. Success Criteria / KPIs
- **Zero-Touch Deployment:** Fully automated from `git push`.  
- **Lead Time:** < 5 minutes from commit → production.  
- **Deployment Frequency:** On-demand, frequent.  
- **Change Failure Rate:** < 5%, easily diagnosable via Jenkins logs.  

---

## 10. Team & Responsibilities

- **DevOps Engineer (Project Lead):**  
  - Design CI/CD architecture.  
  - Set up cloud infrastructure.  
  - Configure Jenkins jobs & security.  
  - Document the process.  

- **Developer:**  
  - Write application code (`index.html`).  
  - Define Dockerfile.  
  - Push changes to GitHub repo.  
