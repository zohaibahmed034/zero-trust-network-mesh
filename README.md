# 🔐 Advanced Network Security in Kubernetes with Network Policies & Istio

## 📌 Project Overview

This project demonstrates **defense-in-depth security in Kubernetes** by combining:

- **Kubernetes Network Policies**
- **Istio Service Mesh**
- **Mutual TLS (mTLS)**
- **Authorization Policies**
- **Traffic Monitoring & Observability**
- **Rate Limiting**
- **Attack Simulation & Mitigation**

A multi-tier application is deployed across isolated namespaces to showcase secure communication between services while blocking unauthorized traffic.

---

## 🎯 Objectives

This lab focuses on:

- Implementing Kubernetes Network Policies
- Restricting pod-to-pod communication
- Deploying Istio for service mesh security
- Enabling strict mTLS
- Applying Authorization Policies
- Monitoring traffic with observability tools
- Simulating network attacks
- Implementing rate limiting
- Performing security audits

---

## 🛠️ Tech Stack

- **Kubernetes**
- **Istio**
- **Network Policies**
- **Prometheus**
- **Grafana**
- **Jaeger**
- **Kiali**
- **Docker**
- **Ubuntu 22.04**

---

## 🏗️ Architecture

```text
Frontend Namespace
   ↓
Backend Namespace
   ↓
Database Namespace
```

Security layers:
- Network Policies
- Istio mTLS
- Authorization Policies
- Rate Limiting

---

## 📂 Project Structure

```bash
.
├── network-policies/
│   ├── default-deny.yaml
│   ├── allow-frontend-backend.yaml
│   ├── allow-backend-database.yaml
│
├── istio-security/
│   ├── authorization-policy.yaml
│   ├── mtls-policy.yaml
│   ├── rate-limit.yaml
│
├── monitoring/
│   ├── prometheus.yaml
│   ├── grafana.yaml
│   ├── jaeger.yaml
│   └── kiali.yaml
│
├── attack-simulation/
│   ├── attacker-pod.yaml
│   └── security-audit-report.txt
│
└── README.md
```

---

## ⚙️ Prerequisites

Before starting:

- Kubernetes cluster
- kubectl configured
- Docker installed
- Ubuntu/Linux environment
- Internet connectivity

Knowledge required:

- Kubernetes basics
- Networking concepts
- YAML configuration

---

## 🚀 Setup & Deployment

## 1. Verify Cluster

```bash
kubectl cluster-info
kubectl get nodes
```

---

## 2. Create Namespaces

```bash
kubectl create namespace frontend
kubectl create namespace backend
kubectl create namespace database
kubectl create namespace monitoring
```

---

## 3. Deploy Applications

Deploy:

- Frontend (Nginx)
- Backend (Apache HTTPD)
- Database (PostgreSQL)

```bash
kubectl apply -f frontend.yaml
kubectl apply -f backend.yaml
kubectl apply -f database.yaml
```

Verify:

```bash
kubectl get pods --all-namespaces
kubectl get services --all-namespaces
```

---

## 🔒 Network Security Implementation

## Default Deny Policies

Applied to all namespaces:

```bash
kubectl apply -f network-policies/default-deny.yaml
```

This blocks:

- All ingress traffic
- All egress traffic

---

## Allow Required Traffic Only

### Frontend → Backend

```bash
kubectl apply -f network-policies/allow-frontend-backend.yaml
```

### Backend → Database

```bash
kubectl apply -f network-policies/allow-backend-database.yaml
```

---

## ☸️ Istio Service Mesh Installation

Install Istio:

```bash
curl -L https://istio.io/downloadIstio | sh -
export PATH=$PWD/istio-*/bin:$PATH
istioctl install --set values.defaultRevision=default -y
```

Enable injection:

```bash
kubectl label namespace frontend istio-injection=enabled
kubectl label namespace backend istio-injection=enabled
kubectl label namespace database istio-injection=enabled
```

Restart deployments:

```bash
kubectl rollout restart deployment frontend-app -n frontend
kubectl rollout restart deployment backend-app -n backend
kubectl rollout restart deployment database-app -n database
```

---

## 🔐 Enable Mutual TLS

Apply strict mTLS:

```bash
kubectl apply -f istio-security/mtls-policy.yaml
```

Verify:

```bash
kubectl get peerauthentication --all-namespaces
```

---

## 🛡️ Authorization Policies

Apply:

```bash
kubectl apply -f istio-security/authorization-policy.yaml
```

Policies enforce:

- Frontend can access backend
- Backend can access database
- Unauthorized traffic blocked

---

## 📊 Observability Stack

Install:

```bash
kubectl apply -f monitoring/prometheus.yaml
kubectl apply -f monitoring/grafana.yaml
kubectl apply -f monitoring/jaeger.yaml
kubectl apply -f monitoring/kiali.yaml
```

---

## Dashboards

### Grafana

```bash
kubectl port-forward -n istio-system svc/grafana 3000:3000
```

Access:
`http://localhost:3000`

---

### Jaeger

```bash
kubectl port-forward -n istio-system svc/jaeger 16686:16686
```

Access:
`http://localhost:16686`

---

### Kiali

```bash
kubectl port-forward -n istio-system svc/kiali 20001:20001
```

Access:
`http://localhost:20001`

---

## ⚔️ Attack Simulation

A malicious pod is deployed to test security:

```bash
kubectl apply -f attack-simulation/attacker-pod.yaml
```

Simulated attacks:

- Direct database access
- Backend enumeration
- Port scanning

Expected result:

❌ Access denied by Network Policies and Istio.

---

## 🚦 Rate Limiting

Apply rate limiting:

```bash
kubectl apply -f istio-security/rate-limit.yaml
```

Test:

```bash
for i in {1..15}; do
  curl http://backend-service.backend.svc.cluster.local:8080
done
```

Result:
- Excess requests blocked automatically.

---

## 📋 Security Audit

Generate audit report:

```bash
cat security-audit-report.txt
```

Includes:

- Network Policies
- Authorization Policies
- mTLS status
- Service Mesh status
- Security violations

---

## ✅ Key Features Implemented

- Kubernetes Network Policies
- Default Deny Strategy
- Namespace Isolation
- Istio Service Mesh
- Strict mTLS
- Authorization Policies
- Attack Simulation
- Rate Limiting
- Security Monitoring
- Audit Reporting

---

## 🎓 Learning Outcomes

This project demonstrates:

- Kubernetes network segmentation
- Zero-trust architecture
- Defense-in-depth security
- Service mesh security
- Cloud-native observability
- Security monitoring & auditing

---

## 📜 Certification Alignment

Relevant for:

- **Kubernetes and Cloud Native Security Associate (KCSA)**
- Kubernetes Security
- Cloud Security Engineering
- DevSecOps

---

## 👨‍💻 Author

**Zohaib Ahmed**  
DevOps | Kubernetes | Cloud Security Enthusiast  

GitHub: https:(https://github.com/zohaibahmed034)

LinkedIn: https://linkedin.com/in/zohaibahmed034/
