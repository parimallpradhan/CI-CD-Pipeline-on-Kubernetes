# 📘 Phase 8 – DevSecOps: Scan Docker Images with Trivy

## 🎯 Objective

In this phase, we will:

* Install Trivy
* Scan the local filesystem
* Scan the Docker image
* Generate HTML reports
* Fail the Jenkins pipeline on HIGH/CRITICAL vulnerabilities
* Prepare for Kubernetes deployment

---

# 🏗️ Architecture

```text id="w3d4q1"
              Jenkins
                  │
                  ▼
            Maven Build
                  │
                  ▼
           Docker Build
                  │
                  ▼
            Trivy Scanner
                  │
        ┌─────────┴─────────┐
        │                   │
   No Critical CVEs    Critical CVEs
        │                   │
        ▼                   ▼
  Push to Docker Hub    Pipeline Failed
```

---

# Step 1 – Install Trivy

Add the official repository:

```bash
sudo apt-get install wget apt-transport-https gnupg lsb-release -y

wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key \
| gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null

echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] \
https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" \
| sudo tee /etc/apt/sources.list.d/trivy.list
```

Update packages:

```bash
sudo apt update
```

Install Trivy:

```bash
sudo apt install trivy -y
```

---

# Step 2 – Verify Installation

```bash
trivy --version
```

Expected:

```text
Version: xx.x.x
```

📸 Screenshot

---

# Step 3 – Update Vulnerability Database

```bash
trivy image --download-db-only
```

This downloads the latest CVE database.

📸 Screenshot

---

# Step 4 – Scan Project Filesystem

Navigate to your project:

```bash
cd springboot-cicd-demo
```

Run:

```bash
trivy fs .
```

Expected:

```text
Target:
springboot-cicd-demo

HIGH : 0

CRITICAL : 0
```

📸 Screenshot

---

# Step 5 – Scan Docker Image

Assuming your image is:

```text
YOUR_DOCKER_USERNAME/springboot-cicd:v1
```

Run:

```bash
trivy image YOUR_DOCKER_USERNAME/springboot-cicd:v1
```

Trivy scans:

* Operating System packages
* Java dependencies
* Application libraries
* Base image vulnerabilities

📸 Screenshot

---

# Step 6 – Generate HTML Report

Install the Trivy HTML template:

```bash
mkdir -p ~/trivy-template
```

Download the template:

```bash
wget https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/html.tpl \
-O ~/trivy-template/html.tpl
```

Generate the report:

```bash
trivy image \
--format template \
--template "@~/trivy-template/html.tpl" \
-o trivy-report.html \
YOUR_DOCKER_USERNAME/springboot-cicd:v1
```

Open the report:

```bash
ls
```

You should see:

```text
trivy-report.html
```

📸 Screenshot

---

# Step 7 – Scan Only HIGH & CRITICAL

```bash
trivy image \
--severity HIGH,CRITICAL \
YOUR_DOCKER_USERNAME/springboot-cicd:v1
```

This keeps the output focused on serious issues.

---

# Step 8 – Fail the Build on Critical Issues

```bash
trivy image \
--exit-code 1 \
--severity HIGH,CRITICAL \
YOUR_DOCKER_USERNAME/springboot-cicd:v1
```

Behavior:

* No HIGH/CRITICAL → Exit code `0`
* HIGH/CRITICAL found → Exit code `1`

This command is ideal for Jenkins because it automatically fails the build when necessary.

---

# Step 9 – Integrate with Jenkins

Add a pipeline stage:

```groovy
stage('Trivy Scan') {
    steps {
        sh '''
        trivy image \
        --exit-code 1 \
        --severity HIGH,CRITICAL \
        YOUR_DOCKER_USERNAME/springboot-cicd:${BUILD_NUMBER}
        '''
    }
}
```

If Trivy detects high-risk vulnerabilities, Jenkins stops the pipeline before deployment.

---

# Step 10 – Verify Reports

You should have:

```text
trivy-report.html
```

and terminal output similar to:

```text
HIGH : 0

CRITICAL : 0
```

📸 Screenshot

---

# Useful Trivy Commands

Filesystem scan:

```bash
trivy fs .
```

Image scan:

```bash
trivy image IMAGE_NAME
```

Config scan:

```bash
trivy config .
```

Repository scan:

```bash
trivy repo https://github.com/your-user/your-repository
```

---

# Verification Checklist

* Trivy installed
* Version verified
* Database downloaded
* Filesystem scan completed
* Docker image scanned
* HTML report generated
* Jenkins pipeline configured to fail on HIGH/CRITICAL vulnerabilities

---

# Screenshots for GitHub

```text
images/

trivy-install.png

trivy-version.png

filesystem-scan.png

docker-image-scan.png

critical-report.png

html-report.png

jenkins-trivy-stage.png
```

---

# README Update

```markdown
## Phase 8 – Security Scan with Trivy

### Objective

Integrate vulnerability scanning into the CI/CD pipeline using Trivy.

### Commands Used

- trivy fs .
- trivy image
- trivy image --exit-code 1
- trivy image --format template

### Verification

- Filesystem scanned
- Docker image scanned
- HTML report generated
- Pipeline fails on HIGH/CRITICAL vulnerabilities

📸 Screenshots
```

---

# 🎉 Phase 8 Completed

Current Pipeline:

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
Trivy Scan
   │
   ▼
Docker Hub
```

---

# 🚀 Phase 9 – Deploy to Kubernetes (Production Style)

This is where everything comes together.

We'll create:

* `deployment.yaml`
* `service.yaml`
* A Kubernetes namespace
* Rolling updates
* Readiness and liveness probes
* Image pull secrets (if your Docker Hub repository is private)
* Jenkins automation to deploy the new image after a successful build and security scan.

By the end of Phase 9, every successful GitHub push will automatically result in a new version of your application running on your Kubernetes cluster. This is the point where the project becomes a complete end-to-end CI/CD and DevSecOps solution.
