# AWS EKS DevOps Platform

[![Terraform](https://img.shields.io/badge/Terraform-v1.6+-623CE4?logo=terraform)](https://terraform.io)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-v1.29-326CE5?logo=kubernetes)](https://kubernetes.io)
[![AWS](https://img.shields.io/badge/AWS-EKS-FF9900?logo=amazonaws)](https://aws.amazon.com/eks/)
[![Python](https://img.shields.io/badge/Python-3.11-3776AB?logo=python)](https://python.org)

> **Production-Grade Microservices Platform on AWS EKS using Terraform, GitOps & Jenkins CI/CD**

A complete Kubernetes platform on AWS EKS featuring Infrastructure as Code, GitOps-based continuous deployment with ArgoCD, Jenkins CI/CD pipelines, comprehensive observability with Prometheus/Grafana, and enterprise security patterns.

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              AWS Cloud (ap-south-1)                          │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │                         VPC (10.0.0.0/16)                              │  │
│  │  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐        │  │
│  │  │   AZ-1 (a)      │  │   AZ-2 (b)      │  │   AZ-3 (c)      │        │  │
│  │  │ ┌─────────────┐ │  │ ┌─────────────┐ │  │ ┌─────────────┐ │        │  │
│  │  │ │Public Subnet│ │  │ │Public Subnet│ │  │ │Public Subnet│ │        │  │
│  │  │ │  (NAT GW)   │ │  │ │  (NAT GW)   │ │  │ │  (NAT GW)   │ │        │  │
│  │  │ └─────────────┘ │  │ └─────────────┘ │  │ └─────────────┘ │        │  │
│  │  │ ┌─────────────┐ │  │ ┌─────────────┐ │  │ ┌─────────────┐ │        │  │
│  │  │ │Private Sub  │ │  │ │Private Sub  │ │  │ │Private Sub  │ │        │  │
│  │  │ │(EKS Nodes)  │ │  │ │(EKS Nodes)  │ │  │ │(EKS Nodes)  │ │        │  │
│  │  │ └─────────────┘ │  │ └─────────────┘ │  │ └─────────────┘ │        │  │
│  │  │ ┌─────────────┐ │  │ ┌─────────────┐ │  │ ┌─────────────┐ │        │  │
│  │  │ │  DB Subnet  │ │  │ │  DB Subnet  │ │  │ │  DB Subnet  │ │        │  │
│  │  │ │   (RDS)     │ │  │ │   (RDS)     │ │  │ │   (RDS)     │ │        │  │
│  │  │ └─────────────┘ │  │ └─────────────┘ │  │ └─────────────┘ │        │  │
│  │  └─────────────────┘  └─────────────────┘  └─────────────────┘        │  │
│  │                                                                        │  │
│  │  ┌──────────────────────────────────────────────────────────────────┐ │  │
│  │  │                    EKS Cluster (v1.29)                           │ │  │
│  │  │  ┌────────────┐ ┌────────────┐ ┌────────────┐ ┌────────────┐    │ │  │
│  │  │  │  ArgoCD    │ │  Jenkins   │ │ Prometheus │ │  Grafana   │    │ │  │
│  │  │  └────────────┘ └────────────┘ └────────────┘ └────────────┘    │ │  │
│  │  │  ┌────────────┐ ┌────────────┐ ┌────────────┐ ┌────────────┐    │ │  │
│  │  │  │ User Svc   │ │ Order Svc  │ │Product Svc │ │ALB Ingress │    │ │  │
│  │  │  │  (FastAPI) │ │  (FastAPI) │ │  (FastAPI) │ │ Controller │    │ │  │
│  │  │  └────────────┘ └────────────┘ └────────────┘ └────────────┘    │ │  │
│  │  └──────────────────────────────────────────────────────────────────┘ │  │
│  │                                                                        │  │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐   │  │
│  │  │    RDS      │  │     S3      │  │    ECR      │  │  Secrets    │   │  │
│  │  │(PostgreSQL) │  │ (Artifacts) │  │  (Images)   │  │  Manager    │   │  │
│  │  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘   │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 🚀 Features

### Infrastructure (Terraform)
- **Multi-AZ VPC** with public, private, and database subnets
- **EKS Cluster v1.29** with managed node groups and autoscaling
- **RDS PostgreSQL 16.6** with Multi-AZ deployment
- **S3 Buckets** for artifacts with lifecycle policies
- **ECR Repositories** for container images
- **IAM Roles for Service Accounts (IRSA)** for secure AWS access
- **AWS Secrets Manager** integration
- **EBS CSI Driver** for persistent storage

### Kubernetes Platform
- **AWS ALB Ingress Controller** for load balancing
- **Cluster Autoscaler** for dynamic node scaling
- **Horizontal Pod Autoscaler (HPA)** for workload scaling
- **Network Policies** for traffic control
- **Pod Disruption Budgets** for high availability

### CI/CD Pipeline
- **Jenkins** (Primary) - CI pipelines with Kubernetes agents
- **GitHub Actions** (Backup) - Automated workflows as fallback
- **ArgoCD** - GitOps-based continuous deployment
- **Helm Charts** - Templated Kubernetes deployments

### Observability
- **Prometheus** for metrics collection
- **Grafana** with pre-configured dashboards
- **AlertManager** for alerting
- **Node Exporter** for host metrics

### Microservices (Python FastAPI)
- **User Service** - User management and authentication
- **Order Service** - Order processing
- **Product Service** - Product catalog management

---

## 📁 Project Structure

```
aws-eks-devops-platform/
├── terraform/                    # Infrastructure as Code
│   ├── environments/
│   │   └── dev/                  # Dev environment configuration
│   ├── modules/
│   │   ├── vpc/                  # VPC with multi-AZ subnets
│   │   ├── eks/                  # EKS cluster with node groups
│   │   ├── rds/                  # PostgreSQL RDS instance
│   │   ├── s3/                   # S3 buckets with policies
│   │   └── iam/                  # IAM roles and policies
│   └── backend.tf
├── kubernetes/                   # Kubernetes manifests
│   ├── base/
│   │   ├── namespaces/
│   │   ├── network-policies/
│   │   └── rbac/
│   ├── apps/                     # Microservice deployments
│   ├── platform/                 # Platform components
│   └── monitoring/               # Prometheus & Grafana
├── argocd/                       # ArgoCD configurations
│   ├── apps/                     # Application definitions
│   └── projects/                 # Project configurations
├── apps/                         # Microservices source code
│   ├── user-service/             # FastAPI user service
│   ├── order-service/            # FastAPI order service
│   └── product-service/          # FastAPI product service
├── helm/                         # Helm charts and values
│   ├── charts/
│   │   └── microservice/         # Generic reusable chart
│   └── values/                   # Service & platform values
├── jenkins/                      # Jenkins configurations
│   ├── Jenkinsfile.terraform
│   └── vars/
├── .github/workflows/            # GitHub Actions (backup CI)
├── scripts/                      # Utility scripts
└── docs/                         # Documentation
```

---

## 🛠️ Prerequisites

| Tool | Version | Purpose |
|------|---------|---------|
| AWS CLI | v2+ | AWS resource management |
| Terraform | >= 1.6 | Infrastructure provisioning |
| kubectl | >= 1.29 | Kubernetes management |
| Helm | >= 3.12 | Package management |
| Git | Latest | Version control |

### AWS Permissions Required
- EC2, VPC, EKS (Full Access)
- RDS, S3, ECR (Full Access)
- IAM (Create Roles/Policies)
- Secrets Manager
- Elastic Load Balancing

---

## 🚀 Deployment Guide

### Step 1: Clone the Repository
```bash
git clone https://github.com/AnishKini007/aws-eks-devops-platform.git
cd aws-eks-devops-platform
```

### Step 2: Configure AWS Credentials
```powershell
# Windows PowerShell
$env:AWS_ACCESS_KEY_ID="your-access-key"
$env:AWS_SECRET_ACCESS_KEY="your-secret-key"
$env:AWS_DEFAULT_REGION="ap-south-1"

# Or use AWS CLI
aws configure
```

### Step 3: Deploy Infrastructure with Terraform
```bash
cd terraform/environments/dev
terraform init
terraform plan
terraform apply -auto-approve
```

### Step 4: Configure kubectl
```bash
aws eks update-kubeconfig --name eks-platform-dev --region ap-south-1
kubectl get nodes
```

### Step 5: Install EBS CSI Driver
```bash
# Get OIDC ID
OIDC_ID=$(aws eks describe-cluster --name eks-platform-dev --region ap-south-1 \
  --query "cluster.identity.oidc.issuer" --output text | sed 's|https://||')

# Create IAM Role for EBS CSI
aws iam create-role --role-name eks-platform-dev-ebs-csi-driver \
  --assume-role-policy-document file://ebs-trust-policy.json

aws iam attach-role-policy --role-name eks-platform-dev-ebs-csi-driver \
  --policy-arn arn:aws:iam::aws:policy/service-role/AmazonEBSCSIDriverPolicy

# Install EBS CSI Driver addon
aws eks create-addon --cluster-name eks-platform-dev --addon-name aws-ebs-csi-driver \
  --region ap-south-1 \
  --service-account-role-arn arn:aws:iam::YOUR_ACCOUNT_ID:role/eks-platform-dev-ebs-csi-driver
```

### Step 6: Deploy ArgoCD
```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# Get ArgoCD admin password
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

### Step 7: Deploy Applications via ArgoCD
```bash
kubectl apply -f argocd/projects/
kubectl apply -f argocd/apps/
```

---

## 🔑 Access Credentials

### Get Credentials
```powershell
# ArgoCD Password
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | 
  ForEach-Object { [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($_)) }

# Jenkins Password
kubectl get secret -n jenkins jenkins -o jsonpath="{.data.jenkins-admin-password}" | 
  ForEach-Object { [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($_)) }

# Grafana Password (randomly generated by the chart)
kubectl get secret -n monitoring monitoring-grafana -o jsonpath="{.data.admin-password}" | 
  ForEach-Object { [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($_)) }
```

### Port Forwarding
```bash
# ArgoCD UI
kubectl port-forward -n argocd svc/argocd-server 8081:443

# Jenkins
kubectl port-forward -n jenkins svc/jenkins 8080:8080

# Grafana
kubectl port-forward -n monitoring svc/monitoring-grafana 3000:80

# Prometheus
kubectl port-forward -n monitoring svc/monitoring-kube-prometheus-prometheus 9090:9090

# Microservices
kubectl port-forward -n microservices svc/user-service 8001:80
kubectl port-forward -n microservices svc/order-service 8002:80
kubectl port-forward -n microservices svc/product-service 8003:80
```

### Access URLs (via port-forward)
| Service | URL | Username | Password |
|---------|-----|----------|----------|
| ArgoCD | https://localhost:8081 | admin | (from secret) |
| Jenkins | http://localhost:8080 | admin | (from secret) |
| Grafana | http://localhost:3000 | admin | (from secret) |
| Prometheus | http://localhost:9090 | - | - |
| User Service | http://localhost:8001 | - | - |
| Order Service | http://localhost:8002 | - | - |
| Product Service | http://localhost:8003 | - | - |

---

## 🐛 Troubleshooting Guide

### Issues Encountered & Solutions

#### 1. EKS Version Jump Error
**Error:** `Cannot upgrade more than one minor version at a time`

**Solution:** EKS only allows upgrading one minor version at a time (e.g., 1.28 → 1.29, not 1.28 → 1.30)
```hcl
# terraform/environments/dev/main.tf
cluster_version = "1.29"  # Not 1.30
```

#### 2. PostgreSQL Version Not Available
**Error:** `InvalidParameterValue: Cannot find version 16.3 for postgres`

**Solution:** Check available versions in your region and use an available one
```bash
aws rds describe-db-engine-versions --engine postgres --query 'DBEngineVersions[].EngineVersion' --region ap-south-1
```
```hcl
# Use available version
engine_version = "16.6"
```

#### 3. RDS Parameter Group Family Mismatch
**Error:** `InvalidParameterCombination: RDS does not support creating a DB instance with the following combination: DBInstanceClass=db.t3.micro, Engine=postgres, EngineVersion=16.6, DBParameterGroupFamily=postgres15`

**Solution:** Parameter group family must match engine version
```hcl
# terraform/modules/rds/main.tf
resource "aws_db_parameter_group" "main" {
  family = "postgres16"  # Match engine version 16.x
}
```

#### 4. S3 Lifecycle Rule Filter Warning
**Error:** `lifecycle rule filter must be set`

**Solution:** Add empty filter blocks to lifecycle rules
```hcl
lifecycle_rule {
  id      = "cleanup"
  enabled = true
  filter {}  # Required even for "apply to all" rules
  # ...
}
```

#### 5. EBS CSI Driver CrashLoopBackOff
**Error:** `Could not assume role: WebIdentityErr: failed to retrieve credentials`

**Solution:** Create proper IAM role with OIDC trust policy
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": {
      "Federated": "arn:aws:iam::ACCOUNT_ID:oidc-provider/oidc.eks.REGION.amazonaws.com/id/OIDC_ID"
    },
    "Action": "sts:AssumeRoleWithWebIdentity",
    "Condition": {
      "StringEquals": {
        "oidc.eks.REGION.amazonaws.com/id/OIDC_ID:aud": "sts.amazonaws.com",
        "oidc.eks.REGION.amazonaws.com/id/OIDC_ID:sub": "system:serviceaccount:kube-system:ebs-csi-controller-sa"
      }
    }
  }]
}
```

#### 6. Grafana CrashLoopBackOff - Datasource Error
**Error:** `Only one datasource per organization can be marked as default`

**Solution:** Add deleteDatasources section to monitoring.yaml
```yaml
grafana:
  deleteDatasources:
    - name: Prometheus
      orgId: 1
  additionalDataSources:
    - name: Prometheus
      type: prometheus
      url: http://monitoring-kube-prometheus-prometheus:9090
      isDefault: true
```

#### 7. Jenkins CrashLoopBackOff - Configuration Error
**Error:** `No hudson.security.AuthorizationStrategy implementation found for globalMatrix`

**Solution:** Use built-in authorization strategy or add matrix-auth plugin
```yaml
# helm/values/jenkins.yaml
JCasC:
  configScripts:
    security: |
      jenkins:
        authorizationStrategy:
          loggedInUsersCanDoAnything:
            allowAnonymousRead: false
installPlugins:
  - matrix-auth:latest  # If using globalMatrix
```

#### 8. ArgoCD Application Sync Failed - PodDisruptionBudget
**Error:** `Resource policy/PodDisruptionBudget is not permitted in project`

**Solution:** Add PDB to project's allowed resources
```yaml
# argocd/projects/microservices-project.yaml
spec:
  namespaceResourceWhitelist:
    - group: 'policy'
      kind: 'PodDisruptionBudget'
```

#### 9. Port Already in Use
**Error:** `bind: Only one usage of each socket address`

**Solution:** Use a different port
```bash
kubectl port-forward -n microservices svc/user-service 8001:80  # Use 8001 instead of 8000
```

#### 10. Region Permission Issues
**Error:** `UnauthorizedOperation` or `AccessDenied` for certain resources

**Solution:** Switch to a region where you have full permissions
```hcl
# terraform/environments/dev/main.tf
provider "aws" {
  region = "ap-south-1"  # Use region with proper permissions
}
```

---

## 📊 Monitoring & Observability

### Pre-configured Grafana Dashboards
- Kubernetes Cluster Overview
- Node Exporter Full
- Kubernetes Pod Metrics
- Application Metrics (via Prometheus)

### Prometheus Targets
- kube-state-metrics
- node-exporter
- kubelet
- apiserver
- Microservices (via ServiceMonitor)

### Alerting Rules
- High CPU/Memory Usage
- Pod CrashLooping
- Node Not Ready
- PVC Pending

---

## 🔒 Security Features

- **IRSA** - Service accounts with IAM roles
- **Network Policies** - Pod-level traffic control
- **Pod Security Standards** - Restricted security context
- **Secrets Encryption** - AWS Secrets Manager integration
- **Private ECR** - Container images in private registry
- **RBAC** - Role-based access control

---

## 🧹 Cleanup

### Destroy All Resources
```bash
# Delete ArgoCD applications first
kubectl delete -f argocd/apps/
kubectl delete -f argocd/projects/

# Delete ArgoCD
kubectl delete namespace argocd

# Destroy Terraform infrastructure
cd terraform/environments/dev
terraform destroy -auto-approve
```

### Delete EBS CSI Driver Role
```bash
aws iam detach-role-policy --role-name eks-platform-dev-ebs-csi-driver \
  --policy-arn arn:aws:iam::aws:policy/service-role/AmazonEBSCSIDriverPolicy
aws iam delete-role --role-name eks-platform-dev-ebs-csi-driver
```

---

## 📈 Cost Optimization Tips

1. Use **Spot Instances** for non-critical workloads
2. Enable **Cluster Autoscaler** to scale down during low usage
3. Use **t3.medium** or smaller for dev environments
4. Set **RDS** to single-AZ for dev
5. Use **S3 Lifecycle Policies** to clean up old artifacts

---

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 👤 Author

**Anish Kini**
- GitHub: [@AnishKini007](https://github.com/AnishKini007)

---

## 🙏 Acknowledgments

- AWS EKS Documentation
- Terraform AWS Provider
- ArgoCD Project
- Jenkins Helm Chart
- kube-prometheus-stack

---

## 📝 Resume Highlights

This project demonstrates proficiency in:

| Category | Technologies |
|----------|-------------|
| **Cloud Infrastructure** | AWS VPC, EKS, RDS, S3, ECR, IAM, Secrets Manager, ELB |
| **Infrastructure as Code** | Terraform modules, state management, multi-environment |
| **Container Orchestration** | Kubernetes, Helm charts, HPA, Cluster Autoscaler, PDB |
| **CI/CD** | Jenkins pipelines, ArgoCD GitOps, GitHub Actions |
| **Observability** | Prometheus, Grafana, AlertManager, metrics collection |
| **Security** | IRSA, Network Policies, RBAC, Secrets Management |
| **Microservices** | Python FastAPI, REST APIs, Health checks |
| **DevOps Practices** | GitOps, IaC, Automated deployments, Multi-AZ HA |
