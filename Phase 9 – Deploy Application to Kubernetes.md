# 📘 Phase 9 – Deploy Application to Kubernetes

## 🎯 Objective

Deploy the Docker image to Kubernetes using:

* Namespace
* Deployment
* Service
* Rolling Updates
* Health Checks
* Image Pull Secret (optional)
* Zero Downtime Deployment

---

# 🏗 Architecture

```text
                    GitHub
                       │
                       ▼
                   Jenkins
                       │
        ┌──────────────┼──────────────┐
        │              │              │
    Maven Build   Docker Build   Trivy Scan
        │              │              │
        └──────────────┼──────────────┘
                       │
                Docker Hub Registry
                       │
                       ▼
                Kubernetes Cluster
                       │
            ┌──────────┴──────────┐
            │                     │
        Deployment            Service
            │                     │
            └──────────┬──────────┘
                       ▼
                  Spring Boot Pods
```

---

# Project Structure

```
springboot-cicd-demo/

├── Dockerfile

├── Jenkinsfile

├── kubernetes/
│
├── namespace.yaml
├── deployment.yaml
├── service.yaml
│
├── src/
├── pom.xml
└── README.md
```

---

# Step 1 — Create Kubernetes Namespace

Create a file:

```
kubernetes/namespace.yaml
```

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: cicd
```

Apply:

```bash
kubectl apply -f kubernetes/namespace.yaml
```

Verify:

```bash
kubectl get ns
```

Expected:

```
cicd
```

📸 Screenshot

---

# Step 2 — Create Deployment

Create:

```
kubernetes/deployment.yaml
```

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: springboot-app
  namespace: cicd

spec:
  replicas: 2

  selector:
    matchLabels:
      app: springboot

  template:

    metadata:
      labels:
        app: springboot

    spec:

      containers:

      - name: springboot

        image: YOUR_DOCKER_USERNAME/springboot-cicd:v1

        imagePullPolicy: Always

        ports:
        - containerPort: 8080

        readinessProbe:
          httpGet:
            path: /health
            port: 8080

          initialDelaySeconds: 20
          periodSeconds: 10

        livenessProbe:
          httpGet:
            path: /health
            port: 8080

          initialDelaySeconds: 30
          periodSeconds: 15

        resources:

          requests:
            cpu: "250m"
            memory: "256Mi"

          limits:
            cpu: "500m"
            memory: "512Mi"
```

---

## Why use Probes?

### Readiness Probe

Determines when the pod is ready to receive traffic.

Without it,

Users may receive

```
503 Service Unavailable
```

---

### Liveness Probe

Detects unhealthy applications.

If unhealthy,

Kubernetes automatically restarts the pod.

---

# Step 3 — Create Service

```
kubernetes/service.yaml
```

```yaml
apiVersion: v1

kind: Service

metadata:

  name: springboot-service

  namespace: cicd

spec:

  selector:

    app: springboot

  ports:

  - port: 80

    targetPort: 8080

  type: NodePort
```

---

# Step 4 — Deploy Resources

```bash
kubectl apply -f kubernetes/
```

Expected

```
namespace created

deployment created

service created
```

📸 Screenshot

---

# Step 5 — Verify Deployment

```bash
kubectl get all -n cicd
```

Expected

```
Pods

ReplicaSets

Deployments

Service
```

📸 Screenshot

---

# Step 6 — Verify Pods

```bash
kubectl get pods -n cicd
```

Expected

```
Running
```

📸 Screenshot

---

# Step 7 — Describe Pod

```bash
kubectl describe pod POD_NAME -n cicd
```

Check

* Events

* Image

* Health

📸 Screenshot

---

# Step 8 — Check Logs

```bash
kubectl logs POD_NAME -n cicd
```

Expected

```
Started Application
```

📸 Screenshot

---

# Step 9 — Verify Service

```bash
kubectl get svc -n cicd
```

Example

```
springboot-service

NodePort

30080
```

📸 Screenshot

---

# Step 10 — Test Application

Find EC2 Public IP

Browser

```
http://EC2_PUBLIC_IP:NodePort
```

Example

```
http://13.234.xx.xx:30080
```

Expected

```
Welcome to Enterprise CI/CD Pipeline!
```

📸 Screenshot

---

# Step 11 — Scale Application

Increase replicas

```bash
kubectl scale deployment springboot-app \
--replicas=5 \
-n cicd
```

Verify

```bash
kubectl get pods -n cicd
```

Expected

```
5 Running Pods
```

📸 Screenshot

---

# Step 12 — Rolling Update

Update Docker Image

```
v2
```

Run

```bash
kubectl set image deployment/springboot-app \
springboot=YOUR_DOCKER_USERNAME/springboot-cicd:v2 \
-n cicd
```

Check rollout

```bash
kubectl rollout status deployment springboot-app \
-n cicd
```

📸 Screenshot

---

# Step 13 — Rollback

If deployment fails

```bash
kubectl rollout undo deployment springboot-app \
-n cicd
```

Verify

```bash
kubectl rollout history deployment springboot-app \
-n cicd
```

📸 Screenshot

---

# Step 14 — Delete Resources

```bash
kubectl delete -f kubernetes/
```

Verify

```bash
kubectl get all -n cicd
```

Everything removed.

---

# Verification Commands

```bash
kubectl get nodes

kubectl get pods

kubectl get svc

kubectl get deployments

kubectl get rs

kubectl describe pod

kubectl logs

kubectl rollout history

kubectl rollout status
```

---

# Kubernetes Best Practices

Instead of using

```
latest
```

Use

```
BUILD_NUMBER
```

Example

```
springboot-cicd:45
```

This makes deployments traceable and supports rollbacks.

---

Use

```
imagePullPolicy: Always
```

to ensure the newest image is pulled.

---

Always define

* CPU Requests
* CPU Limits
* Memory Requests
* Memory Limits

to improve scheduling and stability.

---

Avoid running multiple replicas on a single-node cluster in production. For learning, it's acceptable, but a production setup should use multiple worker nodes.

---

# Screenshots

```
images/

namespace.png

deployment.png

pods.png

service.png

application.png

rollout.png

scaling.png

rollback.png
```

---

# README Update

Add a section:

```markdown
## Phase 9 – Kubernetes Deployment

### Objective

Deploy the Docker image to Kubernetes using a Deployment and Service, and validate rolling updates, scaling, and rollback.

### Kubernetes Resources

- Namespace
- Deployment
- Service

### Verification

- Pods Running
- Service Accessible
- Scaling Successful
- Rolling Update Successful
- Rollback Successful

📸 Screenshots
```

---

# 🎉 Phase 9 Complete

Your pipeline is now capable of:

```
GitHub
    │
    ▼
Jenkins
    │
    ▼
Maven Build
    │
    ▼
Docker Build
    │
    ▼
Trivy Scan
    │
    ▼
Docker Hub
    │
    ▼
Kubernetes Deployment
    │
    ▼
Application Live
```

## 🚀 Next Phase (Final): Complete Jenkins Pipeline (`Jenkinsfile`)

This is where we'll automate **everything**. A single GitHub push will trigger Jenkins to:

1. Clone the repository.
2. Build the application with Maven.
3. Run unit tests.
4. Build the Docker image.
5. Scan it with Trivy.
6. Push the image to Docker Hub.
7. Update the Kubernetes deployment with the new image tag.
8. Verify the rollout.
9. Mark the build as successful only if every stage passes.

At the end of Phase 10, you'll have a true **end-to-end CI/CD and DevSecOps pipeline** that you can confidently showcase on GitHub and discuss in interviews.
