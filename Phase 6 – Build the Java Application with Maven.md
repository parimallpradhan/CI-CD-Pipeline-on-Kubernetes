# 📘 Phase 6 – Build the Java Application with Maven

## 🎯 Objective

In this phase, we will:

* Create a Java Spring Boot application
* Push it to GitHub
* Build it using Maven
* Generate the JAR file
* Verify the application locally
* Prepare it for Dockerization

---

# 🏗️ Architecture

```text
                Developer
                     │
                     ▼
                 GitHub Repo
                     │
                     ▼
                  Jenkins
                     │
                     ▼
             Maven Build (JAR)
                     │
                     ▼
          target/application.jar
```

---

# 📂 Project Structure

We'll use the following repository structure:

```text
End-to-End-CICD-Pipeline/
│
├── Jenkinsfile
├── Dockerfile
├── pom.xml
├── README.md
├── kubernetes/
│   ├── deployment.yaml
│   └── service.yaml
│
├── src/
│   ├── main/
│   └── test/
│
├── images/
└── scripts/
```

---

# Step 1 – Create a GitHub Repository

Create a new GitHub repository:

```text
springboot-cicd-demo
```

Clone it to your Jenkins server:

```bash
git clone https://github.com/<your-username>/springboot-cicd-demo.git
cd springboot-cicd-demo
```

Verify:

```bash
pwd
```

📸 Screenshot:

* GitHub repository
* Successful clone

---

# Step 2 – Create a Spring Boot Project

We'll use **Spring Initializr** with these settings:

| Setting     | Value       |
| ----------- | ----------- |
| Project     | Maven       |
| Language    | Java        |
| Spring Boot | 3.x         |
| Group       | com.example |
| Artifact    | cicd-demo   |
| Packaging   | Jar         |
| Java        | 21          |

Dependencies:

* Spring Web
* Spring Boot Actuator (optional)

Generate the project and extract it into your repository.

📸 Screenshot:

* Spring Initializr configuration
* Extracted project files

---

# Step 3 – Verify the Project Structure

Run:

```bash
ls -R
```

You should see:

```text
pom.xml
src/
mvnw
mvnw.cmd
```

📸 Screenshot

---

# Step 4 – Create a Simple REST API

Edit `src/main/java/.../HelloController.java`:

```java
package com.example.cicddemo;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

    @GetMapping("/")
    public String home() {
        return "Welcome to Enterprise CI/CD Pipeline!";
    }

    @GetMapping("/health")
    public String health() {
        return "Application is Healthy";
    }
}
```

---

# Step 5 – Build the Application

Run:

```bash
mvn clean package
```

Expected output:

```text
BUILD SUCCESS
```

📸 Screenshot:

* Maven build success

---

# Step 6 – Verify the JAR

List the target directory:

```bash
ls target/
```

Expected:

```text
cicd-demo-0.0.1-SNAPSHOT.jar
```

📸 Screenshot

---

# Step 7 – Run the Application

Start the application:

```bash
java -jar target/*.jar
```

You should see Spring Boot startup logs.

Open another terminal and test:

```bash
curl http://localhost:8080/
```

Expected:

```text
Welcome to Enterprise CI/CD Pipeline!
```

Test the health endpoint:

```bash
curl http://localhost:8080/health
```

Expected:

```text
Application is Healthy
```

📸 Screenshot:

* Application running
* `curl` output

---

# Step 8 – Commit the Code

Initialize Git (if needed):

```bash
git init
git add .
git commit -m "Initial Spring Boot application"
```

Push to GitHub:

```bash
git branch -M main
git remote add origin https://github.com/<your-username>/springboot-cicd-demo.git
git push -u origin main
```

📸 Screenshot:

* GitHub repository with source code

---

# Step 9 – Configure Jenkins Job

Create a new **Pipeline** job:

* Name: `springboot-cicd`

In the Pipeline configuration:

* **Definition:** Pipeline script from SCM
* **SCM:** Git
* **Repository URL:** Your GitHub repository
* **Branch:** `main`
* **Script Path:** `Jenkinsfile` (we'll add this in the next phase)

Save the job.

📸 Screenshot:

* Jenkins job configuration

---

# Verification Checklist

* Spring Boot project created
* Maven build successful
* JAR generated
* Application runs locally
* REST endpoints accessible
* Code pushed to GitHub
* Jenkins pipeline job created

---

# Screenshots for GitHub

Save these under:

```text
images/
├── github-repo.png
├── spring-initializr.png
├── project-structure.png
├── maven-build.png
├── jar-file.png
├── app-running.png
├── curl-home.png
├── curl-health.png
└── jenkins-job.png
```

---

# README Update

Add a section:

```markdown
## Phase 6 – Build the Java Application

### Objective
Create a Spring Boot application, build it with Maven, verify it locally, and prepare it for containerization.

### Commands Used
- git clone
- mvn clean package
- java -jar
- curl

### Verification
- BUILD SUCCESS
- JAR file created
- REST API accessible

📸 Screenshots
```

---

# 🎉 Phase 6 Complete

You now have:

* ✅ Source code in GitHub
* ✅ Maven build working
* ✅ Executable JAR
* ✅ Application verified locally
* ✅ Jenkins ready to build the application

---

# 🚀 Next Phase – Dockerize the Application

In **Phase 7**, we'll:

1. Write a production-ready `Dockerfile`.
2. Build a Docker image.
3. Test the container locally.
4. Tag the image.
5. Prepare it for Trivy scanning and pushing to Docker Hub.

From this point onward, we'll start assembling the complete end-to-end Jenkins pipeline that automates the entire process.
