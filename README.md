# 🗳️ 3-Tier Voting Application on Amazon EKS

> A production-style deployment of a microservices-based application on Amazon EKS, using Kubernetes, Ingress, DNS, and persistent storage.

---

## 📖 Project Overview

This project demonstrates deploying a **3-tier microservices application** on **Amazon EKS (Elastic Kubernetes Service)** using Kubernetes best practices.

The application is the classic **Voting App**, composed of multiple services that communicate internally via Kubernetes networking and are exposed externally using an **NGINX Ingress Controller** with custom DNS.

The focus of this project is not just deployment, but understanding how Kubernetes components interact, how to debug real issues, and how to design a clean, scalable architecture on EKS.

---

## 🏗️ Architecture

### Application Components

| Service | Technology | Purpose |
|------|-----------|---------|
| Vote | Python (Flask) | Frontend UI where users cast votes |
| Worker | .NET | Background processor that consumes votes |
| Redis | Redis | Temporary queue for incoming votes |
| Postgres | PostgreSQL | Persistent storage for vote counts |
| Result | Node.js | Displays live voting results |

---

### Request Flow

User
↓
NGINX Ingress (DNS + Routing)
↓
Vote Service → Redis
↓
Worker → PostgreSQL → Result Service


---

## ☁️ Platform & Tools

- Amazon EKS  
- Kubernetes (Deployments, Services, PVCs, Ingress)  
- NGINX Ingress Controller  
- AWS Route 53  
- Amazon EBS  
- Docker & Docker Hub  

---

## 🔧 Kubernetes Design Choices

### Namespace Isolation

All resources are deployed in a dedicated namespace:
chinmayee

This ensures clean separation within a shared EKS cluster.

---

### Service-to-Service Communication

- Internal communication uses **ClusterIP Services**
- No hard-coded IP addresses
- Services communicate via **Kubernetes DNS**

Examples:
chinmayee-svc-postgres
chinmayee-svc-redis

---

### Ingress & DNS

- Shared **NGINX Ingress Controller** for the cluster
- Separate **Ingress resource** for this project
- Host-based routing configured using **AWS Route 53**

| URL | Description |
|----|------------|
| http://vote.app.chin.diogohack.shop | Voting frontend |
| http://result.app.chin.diogohack.shop | Results dashboard |

---

### Stateful Workload (Postgres)

- PostgreSQL uses an **EBS-backed PersistentVolumeClaim**
- `PGDATA` subdirectory used to avoid filesystem conflicts
- Data persists across pod restarts

---

## 🧪 Validation & Testing

### Kubernetes Checks

kubectl get pods -n chinmayee
kubectl get svc -n chinmayee
kubectl get endpoints -n chinmayee
kubectl get ingress -n chinmayee

### Application Testing

curl http://vote.app.chin.diogohack.shop
curl http://result.app.chin.diogohack.shop

Votes successfully flow through:

Vote → Redis → Worker → Postgres → Result

### 🛠️ Troubleshooting & Debugging

Real-world issues identified and resolved during this project:

Ingress 404 errors caused by incorrect Load Balancer usage

Services without endpoints resulting in 503 errors

PostgreSQL failing due to lost+found directory on EBS volumes

DNS issues caused by missing Host headers

Conflicts between shared and custom IngressClasses

Debugging tools used:

kubectl describe
kubectl logs
kubectl get endpoints
kubectl get ingress

### 🚀 Future Enhancements

Move database credentials to Kubernetes Secrets

Enable HTTPS using cert-manager and Let’s Encrypt

Add CI/CD using GitHub Actions

Implement Horizontal Pod Autoscaler (HPA)

Add monitoring with Prometheus and Grafana

### 🎯 Key Learnings

Amazon EKS & Kubernetes fundamentals

Ingress controllers and DNS routing

Stateful workloads on Kubernetes

Microservices communication patterns

Debugging real production-like issues

### 👩‍💻 Author

Chinmayee Pradhan
Aspiring Cloud / DevOps Engineer
📍 Netherlands

⭐ Feel free to explore the repository or reach out for feedback
