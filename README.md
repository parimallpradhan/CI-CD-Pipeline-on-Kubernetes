# 📘 Project: Enterprise End-to-End CI/CD Pipeline on Kubernetes

## 🎯 Goal

Automatically perform the following whenever code is pushed to GitHub:

```text
Developer Push

↓

GitHub

↓

Jenkins

↓

Build Application

↓

Run Unit Tests

↓

Build Docker Image

↓

Scan Docker Image (Trivy)

↓

Push Image to Docker Hub

↓

Deploy to Kubernetes

↓

Application Live
```

---

# 🏗️ High-Level Architecture

```text
                     GitHub
                        │
                 GitHub Webhook
                        │
                ┌────────────────┐
                │ Jenkins Server │
                └────────────────┘
                        │
        ┌───────────────┼────────────────┐
        │               │                │
     Maven Build    Docker Build     Trivy Scan
        │               │                │
        └───────────────┼────────────────┘
                        │
                 Docker Hub Registry
                        │
                Kubernetes Cluster
                        │
                 Application Running
```

---

# 🛠️ Tools

| Tool       | Purpose          |
| ---------- | ---------------- |
| GitHub     | Source Code      |
| Jenkins    | CI/CD            |
| Docker     | Containerization |
| Trivy      | Security Scan    |
| Kubernetes | Deployment       |
| Docker Hub | Image Registry   |
| Java       | Application      |
| Maven      | Build Tool       |
| Ubuntu     | Operating System |

---

# 📅 Implementation Plan

We'll complete this in **10 phases**.

| Phase | Topic                           |
| ----- | ------------------------------- |
| 1     | AWS Infrastructure              |
| 2     | Install Jenkins                 |
| 3     | Install Docker                  |
| 4     | Install Kubernetes              |
| 5     | Configure Jenkins               |
| 6     | Build Java Application          |
| 7     | Dockerize Application           |
| 8     | Trivy Scan                      |
| 9     | Deploy to Kubernetes            |
| 10    | Build Complete Jenkins Pipeline |

---

[Phase 1 – AWS Infrastructure](https://github.com/parimallpradhan/CI-CD-Pipeline-on-Kubernetes/blob/main/Phase%201%20%E2%80%93%20AWS%20Infrastructure.md)
Phase 2 – Install & Configure Jenkins on AWS EC2
Phase 3 – Install Docker & Integrate with Jenkins
Phase 4 – Install Kubernetes Using kubeadm
Phase 5 – Configure Jenkins for Kubernetes CICD
Phase 6 – Build the Java Application with Maven
Phase 7 – Dockerize the Spring Boot Application
Phase 8 – DevSecOps: Scan Docker Images with Trivy
Phase 9 – Deploy Application to Kubernetes
Phase 10 – Build the Complete Jenkins CICD Pipeline

