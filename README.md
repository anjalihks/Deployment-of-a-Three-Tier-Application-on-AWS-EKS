# 🚀 Production-Grade 3-Tier Application on AWS EKS

## 📌 Project Overview

This project demonstrates how to deploy a **production-ready 3-tier web application** on AWS using:

- Infrastructure as Code (Terraform)
- Containerization (Docker)
- Kubernetes Orchestration (Amazon EKS)
- Secure Networking (VPC, Private Subnets, NAT)
- Load Balancing (AWS ALB)
- Auto Scaling (Cluster Autoscaler)
- Observability (Prometheus)

The application is a simple **To-Do App** built with:

- **Frontend**: React
- **Backend**: Node.js + Express
- **Database**: MongoDB

The focus of this project is not the application itself, but the **production-grade infrastructure design and deployment strategy**.

---

# 🏗️ Architecture Overview

```
User (Browser)
      ↓
DNS (Route 53)
      ↓
AWS ALB (Public Subnet)
      ↓
Kubernetes Ingress
      ↓
Kubernetes Services
      ↓
Pods (Frontend / Backend)
      ↓
MongoDB (Private Subnet)
```

---

# 🌍 High-Level AWS Architecture

```
VPC (10.0.0.0/16)
│
├── Public Subnets
│     └── Application Load Balancer (ALB)
│
├── Private Subnets
│     ├── EKS Worker Nodes
│     │     ├── Frontend Pods
│     │     ├── Backend Pods
│     │     └── MongoDB Pod
│     │
│     └── NAT Gateway (Outbound Internet Access)
│
└── EKS Control Plane (Managed by AWS)
```

---

# 🔧 Tech Stack

## ☁️ Cloud & Infrastructure
- AWS
- Amazon EKS
- VPC (Public & Private Subnets)
- NAT Gateway
- IAM & IRSA
- Application Load Balancer (ALB)

## 📦 DevOps & Automation
- Terraform (Infrastructure as Code)
- Docker (Containerization)
- Kubernetes (Container Orchestration)
- Helm (Package Management)
- AWS Load Balancer Controller
- Cluster Autoscaler

## 📊 Observability
- Prometheus
- Alertmanager

---

# 🧱 3-Tier Application Structure

## 🟢 Frontend (React)
- Calls backend via REST API
- Uses environment variable for backend URL
- Containerized with Docker
- Exposed via Kubernetes Service + Ingress

## 🔵 Backend (Node.js + Express)
- REST API for CRUD operations
- Uses Mongoose for MongoDB connection
- Implements:
  - Add Task
  - Get Tasks
  - Update Task
  - Delete Task
- Uses Kubernetes Secrets for DB credentials
- Includes Liveness & Readiness Probes

## 🟡 Database (MongoDB)
- Deployed inside Kubernetes
- Exposed internally via ClusterIP Service
- Credentials managed using Kubernetes Secrets

---

# 🔐 Security Architecture

- Worker nodes deployed in **private subnets**
- ALB deployed in **public subnets**
- IAM Roles for Service Accounts (IRSA)
- Kubernetes Secrets for DB credentials
- No hardcoded credentials
- RBAC enabled

---

# 📈 High Availability & Scaling

## Deployment Strategy
- Rolling updates enabled
- Replica-based deployments

## Auto Scaling
- **Cluster Autoscaler**
  - Scales worker nodes automatically
  - Supports On-Demand + Spot instances
- Managed Node Groups:
  - On-Demand (stable workloads)
  - Spot (cost-optimized workloads)

---

# 🌐 Networking Design

- Custom VPC
- 2 Availability Zones
- Public + Private Subnets
- NAT Gateway for outbound traffic
- ALB for internet-facing traffic
- Internal Service discovery via Kubernetes DNS

---

# 📦 Infrastructure as Code

Terraform provisions:

- VPC & Subnets
- NAT Gateway
- EKS Cluster
- Managed Node Groups
- IAM Roles
- IRSA Roles
- AWS Load Balancer Controller
- Cluster Autoscaler

State stored in:
```
S3 Backend
```

---

# 🔍 Observability

Prometheus configured to monitor:

- Kubernetes API Server
- Nodes
- Pods
- Services

Alertmanager enabled for production-grade monitoring.

---

# 🚀 Deployment Flow

1. Provision infrastructure using Terraform
2. Build Docker images (frontend & backend)
3. Push images to container registry
4. Deploy Kubernetes manifests
5. Ingress creates ALB automatically
6. Application becomes publicly accessible

---

# 🎯 Key Production Features

✔ Infrastructure as Code  
✔ Secure VPC Architecture  
✔ Private Worker Nodes  
✔ Auto Scaling (Nodes)  
✔ Rolling Deployments  
✔ Health Probes  
✔ IAM Role Segregation  
✔ Cost Optimization (Spot Instances)  
✔ Monitoring & Alerting  

---

# 🧠 What This Project Demonstrates

- End-to-end cloud-native deployment
- Production-level Kubernetes architecture
- AWS networking best practices
- Infrastructure automation
- Secure workload identity management
- Horizontal scaling strategies

---

# 📎 How To Run (High-Level)

```bash
terraform init
terraform apply
kubectl apply -f k8s-manifests/
```

---

# 📌 Future Improvements

- Persistent storage for MongoDB
- CI/CD Pipeline integration
- Blue-Green or Canary deployments
- Centralized logging (ELK / Loki)
- Horizontal Pod Autoscaler (HPA)

---

# 👨‍💻 Author
# Anjali Yadav

An end-to-end production-ready DevOps deployment showcasing real-world cloud architecture patterns.
