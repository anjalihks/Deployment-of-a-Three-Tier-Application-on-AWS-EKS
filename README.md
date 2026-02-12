# 🚀 3-Tier Cloud-Native Application on AWS EKS

## 📌 Project Overview

This project demonstrates deployment of a production-ready **3-tier cloud-native web application** on **Amazon EKS (Elastic Kubernetes Service)**.

The application follows modern DevOps best practices including:

- Containerization using Docker
- Kubernetes orchestration
- Secure AWS networking
- CI/CD automation
- Scalable and resilient architecture

---

## 🏗️ Architecture

<img width="1280" height="629" alt="image" src="https://github.com/user-attachments/assets/fb923a83-9f9a-4bd8-9367-9d60142b7cff" />




The system is designed using a 3-tier architecture:

### 🟢 Tier 1 – Frontend
- Built using React
- Exposed via Kubernetes Service (LoadBalancer / Ingress)
- Communicates with backend API

### 🔵 Tier 2 – Backend
- REST API built with Flask / Node.js
- Handles business logic
- Connects securely to MySQL database
- Deployed using Kubernetes Deployment

### 🟡 Tier 3 – Database
- MySQL database
- Runs inside Kubernetes
- Persistent storage using PV & PVC
- Credentials managed via Kubernetes Secrets

---

## ☁️ Cloud Infrastructure

- Cloud Provider: AWS
- Kubernetes Service: Amazon EKS
- Container Registry: Amazon ECR
- Networking: VPC with public & private subnets
- Load Balancer: AWS Application Load Balancer (ALB)
- IAM Roles for secure cluster access

---

## 🔁 CI/CD Pipeline

Automated CI/CD workflow:

1. Code pushed to GitHub
2. Docker image automatically built
3. Image pushed to Amazon ECR
4. Kubernetes manifests updated
5. Rolling deployment applied to cluster

Ensures zero-downtime deployments.

---

## 📦 Tech Stack

- Docker
- Kubernetes
- Amazon EKS
- Amazon ECR
- AWS VPC
- MySQL
- Flask / Node.js
- React
- GitHub Actions

---

## ☸️ Kubernetes Components Used

- Deployments
- ReplicaSets
- Services (ClusterIP, LoadBalancer)
- Ingress Controller
- ConfigMaps
- Secrets
- Persistent Volumes (PV)
- Persistent Volume Claims (PVC)

---

## 🔐 Security Best Practices

- Database credentials stored in Kubernetes Secrets
- Private subnets for worker nodes
- Security Groups restricting inbound/outbound traffic
- IAM Roles for Service Accounts (IRSA)
- Internal communication using ClusterIP

---

## 📈 Scalability & Reliability

- Horizontal pod scaling
- Rolling updates
- Self-healing pods
- Load balancing across replicas
- Persistent storage for stateful workloads

---

## 🚀 Deployment Steps (High-Level)

1. Create AWS EKS cluster
2. Build Docker images for frontend and backend
3. Push images to Amazon ECR
4. Apply Kubernetes manifests
5. Access application via LoadBalancer URL

---

<img width="800" height="336" alt="image" src="https://github.com/user-attachments/assets/9779d34a-d000-41c6-a01d-8820b43e0b61" />

## 📂 Project Structure

├── frontend/
├── backend/
├── k8s/
│ ├── frontend-deployment.yaml
│ ├── backend-deployment.yaml
│ ├── mysql-deployment.yaml
│ ├── services.yaml
│ ├── ingress.yaml
│ └── pvc.yaml
├── Dockerfile
└── README.md


---

## 🎯 Key DevOps Concepts Demonstrated

- Multi-tier architecture deployment
- Container lifecycle management
- Kubernetes orchestration
- Cloud-native networking
- Infrastructure isolation
- CI/CD automation
- Secure secret management
- Rolling deployments

---

## 🏆 Production Highlights

✔ 3-Tier Architecture  
✔ Cloud Deployment on AWS EKS  
✔ Dockerized Services  
✔ CI/CD Pipeline Automation  
✔ Scalable & Resilient Infrastructure  
✔ Secure Configuration Management  

---

## 📚 Learning Outcomes

Through this project, I gained hands-on experience in:

- Deploying scalable applications on Kubernetes
- Managing AWS cloud infrastructure
- Automating build and deployment pipelines
- Implementing DevOps best practices
- Designing fault-tolerant architectures

---

## 📌 Future Enhancements

- Add Horizontal Pod Autoscaler (HPA)
- Integrate Prometheus & Grafana for monitoring
- Implement Blue-Green deployment strategy
- Use AWS RDS instead of in-cluster MySQL
- Infrastructure provisioning using Terraform


# three-tier-eks-iac

# Prerequisite 

**Install Kubectl**
https://kubernetes.io/docs/tasks/tools/


**Install Helm**
https://helm.sh/docs/intro/install/

```
helm repo update
```

**Install/update latest AWS CLI:** (make sure install v2 only)
https://aws.amazon.com/cli/

#update the Kubernetes context
aws eks update-kubeconfig --name my-eks-cluster --region us-west-2

# verify access:
```
kubectl auth can-i "*" "*"
kubectl get nodes
```

# Verify autoscaler running:
```
kubectl get pods -n kube-system
```

# Check Autoscaler logs
```
kubectl logs -f \
  -n kube-system \
  -l app=cluster-autoscaler
```

# Check load balancer logs
```
kubectl logs -f -n kube-system \
  -l app.kubernetes.io/name=aws-load-balancer-controller
```

<!-- aws eks update-kubeconfig \
  --name my-eks \
  --region us-west-2 \
  --profile eks-admin -->


# Build Docker image :
**For Mac:**

```
export DOCKER_CLI_EXPERIMENTAL=enabled
aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/w8u5e4v2
```

Build Front End :

```
docker buildx build --platform linux/amd64 -t workshop-frontend:v1 . 
docker tag workshop-frontend:v1 public.ecr.aws/w8u5e4v2/workshop-frontend:v1
docker push public.ecr.aws/w8u5e4v2/workshop-frontend:v1
```


Build Back End :

```
docker buildx build --platform linux/amd64 -t workshop-backend:v1 . 
docker tag workshop-backend:v1 public.ecr.aws/w8u5e4v2/workshop-backend:v1
docker push public.ecr.aws/w8u5e4v2/workshop-backend:v1
```

**For Linux/Windows:**

Buid Front End :

```
docker build -t workshop-frontend:v1 . 
docker tag workshop-frontend:v1 public.ecr.aws/w8u5e4v2/workshop-frontend:v1
docker push public.ecr.aws/w8u5e4v2/workshop-frontend:v1
```


Build Back End :

```
docker build -t workshop-backend:v1 . 
docker tag workshop-backend:v1 public.ecr.aws/w8u5e4v2/workshop-backend:v1
docker push public.ecr.aws/w8u5e4v2/workshop-backend:v1
```



**Create Namespace**
```
kubectl create ns workshop

kubectl config set-context --current --namespace workshop
```

# MongoDB Database Setup

**To create MongoDB Resources**
```
cd k8s_manifests/mongo_v1
kubectl apply -f secrets.yaml
kubectl apply -f deploy.yaml
kubectl apply -f service.yaml
```

# Backend API Setup

Create NodeJs API deployment by running the following command:
```
kubectl apply -f backend-deployment.yaml
kubectl apply -f backend-service.yaml
``


**Frontend setup**

Create the Frontend  resource. In the terminal run the following command:
```
kubectl apply -f frontend-deployment.yaml
kubectl apply -f frontend-service.yaml
```

Finally create the final load balancer to allow internet traffic:
```
kubectl apply -f full_stack_lb.yaml
```


# Any issue with the pods ? check logs:
kubectl logs -f POD_ID -f


# Grafana setup 
Username: admin
Password: prom-operator

Import Dashboard ID: 1860

Exlore more at: https://grafana.com/grafana/dashboards/
