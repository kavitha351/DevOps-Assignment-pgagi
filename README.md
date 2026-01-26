# DevOps Assignment – PGAGI

## 📌 Project Overview
This project demonstrates an end-to-end **DevOps workflow** covering application development, containerization, CI/CD automation, and Kubernetes deployment on AWS EKS.

The objective was to build a production-style setup with clear separation between **Continuous Integration (CI)** and **Continuous Deployment (CD)**, following cloud-native and container best practices.

---

## 🏗️ Architecture

**Frontend**
- Next.js application
- Communicates with backend via REST APIs

**Backend**
- Python FastAPI application
- Exposes health and message endpoints

**Containerization**
- Multi-stage Docker builds
- Non-root containers
- Optimized image sizes

**CI/CD**
- GitHub Actions
- CI on `develop` branch
- CD on `main` branch

**Orchestration**
- Kubernetes on AWS EKS

---

## 🔧 Backend Details
- Built using FastAPI
- Endpoints:
  - `GET /api/health`
  - `GET /api/message`
- Unit tests written using `pytest`
- Dockerized using a multi-stage Dockerfile

---

## 🎨 Frontend Details
- Built using Next.js
- Uses environment variables via `NEXT_PUBLIC_API_URL`
- Dockerized using a multi-stage build
- Served through a Kubernetes Service

---

## 🔄 CI Pipeline (develop branch)
Triggered on every push to the `develop` branch:
1. Checkout source code
2. Install dependencies
3. Run backend unit tests
4. Build Docker images
5. Tag images using Git SHA
6. Push images to AWS ECR

---

## 🚀 CD Pipeline (main branch)
Triggered on merge to the `main` branch:
1. Authenticate with AWS
2. Update kubeconfig for EKS
3. Deploy images to Kubernetes
4. Perform rolling updates using `kubectl set image`

---

## ☸️ Kubernetes Deployment

Resources used:
- Deployments for frontend and backend
- Services:
  - Backend: `ClusterIP`
  - Frontend: `NodePort`
- Ingress rules for routing `/` and `/api`

### Ingress Note
An NGINX Ingress Controller was installed and Ingress rules were defined. However, AWS LoadBalancer provisioning could not be completed due to missing VPC subnet tags (`kubernetes.io/role/elb` / `kubernetes.io/role/internal-elb`). NodePort access was used for validation.

---

## 🔑 Key Implementation Highlights
- Designed **multi-stage Dockerfiles** for frontend and backend to separate build and runtime concerns
- Executed backend **unit tests inside Docker build stage** to fail fast on errors
- Configured containers to run as **non-root users** for security best practices
- Implemented **branch-based CI/CD** using GitHub Actions (CI on develop, CD on main)
- Used **Git commit SHA-based Docker image tagging** for traceability
- Integrated **AWS ECR authentication** securely using GitHub Secrets
- Deployed applications to **AWS EKS** with rolling updates
- Used **ClusterIP** for internal backend communication and **NodePort** for frontend validation
- Defined Ingress routing rules for frontend and backend paths
- Debugged AWS LoadBalancer issues by inspecting Kubernetes events and subnet tags
- Applied **layered debugging**: Pod → Service → DNS → NodePort → Ingress
- Used ephemeral debug pods instead of modifying production containers
- Identified **build-time vs runtime environment variable behavior** in Next.js
- Performed complete cloud resource cleanup to avoid unnecessary costs

---

## 🧠 Key Learnings
- Practical understanding of Kubernetes networking
- Importance of VPC subnet tagging for AWS LoadBalancers
- Difference between NodePort and Ingress
- CI vs CD separation and automation
- Secure and optimized container practices

---

## 🧹 Cleanup & Cost Control
All cloud resources were deleted after project completion:
- Kubernetes workloads
- Ingress controller
- EKS cluster
- EC2 instances and LoadBalancers

This ensured no ongoing AWS charges.

---

## 📎 Repository
```
https://github.com/kavitha351/DevOps-Assignment-pgagi
```

## Actual Implementation

- For actual implementaion check the readme file on `develop` branch.
