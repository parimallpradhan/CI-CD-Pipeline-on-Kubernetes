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
| Instance Type | m7i-flex.large (recommended)            |
| Storage       | 30 GB gp3                          |
| Key Pair      | Create or use an existing key pair |

**Why `m7i-flex.large`?** Jenkins, Docker, and Kubernetes are memory-intensive. A `t2.micro` or `t3.micro` often struggles with all these services running together.

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

### 1. Sign in to the AWS Management Console.

<img width="1657" height="780" alt="image" src="https://github.com/user-attachments/assets/5088628d-6c97-492d-b09f-6837b608e643" />


### 2. Navigate to **EC2**.

<img width="1387" height="522" alt="image" src="https://github.com/user-attachments/assets/3bdc30a2-5c6c-4134-86cd-6895dfaebc3c" />


### 3. Click **Launch Instance**.

### 4. Name it: `jenkins-k8s-server`.

<img width="1871" height="502" alt="image" src="https://github.com/user-attachments/assets/12bf680c-3838-49bb-b366-c0d196ae91e3" />


### 5. Choose **Ubuntu Server 24.04 LTS**.

<img width="1901" height="641" alt="image" src="https://github.com/user-attachments/assets/ac269ff3-9f1c-48f2-a53b-fe86e946ce32" />


### 6. Select **m7i-flex.large** **8 gib Memory** **2 vcpu** .

<img width="1901" height="381" alt="image" src="https://github.com/user-attachments/assets/130d7848-c610-405c-a1ad-d310095418c4" />


### 7. Create or select an SSH key pair(pem key).

<img width="1028" height="717" alt="image" src="https://github.com/user-attachments/assets/89c909e2-9618-42a1-88ac-3c944e1b9767" />

### 8. Associate the security group created earlier.

<img width="1885" height="553" alt="image" src="https://github.com/user-attachments/assets/df286971-7747-4201-89e6-3ab748c2747d" />

### 9. Attach a **30 GB gp3** root volume.

<img width="1892" height="532" alt="image" src="https://github.com/user-attachments/assets/6d6f7bd0-4453-4993-9f8e-97a0aa079c0a" />


### 10. Launch the instance.
<img width="1908" height="380" alt="image" src="https://github.com/user-attachments/assets/357be181-e04a-4a94-8593-a9ca7e65635f" />



---

## Connect to the Server (Using Mobaxterm to connect EC2  )

### From your local machine:

```bash
ssh -i your-key.pem ubuntu@<EC2_PUBLIC_IP>
```

<img width="1901" height="393" alt="image" src="https://github.com/user-attachments/assets/6dc97495-3b9c-4a72-8ac5-bbbba9c2b020" />

### Verify the operating system:

```bash
cat /etc/os-release
```
<img width="1913" height="491" alt="image" src="https://github.com/user-attachments/assets/ec5fff71-6914-416c-b189-ef40bf717d1c" />

### Expected output should indicate Ubuntu 24.04 LTS.



---

## Update the Server

```bash
sudo apt update
sudo apt upgrade -y
sudo reboot
```
<img width="1911" height="343" alt="image" src="https://github.com/user-attachments/assets/ecf1eaa1-5ff9-4356-a27f-2e61263e7321" />

Reconnect after the reboot and verify connectivity.

<img width="1863" height="597" alt="image" src="https://github.com/user-attachments/assets/771e6dc1-d302-44f0-90df-d4748beb6f92" />



---

