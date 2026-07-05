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
| 1     | [AWS Infrastructure](https://github.com/parimallpradhan/CI-CD-Pipeline-on-Kubernetes/blob/main/Phase%201%20%E2%80%93%20AWS%20Infrastructure.md)              |
| 2     | [Install Jenkins](https://github.com/parimallpradhan/CI-CD-Pipeline-on-Kubernetes/blob/main/Phase%202%20%E2%80%93%20Install%20%26%20Configure%20Jenkins%20on%20AWS%20EC2.md)                 |
| 3     | [Install Docker](https://github.com/parimallpradhan/CI-CD-Pipeline-on-Kubernetes/blob/main/Phase%203%20%E2%80%93%20Install%20Docker%20%26%20Integrate%20with%20Jenkins.md)                  |
| 4     | [Install Kubernetes](https://github.com/parimallpradhan/CI-CD-Pipeline-on-Kubernetes/blob/main/Phase%204%20%E2%80%93%20Install%20Kubernetes%20Using%20kubeadm.md)              |
| 5     | [Configure Jenkins](https://github.com/parimallpradhan/CI-CD-Pipeline-on-Kubernetes/blob/main/Phase%205%20%E2%80%93%20Configure%20Jenkins%20for%20Kubernetes%20CICD.md)               |
| 6     | [Build Java Application](https://github.com/parimallpradhan/CI-CD-Pipeline-on-Kubernetes/blob/main/Phase%206%20%E2%80%93%20Build%20the%20Java%20Application%20with%20Maven.md)         |
| 7     | [Dockerize Application](https://github.com/parimallpradhan/CI-CD-Pipeline-on-Kubernetes/blob/main/Phase%207%20%E2%80%93%20Dockerize%20the%20Spring%20Boot%20Application.md)           |
| 8     | [Trivy Scan](https://github.com/parimallpradhan/CI-CD-Pipeline-on-Kubernetes/blob/main/Phase%208%20%E2%80%93%20DevSecOps%3A%20Scan%20Docker%20Images%20with%20Trivy.md)                      |
| 9     | [Deploy to Kubernetes](https://github.com/parimallpradhan/CI-CD-Pipeline-on-Kubernetes/blob/main/Phase%209%20%E2%80%93%20Deploy%20Application%20to%20Kubernetes.md)            |
| 10    | [Build Complete Jenkins Pipeline](https://github.com/parimallpradhan/CI-CD-Pipeline-on-Kubernetes/blob/main/Phase%2010%20%E2%80%93%20Build%20the%20Complete%20Jenkins%20CICD%20Pipeline.md) |

---
