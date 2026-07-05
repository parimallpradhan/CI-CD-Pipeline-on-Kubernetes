# 📘 Phase 3 – Install Docker & Integrate with Jenkins

## 🎯 Objective

Install Docker on the Jenkins server and configure Jenkins to build and manage container images.

### Commands Used
- Install Docker
- Enable Docker service
- Add Jenkins to the Docker group
- Verify Docker functionality

### Verification
- Docker version
- Docker service status
- Hello World container
- Jenkins Docker access

---

# 🏗 Architecture

```text
                  AWS EC2
                     │
      ┌──────────────┼──────────────┐
      │              │              │
   Jenkins        Docker        Java/Maven
      │
      ▼
Build Docker Images
```

---

# Step 1 – Install Docker

Update package information:

```bash
sudo apt update
```

Install Docker:

```bash
sudo apt install docker.io -y
```

Verify the installation:

```bash
docker --version
```

Expected output:

```text
Docker version 28.x.x
```

📸 **Screenshot**

* Docker version

---

# Step 2 – Start Docker Service

Start Docker:

```bash
sudo systemctl start docker
```

Enable Docker at boot:

```bash
sudo systemctl enable docker
```

Check its status:

```bash
sudo systemctl status docker
```

Expected:

```text
Active: active (running)
```

📸 **Screenshot**

* Docker service status

---

# Step 3 – Verify Docker

Run the official test container:

```bash
sudo docker run hello-world
```

Expected output includes:

```text
Hello from Docker!
```

📸 **Screenshot**

---

# Step 4 – Allow Jenkins to Use Docker

By default, the Jenkins service **cannot** access the Docker daemon.

Add the Jenkins user to the Docker group:

```bash
sudo usermod -aG docker jenkins
```

Also add the Ubuntu user:

```bash
sudo usermod -aG docker ubuntu
```

Verify:

```bash
groups jenkins
```

Expected:

```text
jenkins : jenkins docker
```

📸 **Screenshot**

---

# Step 5 – Restart Services

Restart Docker:

```bash
sudo systemctl restart docker
```

Restart Jenkins:

```bash
sudo systemctl restart jenkins
```

Check Jenkins status:

```bash
sudo systemctl status jenkins
```

---

# Step 6 – Test Docker as Jenkins

Switch to the Jenkins user:

```bash
sudo su - jenkins
```

Run:

```bash
docker ps
```

Expected:

```text
CONTAINER ID
```

If you get **permission denied**, restart Jenkins and log out/in to refresh group membership.

Exit:

```bash
exit
```

📸 **Screenshot**

---

# Step 7 – Install Docker Plugin in Jenkins

Open:

```text
Manage Jenkins
        ↓
Plugins
        ↓
Available Plugins
```

Search for:

* Docker
* Docker Pipeline

Install both plugins.

Restart Jenkins if prompted.

📸 **Screenshot**

---

# Step 8 – Configure Docker in Jenkins

Navigate to:

```text
Manage Jenkins
        ↓
Tools
```

(We'll configure additional tools like Maven and JDK in a later phase.)

For now, ensure Docker is available on the host:

```bash
which docker
```

Expected:

```text
/usr/bin/docker
```

📸 **Screenshot**

---

# Step 9 – Verify Docker Images

Run:

```bash
docker images
```

Initially, it may be empty:

```text
REPOSITORY   TAG   IMAGE ID
```

After running `hello-world`, you'll see that image listed.

📸 **Screenshot**

---

# Step 10 – Verify Docker Containers

```bash
docker ps -a
```

Expected:

```text
hello-world
Exited (0)
```

📸 **Screenshot**

---

# Useful Docker Commands

```bash
docker version
docker info
docker images
docker ps
docker ps -a
docker system df
docker system prune -f
```

---

# Common Issues

## Permission denied while connecting to the Docker daemon

```text
Got permission denied while trying to connect to the Docker daemon socket
```

Fix:

```bash
sudo usermod -aG docker jenkins
sudo systemctl restart docker
sudo systemctl restart jenkins
```

---

## Docker service not running

Check:

```bash
sudo systemctl status docker
```

Start it:

```bash
sudo systemctl start docker
```

---

## Docker daemon not found

Verify:

```bash
which docker
docker --version
```

---

# Verification Checklist

* Docker installed
* Docker service running
* `hello-world` container executed
* Jenkins user added to the Docker group
* Jenkins restarted
* Docker Pipeline plugin installed
* Jenkins can execute Docker commands

---

# Screenshots for GitHub

Save these under:

```text
images/
│
├── docker-version.png
├── docker-status.png
├── hello-world.png
├── docker-group.png
├── docker-plugin.png
├── docker-images.png
└── docker-containers.png
```
---

# 🎯 Phase 3 Complete

At this point you have:

* ✅ Jenkins
* ✅ Java
* ✅ Docker
* ✅ Jenkins integrated with Docker

---

# 🚀 Next Phase – Kubernetes

adm** to create a single-node Kubernetes cluster on the EC2 instance. It exposes you to the same components used in production—`containerd`, `kubelet`, `kubeadm`, `kubectl`, and the control plane—which gives you stronger interview talking points than a Minikube-based setup. From there, Jenkins will deploy directly to Kubernetes as part of the CI/CD pipeline.
