# 📘 Phase 10 – Build the Complete Jenkins CI/CD Pipeline

## 🎯 Objective

Automate the complete software delivery lifecycle:

* Checkout source code
* Build the application
* Run unit tests
* Build Docker image
* Scan Docker image with Trivy
* Push image to Docker Hub
* Deploy to Kubernetes
* Verify deployment
* Clean workspace

---

# 🏗️ Complete CI/CD Architecture

```text
                Developer
                    │
              Git Push (GitHub)
                    │
            GitHub Webhook Trigger
                    │
                    ▼
              Jenkins Pipeline
                    │
      ┌─────────────┼─────────────────────┐
      │             │                     │
 Checkout      Maven Build          Unit Tests
      │
      ▼
 Docker Build
      │
      ▼
 Trivy Scan
      │
      ▼
 Push Image
      │
      ▼
 Kubernetes Deployment
      │
      ▼
 Rolling Update
      │
      ▼
 Application Live
```

---

# Step 1 – Repository Structure

```
springboot-cicd-demo/

│── Jenkinsfile
│── Dockerfile
│── pom.xml
│── README.md

├── kubernetes/
│      deployment.yaml
│      service.yaml
│      namespace.yaml

├── src/

├── images/

└── scripts/
```

---

# Step 2 – Jenkins Credentials

Create the following credentials:

| ID        | Type                               | Purpose          |
| --------- | ---------------------------------- | ---------------- |
| dockerhub | Username & Password                | Docker Hub Login |
| github    | Username & PAT *(if private repo)* | Git Clone        |

---

# Step 3 – Complete Jenkinsfile

Create a file named:

```
Jenkinsfile
```

```groovy
pipeline {

    agent any

    tools {
        jdk 'JDK21'
        maven 'Maven3'
    }

    environment {

        APP_NAME = "springboot-cicd"

        DOCKER_USERNAME = "YOUR_DOCKER_USERNAME"

        IMAGE_NAME = "${DOCKER_USERNAME}/${APP_NAME}:${BUILD_NUMBER}"

    }

    stages {

        stage('Checkout Source') {

            steps {

                git branch: 'main',
                url: 'https://github.com/YOUR_USERNAME/springboot-cicd-demo.git'

            }

        }

        stage('Build Application') {

            steps {

                sh 'mvn clean package'

            }

        }

        stage('Unit Test') {

            steps {

                sh 'mvn test'

            }

        }

        stage('Build Docker Image') {

            steps {

                sh '''
                docker build -t ${IMAGE_NAME} .
                '''

            }

        }

        stage('Trivy Scan') {

            steps {

                sh '''
                trivy image \
                --exit-code 1 \
                --severity HIGH,CRITICAL \
                ${IMAGE_NAME}
                '''

            }

        }

        stage('Push Docker Image') {

            steps {

                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub',
                        usernameVariable: 'USERNAME',
                        passwordVariable: 'PASSWORD'
                    )
                ]) {

                    sh '''
                    echo $PASSWORD | docker login -u $USERNAME --password-stdin

                    docker push ${IMAGE_NAME}
                    '''
                }

            }

        }

        stage('Deploy to Kubernetes') {

            steps {

                sh '''
                kubectl set image deployment/springboot-app \
                springboot=${IMAGE_NAME} \
                -n cicd

                kubectl rollout status deployment/springboot-app \
                -n cicd
                '''

            }

        }

    }

    post {

        always {

            sh 'docker system prune -af'

            cleanWs()

        }

        success {

            echo "Pipeline Completed Successfully"

        }

        failure {

            echo "Pipeline Failed"

        }

    }

}
```

---

# Step 4 – Configure GitHub Webhook

In your GitHub repository:

```
Settings
     ↓
Webhooks
     ↓
Add Webhook
```

Payload URL:

```
http://YOUR_PUBLIC_IP:8080/github-webhook/
```

Content Type:

```
application/json
```

Events:

```
Just the push event
```

---

# Step 5 – Configure Jenkins Job

* New Item → Pipeline
* Definition → Pipeline script from SCM
* SCM → Git
* Repository URL → Your GitHub repository
* Branch → `main`
* Script Path → `Jenkinsfile`

Save the job.

---

# Step 6 – Trigger the Pipeline

Make a small change to your application, for example:

```java
return "Welcome to Enterprise DevOps Pipeline Version 2!";
```

Commit and push:

```bash
git add .
git commit -m "Updated home page"
git push origin main
```

The GitHub webhook will trigger Jenkins automatically.

---

# Step 7 – Monitor the Pipeline

Open:

```
Jenkins Dashboard
        ↓
Pipeline
        ↓
Console Output
```

You should see:

```
Checkout

↓

Build

↓

Test

↓

Docker Build

↓

Trivy Scan

↓

Docker Push

↓

Kubernetes Deploy

↓

SUCCESS
```

---

# Step 8 – Verify Kubernetes

```bash
kubectl get pods -n cicd

kubectl get deployments -n cicd

kubectl get svc -n cicd
```

Verify the rollout:

```bash
kubectl rollout history deployment springboot-app -n cicd
```

---

# Step 9 – Verify the Application

Open your browser:

```
http://EC2_PUBLIC_IP:NODEPORT
```

You should see the updated message from your latest deployment.

---

# Final Verification

Your completed pipeline should look like this:

```
Developer Push
      │
      ▼
GitHub Repository
      │
      ▼
GitHub Webhook
      │
      ▼
Jenkins
      │
      ▼
Checkout
      │
      ▼
Maven Build
      │
      ▼
Unit Test
      │
      ▼
Docker Build
      │
      ▼
Trivy Scan
      │
      ▼
Push to Docker Hub
      │
      ▼
Deploy to Kubernetes
      │
      ▼
Rolling Update
      │
      ▼
Application Live
```

---

# Screenshots for GitHub

Capture and store:

```
images/

01-architecture.png
02-jenkins-dashboard.png
03-github-webhook.png
04-pipeline-success.png
05-maven-build.png
06-docker-build.png
07-trivy-scan.png
08-dockerhub.png
09-kubernetes-pods.png
10-kubernetes-service.png
11-application.png
12-rollout-history.png
```

---


