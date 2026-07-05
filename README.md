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

## Objective

Prepare the infrastructure required for the CI/CD pipeline.

---

## Architecture

For learning and keeping costs low, we'll use **one EC2 instance** that hosts Jenkins, Docker, Trivy, and a local Kubernetes cluster (Minikube).

```text
AWS EC2 (Ubuntu 24.04 LTS)
│
├── Jenkins
├── Docker
├── Trivy
├── kubectl
├── Minikube
└── Java + Maven
```

This is ideal for a portfolio project. Later, we can evolve it into a multi-server architecture with dedicated Jenkins and Kubernetes nodes.

---

## AWS Resources

| Resource      | Value                              |
| ------------- | ---------------------------------- |
| Region        | ap-south-1 (Mumbai)                |
| OS            | Ubuntu Server 24.04 LTS            |
| Instance Type | t3.medium (recommended)            |
| Storage       | 30 GB gp3                          |
| Key Pair      | Create or use an existing key pair |

**Why `t3.medium`?** Jenkins, Docker, and Kubernetes are memory-intensive. A `t2.micro` or `t3.micro` often struggles with all these services running together.

---

## Security Group

Create a security group with these inbound rules:

| Port | Protocol | Purpose                |
| ---- | -------- | ---------------------- |
| 22   | TCP      | SSH                    |
| 8080 | TCP      | Jenkins                |
| 8081 | TCP      | Application (optional) |
| 80   | TCP      | HTTP                   |
| 443  | TCP      | HTTPS (optional)       |

> Avoid opening unnecessary ports to `0.0.0.0/0` in production.

---

## Launch the EC2 Instance

1. Sign in to the AWS Management Console.
2. Navigate to **EC2**.
3. Click **Launch Instance**.
4. Name it: `jenkins-k8s-server`.
5. Choose **Ubuntu Server 24.04 LTS**.
6. Select **t3.medium**.
7. Create or select an SSH key pair.
8. Attach a **30 GB gp3** root volume.
9. Associate the security group created earlier.
10. Launch the instance.

---

## Connect to the Server

From your local machine:

```bash
ssh -i your-key.pem ubuntu@<EC2_PUBLIC_IP>
```

Verify the operating system:

```bash
cat /etc/os-release
```

Expected output should indicate Ubuntu 24.04 LTS.

---

## Update the Server

```bash
sudo apt update
sudo apt upgrade -y
sudo reboot
```

Reconnect after the reboot and verify connectivity.

---

## Verification

Capture the following screenshots for your GitHub README:

* EC2 instance details.
* Security group inbound rules.
* Successful SSH session.
* `cat /etc/os-release` output.
* `hostnamectl` output.

---

## README Progress

As we complete each phase, you'll update your `README.md` with:

* The objective.
* Commands used.
* Why each step is necessary.
* Verification steps.
* Screenshots.
* Any issues encountered and how you resolved them.

By the end of the project, you'll have a complete, professional repository ready for recruiters to review.

---


