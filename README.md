# DevSecOps – Secure CI/CD Pipeline with EKS, Jenkins, SonarQube, Trivy, and Slack Integration

This project demonstrates the implementation of a complete DevSecOps pipeline for a three-tier web application using modern cloud-native tools. It follows best practices in infrastructure automation, security scanning, CI/CD, monitoring, and alerting.

GitHub Repository: [SuchanMadhikarmi/devsecopsProject](https://github.com/SuchanMadhikarmi/devsecopsProject)

## Tech Stack

- Application Stack: Node.js (frontend & backend), MySQL  
- Cloud Provider: AWS (EKS, EC2, VPC, IAM)  
- CI/CD: Jenkins with Webhooks  
- Security Tools: SonarQube, Gitleaks, Trivy  
- Monitoring: Prometheus, Grafana  
- Notifications: Slack integration  
- Certificate Management: Cert Manager + Ingress  
- Domain & DNS: GoDaddy with TLS setup  

## Step-by-Step Implementation

### 1. Local Testing and Setup

- Cloned a reference GitHub repo and verified the application works locally.
- Installed dependencies using `package.json` and `package-lock.json`.
- Started a local MySQL instance and tested both frontend and backend.

### 2. GitHub Organization & Access Management

- Created a GitHub Organization and added members:
  - Admin: full access  
  - Developer: write access  
  - Reviewer: triage access  
- Configured branch protection rules for `main`.

### 3. Infrastructure Setup with Terraform

- Created an EC2 control server for orchestration.
- Installed `awscli`, `kubectl`, `terraform`, and configured credentials.
- Provisioned AWS resources:
  - EKS Cluster  
  - VPC, subnets, gateways, route tables  
  - IAM roles and OIDC provider  
- Configured `kubeconfig` and verified cluster access.

### 4. Kubernetes Setup (EKS)

- Applied Kubernetes manifests to:
  - Configure RBAC (ServiceAccount `jenkins`, RoleBindings, ClusterRoleBindings)  
  - Deploy EBS CSI driver  
  - Install Ingress Controller and Cert Manager  
  - Configure EBS volumes, TLS certificates, and secrets  

### 5. CI/CD Pipeline with Jenkins

#### Jenkins Setup

- Launched EC2 instance and installed Jenkins.
- Installed plugins:
  - Docker, Kubernetes, SonarQube Scanner, NodeJS, Stage View, etc.  
- Integrated SonarQube and DockerHub credentials.

#### Security Tools Setup

- Installed and configured Trivy and Gitleaks on Jenkins EC2.

#### Jenkins Pipeline Workflow

1. Git Checkout  
2. Gitleaks secret scan  
3. SonarQube static analysis  
4. Quality gate check  
5. Docker build (frontend & backend)  
6. Trivy image scan before DockerHub push  
7. Filesystem Trivy scan for dependency vulnerabilities  
8. Push images to DockerHub  
9. Manual approval for production deployment  
10. Kubernetes deployment using `k8s-prod/` manifests  

### 6. Webhook Configuration

- Installed Generic Webhook Trigger plugin.
- Connected GitHub repo to Jenkins via webhook.
- Pipeline triggers on every push.

### 7. Domain & TLS Setup

- Configured Ingress NGINX and Cert Manager for TLS.
- Used GoDaddy to:
  - Add A and CNAME records pointing to LoadBalancer IP  
  - Connect domain with TLS-enabled app endpoint  

### 8. Monitoring with Prometheus & Grafana

- Installed Helm and added Prometheus community chart.
- Deployed monitoring stack:
  - Prometheus  
  - Grafana  
  - kube-state-metrics  
- Exposed key components as LoadBalancer services.

### 9. Slack Notification Integration

- Configured Slack Webhook URL and OAuth token in Jenkins.
- Slack plugin sends pipeline success/failure notifications to a channel.

## Final Outcome

- Full CI/CD pipeline from Git push to production deployment  
- Integrated security at every stage (code + image scanning)  
- Complete observability using Grafana dashboards  
- Automated alerts and domain-level access with HTTPS  

## Lessons Learned

- Infrastructure provisioning with Terraform and EKS  
- Jenkins pipeline design and security scanner integration  
- Helm-based deployment and monitoring  
- Kubernetes RBAC and service account best practices  
- DNS & TLS setup using Ingress and external providers  
