🚀 DevSecOps CI/CD Pipeline with GitOps
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

🏗️ URL Shortener web service Architecture Flow
text
┌─────────────────────────────────────────────────────────────────┐
│                     COMPLETE INFRASTRUCTURE PIPELINE FLOW       |              
└─────────────────────────────────────────────────────────────────┘

  ┌─────────────┐         ┌─────────────┐         ┌─────────────┐
  │   GitHub    │───────▶ │   Jenkins   │───────▶│     AWS     │
  │  (Source)   │         │  (Trigger)  │         │    (Infra)  │
  └─────────────┘         └─────────────┘         └─────────────┘
                                │
                    ┌───────────┴───────────┐
                    ▼                       ▼
            ┌───────────────┐      ┌───────────────┐
            │ CI Pipeline   │      │  Security     │
            │ (Build/Push)  │      │  Scanning     │
            └───────────────┘      └───────────────┘
                    │                       │
                    └───────────┬───────────┘
                                ▼
                    ┌───────────────────────┐
                    │   ArgoCD (GitOps)     │
                    │  K8s Deployment       │
                    └───────────────────────┘
                                ▼
                    ┌───────────────────────┐
                    │   Monitoring Stack    │
                    │ Prometheus + Grafana  │
                    └───────────────────────┘

📦 Project Structure

🔗 <h2> <b> 1. Infrastructure Pipeline </b> </h2>
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



🔗 <h3> <b> 2. CI Pipeline </b> </h3>
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

🔗 <h3> <b> 3. GitOps Deployment Pipeline </b> </h3>
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
text
1. Push to GitHub → Jenkins triggers Infrastructure Pipeline
2. Terraform provisions AWS EKS cluster
3. Security scans: Gitleaks + Trivy + SonarQube ✓
4. Infrastructure succeeds → Auto-triggers:
   ├─ Install ArgoCD on EKS
   ├─ Deploy URL Shortener application
   ├─ Deploy Monitoring stack
   └─ Deploy Security tools
5. Complete environment ready in ~15 minutes
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

# Configure Jenkins with AWS credentials
# Create 6 Jenkins pipelines (one per Jenkinsfile)
# Run Infrastructure-Pipeline → Everything deploys automatically
Access deployed services:

bash
# URL Shortener
kubectl get svc url-shortener

# ArgoCD UI
kubectl port-forward svc/argocd-server -n argocd 9000:443

# Grafana
kubectl port-forward svc/grafana -n monitoring 3000:80
🔒 Security Features
Security integrated at every pipeline stage:

Pre-deployment: 

- Gitleaks (secrets)
- Trivy (IaC)
- SonarQube (code quality)

Build time: Trivy Docker image scanning, multi-stage builds

Runtime: Trivy Operator, RBAC, Network Policies, Pod Security Standards

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

📂 Project Repositories:

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
