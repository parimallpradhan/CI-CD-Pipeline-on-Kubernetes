# 📘 Phase 7 – Dockerize the Spring Boot Application

## 🎯 Objective

In this phase, we will:

* Create a production-ready Dockerfile
* Build a Docker image
* Test the container locally
* Push the image to Docker Hub
* Prepare for Trivy security scanning

---

# 🏗️ Architecture

```text
               Jenkins
                   │
                   ▼
          Maven Build (JAR)
                   │
                   ▼
            Docker Build
                   │
                   ▼
        Spring Boot Docker Image
                   │
                   ▼
            Docker Container
```

---

# 📂 Repository Structure

Your repository should now look like this:

```text
springboot-cicd-demo/
│
├── Dockerfile
├── Jenkinsfile
├── pom.xml
├── README.md
│
├── src/
│
├── kubernetes/
│   ├── deployment.yaml
│   └── service.yaml
│
└── images/
```

---

# Step 1 – Create Dockerfile

Inside the project root:

```bash
touch Dockerfile
```

---

## Dockerfile

```dockerfile
FROM eclipse-temurin:21-jre

LABEL maintainer="Tushar More"

WORKDIR /app

COPY target/*.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java","-jar","app.jar"]
```

---

## Explanation

| Instruction | Purpose           |
| ----------- | ----------------- |
| FROM        | Base Java Runtime |
| LABEL       | Metadata          |
| WORKDIR     | Working Directory |
| COPY        | Copy Jar File     |
| EXPOSE      | Application Port  |
| ENTRYPOINT  | Start Application |

---

# Step 2 – Build Docker Image

```bash
docker build -t springboot-cicd:v1 .
```

Expected

```
Successfully built
Successfully tagged springboot-cicd:v1
```

📸 Screenshot

---

# Step 3 – Verify Image

```bash
docker images
```

Expected

```
springboot-cicd
v1
```

📸 Screenshot

---

# Step 4 – Run Docker Container

```bash
docker run -d \
--name springboot-app \
-p 8081:8080 \
springboot-cicd:v1
```

---

# Step 5 – Verify Running Container

```bash
docker ps
```

Expected

```
springboot-app
```

📸 Screenshot

---

# Step 6 – Test Application

```bash
curl http://localhost:8081/
```

Expected

```
Welcome to Enterprise CI/CD Pipeline!
```

Health Endpoint

```bash
curl http://localhost:8081/health
```

Expected

```
Application is Healthy
```

📸 Screenshot

---

# Step 7 – Check Logs

```bash
docker logs springboot-app
```

Expected

```
Started Application
```

📸 Screenshot

---

# Step 8 – Enter Container

```bash
docker exec -it springboot-app bash
```

Verify

```bash
ls
```

Exit

```bash
exit
```

📸 Screenshot

---

# Step 9 – Stop Container

```bash
docker stop springboot-app
```

Verify

```bash
docker ps
```

---

# Step 10 – Remove Container

```bash
docker rm springboot-app
```

Verify

```bash
docker ps -a
```

---

# Step 11 – Tag Docker Image

Login

```bash
docker login
```

Tag

```bash
docker tag springboot-cicd:v1 YOUR_DOCKER_USERNAME/springboot-cicd:v1
```

Verify

```bash
docker images
```

📸 Screenshot

---

# Step 12 – Push to Docker Hub

```bash
docker push YOUR_DOCKER_USERNAME/springboot-cicd:v1
```

Verify

Open Docker Hub

Image should appear

📸 Screenshot

---

# Verification Commands

```bash
docker images

docker ps

docker logs springboot-app

docker inspect springboot-app

docker image inspect springboot-cicd:v1
```

---

# Common Docker Commands

```bash
docker image ls

docker container ls

docker stop CONTAINER_ID

docker rm CONTAINER_ID

docker rmi IMAGE_ID

docker system prune -a
```

---

# Troubleshooting

### Port Already Allocated

```
Bind for 0.0.0.0:8081 failed
```

Check

```bash
docker ps
```

Stop running container.

---

### Container Exits Immediately

Check logs

```bash
docker logs springboot-app
```

---

### Image Not Found

```
Unable to find image
```

Run

```bash
docker images
```

---

# Screenshots

```
images/

dockerfile.png

docker-build.png

docker-images.png

docker-run.png

docker-ps.png

application-browser.png

docker-logs.png

dockerhub.png
```

---

# README Update

```markdown
## Phase 7 – Dockerize Application

### Objective

Containerize the Spring Boot application using Docker and publish the image to Docker Hub.

### Commands Used

- docker build
- docker run
- docker logs
- docker tag
- docker push

### Verification

- Docker image created
- Container running
- Application accessible
- Image uploaded to Docker Hub

📸 Screenshots
```

---

# 🎉 Phase 7 Completed

Current Pipeline Progress:

```text
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
Docker Hub
```

---

# 🚀 Next Phase – DevSecOps with Trivy

This is where your project becomes much stronger than a standard CI/CD demo. We'll integrate **Trivy** to:

* Scan the application filesystem.
* Scan the Docker image for vulnerabilities.
* Fail the Jenkins pipeline automatically if high or critical vulnerabilities are found.
* Generate vulnerability reports for review.

This addition demonstrates **DevSecOps** practices and is a strong talking point in interviews because it shows security is integrated into the delivery pipeline rather than treated as a separate step.
