📌 Project Overview

This project demonstrates deployment of a production-ready 3-tier application on Kubernetes using Amazon EKS.

The application follows modern DevOps best practices including:

Containerization using Docker
Kubernetes orchestration
Secure networking in AWS VPC
CI/CD automation

Scalable and resilient architecture
🏗️ Architecture

The system follows a 3-tier architecture model:

🟢 Tier 1 – Frontend

React-based web UI
Exposed via Kubernetes Service (LoadBalancer / Ingress)
Communicates with Backend API

🔵 Tier 2 – Backend

REST API built with Flask / Node.js
Handles business logic
Connects securely to MySQL database
Deployed as Kubernetes Deployment

🟡 Tier 3 – Database

MySQL database
Runs inside Kubernetes with Persistent Volume
Credentials managed using Kubernetes Secrets

☁️ Cloud Infrastructure

Cloud Provider: Amazon Web Services (AWS)
Kubernetes: Amazon Elastic Kubernetes Service (EKS)
Container Registry: Amazon ECR
Networking: VPC with public & private subnets
Load Balancer: AWS Application Load Balancer (ALB)
IAM roles for secure cluster access

🔁 CI/CD Pipeline

The project includes automated CI/CD workflow:
Code pushed to GitHub
Docker image built automatically
Image pushed to Amazon ECR
Kubernetes deployment updated
Rolling update ensures zero downtime

📦 Tech Stack

Docker
Kubernetes
Amazon EKS
Amazon ECR
AWS VPC
MySQL
Flask / Node.js
React (Frontend)
GitHub Actions (CI/CD)


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
