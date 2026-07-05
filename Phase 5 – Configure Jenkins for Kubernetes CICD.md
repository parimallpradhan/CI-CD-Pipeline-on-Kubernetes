# 📘 Phase 5 – Configure Jenkins for Kubernetes CI/CD

## 🎯 Objective

Configure Jenkins with:

* Java
* Maven
* Git
* Docker
* kubectl
* Docker Hub Credentials
* GitHub Credentials
* Kubernetes Access
* Required Jenkins Plugins

By the end of this phase, Jenkins will be fully prepared for the CI/CD pipeline.

---

# 🏗 Architecture

```text
                  GitHub
                     │
                     ▼
               Jenkins Server
                     │
      ┌──────────────┼──────────────┐
      │              │              │
    Maven         Docker         kubectl
      │              │              │
      └──────────────┼──────────────┘
                     │
              Kubernetes Cluster
```

---

# Step 1 – Install Git

Verify Git:

```bash
git --version
```

If not installed:

```bash
sudo apt update
sudo apt install git -y
```

Verify:

```bash
git --version
```

📸 Screenshot

---

# Step 2 – Install Maven

Install Maven

```bash
sudo apt install maven -y
```

Verify

```bash
mvn -version
```

Expected

```
Apache Maven 3.x.x
```

📸 Screenshot

---

# Step 3 – Verify Java

```bash
java -version
```

Expected

```
OpenJDK 21
```

📸 Screenshot

---

# Step 4 – Install Required Jenkins Plugins

Open

```
Manage Jenkins

↓

Plugins

↓

Available Plugins
```

Install

| Plugin                  | Purpose                |
| ----------------------- | ---------------------- |
| Pipeline                | CI/CD Pipeline         |
| Git                     | Git Integration        |
| GitHub                  | GitHub Integration     |
| Docker                  | Docker Support         |
| Docker Pipeline         | Docker Build           |
| Kubernetes CLI          | kubectl Support        |
| Credentials Binding     | Secrets                |
| SSH Agent               | SSH Authentication     |
| Blue Ocean *(Optional)* | Pipeline Visualization |

Restart Jenkins.

📸 Screenshot

---

# Step 5 – Configure JDK in Jenkins

Navigate

```
Manage Jenkins

↓

Tools
```

### JDK

Click

```
Add JDK
```

Name

```
JDK21
```

Home

```bash
/usr/lib/jvm/java-21-openjdk-amd64
```

Save.

📸 Screenshot

---

# Step 6 – Configure Maven

Again

```
Manage Jenkins

↓

Tools
```

Add Maven

```
Maven3
```

Home

```bash
/usr/share/maven
```

Verify

```bash
which mvn
```

Expected

```bash
/usr/bin/mvn
```

📸 Screenshot

---

# Step 7 – Verify kubectl

```bash
kubectl version --client
```

Expected

```
Client Version
```

📸 Screenshot

---

# Step 8 – Give Jenkins Access to Kubernetes

Jenkins runs as the **jenkins** user, so it needs access to the Kubernetes configuration.

Create the Jenkins kube directory:

```bash
sudo mkdir -p /var/lib/jenkins/.kube
```

Copy the Kubernetes config:

```bash
sudo cp ~/.kube/config /var/lib/jenkins/.kube/config
```

Update ownership:

```bash
sudo chown -R jenkins:jenkins /var/lib/jenkins/.kube
```

Verify:

```bash
sudo -u jenkins kubectl get nodes
```

Expected

```
Ready
```

📸 Screenshot

---

# Step 9 – Create Docker Hub Credentials

Open

```
Manage Jenkins

↓

Credentials

↓

Global

↓

Add Credentials
```

Kind

```
Username with Password
```

Example

| Field       | Value                                        |
| ----------- | -------------------------------------------- |
| Username    | your Docker Hub username                     |
| Password    | Docker Hub Personal Access Token or password |
| ID          | dockerhub                                    |
| Description | Docker Hub Credentials                       |

Save.

📸 Screenshot

---

# Step 10 – Create GitHub Credentials

If your repository is private:

```
Manage Jenkins

↓

Credentials

↓

Add
```

Use a GitHub Personal Access Token.

Example

| Field    | Value           |
| -------- | --------------- |
| Username | GitHub Username |
| Password | GitHub PAT      |
| ID       | github          |

If the repository is public, credentials aren't required for cloning.

📸 Screenshot

---

# Step 11 – Test Docker

Run

```bash
docker images
```

Then

```bash
docker ps
```

Everything should work.

📸 Screenshot

---

# Step 12 – Test Kubernetes

```bash
kubectl get nodes
```

```bash
kubectl get pods -A
```

Expected

```
STATUS : Ready
```

📸 Screenshot

---

# Step 13 – Create a Test Pipeline

Open

```
New Item

↓

Pipeline

↓

test-pipeline
```

Pipeline Script

```groovy
pipeline {
    agent any

    stages {

        stage('Hello') {
            steps {
                echo 'Jenkins is working successfully!'
            }
        }

    }
}
```

Click

```
Build Now
```

Expected

```
SUCCESS
```

📸 Screenshot

---

# Step 14 – Test Docker in Jenkins

Replace the pipeline with:

```groovy
pipeline {
    agent any

    stages {

        stage('Docker Version') {

            steps {

                sh 'docker --version'

            }
        }

    }
}
```

Build.

Expected

```
Docker version ...
```

📸 Screenshot

---

# Step 15 – Test Kubernetes in Jenkins

Pipeline

```groovy
pipeline {

    agent any

    stages {

        stage('Check Cluster') {

            steps {

                sh 'kubectl get nodes'

            }

        }

    }

}
```

Build.

Expected

```
Ready
```

📸 Screenshot

---

# Verification Checklist

✅ Jenkins Running

✅ Git Installed

✅ Maven Installed

✅ Java Installed

✅ Docker Installed

✅ Kubernetes Connected

✅ Docker Hub Credentials Added

✅ GitHub Credentials Added (if needed)

✅ Jenkins Pipeline Successful

---

# Folder Structure

```
End-to-End-CICD-Pipeline/

├── Jenkinsfile
├── kubernetes/
├── Dockerfile
├── scripts/
├── images/
└── README.md
```

---

# Screenshots

```
images/

jenkins-dashboard.png

jenkins-tools.png

plugins.png

dockerhub-credentials.png

github-credentials.png

kubectl-ready.png

docker-version.png

pipeline-success.png
```

---

# 🎉 Phase 5 Completed

At this stage, your environment is fully prepared:

```
GitHub

↓

Jenkins

↓

Java

↓

Maven

↓

Docker

↓

Kubernetes
```

The infrastructure is now ready to execute a complete CI/CD workflow.

## 🚀 Next Phase – Build the Java Application


