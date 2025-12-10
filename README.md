<b> <h1> 🚀 DevSecOps CI/CD Pipeline with GitOps </b> </h1>
<div align="center">
Complete automated DevSecOps workflow deploying a URL Shortener microservice

View Infrastructure Repo - View CI Pipeline - View GitOps Repo

</div>
📋 What This Project Does
End-to-end automated DevSecOps pipeline that:

- Provisions AWS infrastructure (EKS, VPC, EC2) using Terraform.
- Builds and scans Docker images with integrated security tools (Trivy, Gitleaks, SonarQube).
- Deploys a URL Shortener microservice to Kubernetes via ArgoCD (GitOps).
- Implements full monitoring stack (Prometheus, Grafana, Loki).
- Auto-triggers downstream pipelines for complete workflow automation.

Main Application: 
A URL Shortener web service that converts long URLs into short, shareable links. Built with Node.js, SQLlite, containerized with Docker, and deployed on Kubernetes via ArgoCD.

🏗️ URL Shortener Webservice Architecture Flow 
```

┌──────────────────────────────────────────────────────────────────────┐
│                 COMPLETE INFRA + CI/CD GITOPS FLOW                   │
└──────────────────────────────────────────────────────────────────────┘

INFRASTRUCTURE LAYER (ONE-TIME / WHEN INFRA CHANGES)
----------------------------------------------------

           ┌──────────────── Infra Repo (GitHub) ────────────────┐
           │  (Terraform for AWS EKS + VPC + Nodes + IAM)        │
           └─────────────────────────────────────────────────────┘

        ┌─────────────┐        Webhook        ┌────────────────┐
        │  GitHub     │ ───────────────────▶ │    Jenkins     │
        │ (Infra IaC) │                       │ Infra Pipeline │
        └─────────────┘                       └────────────────┘
                                                      │
                                                      │  Terraform apply
                                                      ▼
                                              ┌───────────────┐
                                              │   AWS (EKS,   │
                                              │   VPC, Nodes) │
                                              └───────────────┘
                                                      │
                            After first successful Infra run
                                                      ▼
                                              ┌───────────────┐
                                              │ Install       │
                                              │ ArgoCD on EKS │
                                              └──────┬────────┘
                                                     ▼
                                       ┌────────────────────────┐
                                       │ ArgoCD Applications    │
                                       │ - URL Shortener        │
                                       │ - Monitoring stack     │
                                       │ - Security tools       │
                                       └──────────┬─────────────┘
                                                  ▼


CI / CD + SECURITY (ON EVERY APP CODE CHANGE)
---------------------------------------------

           ┌──────────────── CI Repo (GitHub) ──────────────────┐
           │ (URL Shortener source code + Jenkinsfile)          │
           └────────────────────────────────────────────────────┘

        ┌─────────────┐        Webhook        ┌───────────────┐
        │  GitHub     │ ───────────────────▶ │    Jenkins     │
        │ (App Code)  │                      │   CI Pipeline  │
        └─────────────┘                      └────────────────┘
                                                      │
                                                      │ build / test / package
                                                      ▼
                                           ┌────────────────────┐
                                           │ Security Scanning  │
                                           │ Gitleaks + Trivy + │
                                           │ SonarQube          │
                                           └─────────┬──────────┘
                                                     │  all checks pass
                                                     ▼
                                       ┌────────────────────────┐
                                       │ DockerHub (Images      │
                                       │ with auto tags)        │
                                       └──────────┬─────────────┘
                                                  │
                                                  │ Update image tag
                                                  │ in CD GitOps repo
                                                  ▼
                                       ┌────────────────────────┐
                                       │ ArgoCD CD Repo         │
                                       │ (ArgoCD-Pipeline       │
                                       │  manifests)            │
                                       └──────────┬─────────────┘
                                                  │
                                                  │ GitOps sync
                                                  ▼
                                       ┌────────────────────────┐
                                       │ EKS Cluster Runtime    │
                                       │ URL Shortener + Tools  │
                                       └──────────┬─────────────┘
                                                  ▼


OBSERVABILITY & MONITORING
--------------------------

                                       ┌─────────────────────────┐
                                       │ Monitoring Stack        │
                                       │ Prometheus + Grafana +  │
                                       │ Loki / Promtail         │
                                       └─────────────────────────┘


```

📦<h2> <b> Project Structure </h2> </b>

<h3> <b> 1. Infrastructure Pipeline </b> </h3>
Purpose: Automates AWS infrastructure provisioning with Terraform

Key Features:

- Provisions EKS cluster, VPC, EC2 instances on AWS.
- Modular Terraform architecture with S3 remote state.
- Integrated security scanning: Gitleaks (secrets), Trivy (IaC), SonarQube (code quality).
- Auto-triggers all downstream pipelines on success.

Tech Stack: 

- Terraform
- AWS (EKS/EC2/VPC)
- Jenkins
- Security Tools (Trivy, Gitleaks, SonarQube)



<h3> <b> 2. CI Pipeline </b> </h3>
Purpose: Builds and pushes URL Shortener Docker images

Key Features:

- Builds Docker image for URL Shortener application (Node.js + SQLlite).
- Trivy vulnerability scanning before image push.
- Multi-stage Docker builds for optimization.
- Pushes to Docker Hub/ECR.

Application: Full-stack URL Shortener with REST API

- Frontend: HTML/CSS/JavaScript
- Backend: Node.js + Express
- Database: SQLlite

Tech Stack: 

- Docker
- Node.js
- PostgreSQL
- Jenkins
- Trivy

<h3> <b> 3. GitOps Deployment Pipeline </b> </h3>
Purpose: Deploys applications using ArgoCD (GitOps approach)

Key Features:

- Installs ArgoCD on EKS cluster.
- Deploys URL Shortener service with auto-scaling (3-10 pods).
- Deploys monitoring stack: Prometheus, Grafana, Loki, Promtail.
- Deploys security tools: Trivy Operator, Gitleaks, SonarQube.

Components:

- ArgoCD Application: URL Shortener deployment + LoadBalancer service.
- Monitoring: Full observability with metrics, logs, and dashboards.
- Security: Runtime vulnerability scanning and code quality monitoring.

Tech Stack:

- ArgoCD
- Kubernetes
- Helm
- Prometheus
- Grafana
- Loki

🔄 Workflow Automation
```
1. Push to Infra GitHub repo → Jenkins triggers:
   └─ Infra Pipeline (provision / update EKS infrastructure)

2. Infra Pipeline:
   ├─ Runs Terraform to provision AWS EKS cluster, VPC, and node groups
   └─ Configures base IAM roles, networking, and access for ArgoCD and workloads

3. After Infra Pipeline succeeds (first run), Jenkins automatically runs:
   ├─ ArgoCD installation job on the EKS cluster
   ├─ ArgoCD Application for the URL Shortener (GitOps repo: ArgoCD-Pipeline)
   ├─ ArgoCD Application for the Monitoring stack (Prometheus + Grafana + Loki)
   └─ ArgoCD Application for Security tools (Gitleaks, Trivy, SonarQube)

4. Push to CI GitHub repo (URL Shortener source code) → GitHub Webhook triggers:
   └─ Jenkins CI Pipeline (build, security scans, and image push)

5. CI Pipeline:
   ├─ Builds the URL Shortener Docker image
   ├─ Runs Gitleaks on the repository, Trivy on the image, and SonarQube on the code
   ├─ Pushes the image to DockerHub with an auto-incremented tag
   └─ Updates the ArgoCD GitOps repo (ArgoCD-Pipeline) with the new image tag

6. ArgoCD watches the CD repo:
   └─ Detects the updated image tag, auto-syncs, and deploys the new version to EKS

7. Monitoring and observability:
   ├─ Prometheus scrapes metrics from the URL Shortener and Kubernetes components
   ├─ Grafana provides dashboards for application, cluster, and CI/CD metrics
   └─ Loki + Promtail collect logs for troubleshooting and performance analysis

8. From zero to full environment (EKS + URL Shortener + monitoring + security tools).

```
Result: Fully automated infrastructure → build → deploy → monitor workflow with security integrated at every stage.

🛠️ Tech Stack

- Infrastructure	-> Terraform, AWS (EKS, EC2, VPC, S3)
- CI/CD	-> Jenkins, Docker, Git
- GitOps	-> ArgoCD, Kubernetes, Helm
- Security	-> Trivy, Gitleaks, SonarQube
- Monitoring	-> Prometheus, Grafana, Loki, Promtail
- Application	-> Node.js, Express, SQLlite
  
🚀 Quick Start
bash
# Clone all repositories
git clone https://github.com/Ahmedlebshten/Jenkins-Pipeline-Build-Infra.git

git clone https://github.com/Ahmedlebshten/Jenkins-CI-Pipeline.git

git clone https://github.com/Ahmedlebshten/ArgoCD-Pipeline.git

Access deployed services:

bash
# URL Shortener
kubectl get svc url-shortener

🔒 Security Features
Security integrated at every pipeline stage:

Pre-deployment: 

- Gitleaks (secrets)
- Trivy (IaC)
- SonarQube (code quality)

Build time: Trivy Docker image scanning, multi-stage builds

Runtime: Trivy Operator, Network Policies, Pod Security Standards

📊 Monitoring:

- Prometheus: Collects metrics (requests/sec, response time, resource usage).
- Grafana: Pre-configured dashboards for URL Shortener, Kubernetes, and security.
- Loki: Centralized log aggregation from all pods.

💡 Skills Demonstrated:

- ✅ DevSecOps (security shift-left)
- ✅ Infrastructure as Code (Terraform)
- ✅ CI/CD Pipeline Automation (Jenkins)
- ✅ GitOps Deployment (ArgoCD)
- ✅ Container Orchestration (Kubernetes)
- ✅ Cloud Infrastructure (AWS EKS)
- ✅ Monitoring & Observability (Prometheus/Grafana)
- ✅ Security Integration (Trivy/Gitleaks/SonarQube)
- ✅ Microservices Architecture (URL Shortener)

📂 <b> <h3> Project Repositories: </b> </h3>

🔗 Infrastructure Pipeline
[github.com/Ahmedlebshten/Jenkins-Pipeline-Build-Infra](https://github.com/Ahmedlebshten/Jenkins-Pipeline-Build-Infra)

Terraform IaC for AWS EKS provisioning with security scanning

🔗 CI Pipeline
[github.com/Ahmedlebshten/Jenkins-CI-Pipeline](https://github.com/Ahmedlebshten/Jenkins-CI-Pipeline)

Docker build pipeline for URL Shortener application

🔗 GitOps Deployment
[github.com/Ahmedlebshten/ArgoCD-Pipeline](https://github.com/Ahmedlebshten/ArgoCD-Pipeline)

ArgoCD deployment manifests with monitoring and security tools

📬 Contact

- GitHub: [https://github.com/Ahmedlebshten]
- LinkedIn: [https://www.linkedin.com/in/ahmedlebshten]
- Email: [ahmedlebshtenlebshten@gmail.com]

<div align="center">
⭐ Star this project if you find it useful!
DevSecOps Pipeline - Production Ready - Fully Automated

</div>
