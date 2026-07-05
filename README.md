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

# 🚀 Phase 1 – AWS Infrastructure



