# 📘 Phase 2 – Install & Configure Jenkins on AWS EC2

## 🎯 Objective

Install Jenkins on an Ubuntu EC2 instance and prepare it for CI/CD.

By the end of this phase, you'll have:

* ✅ Java Installed
* ✅ Jenkins Installed
* ✅ Jenkins Running
* ✅ Jenkins Accessible via Browser
* ✅ Initial Admin User Created

---

# 🏗 Architecture

```text
                 AWS EC2 Ubuntu
                      │
              ┌───────────────┐
              │     Jenkins   │
              │ Port : 8080   │
              └───────────────┘
```

---

# Step 1 – Verify the Server

Check the hostname:

```bash
hostnamectl
```

Expected Output:

```text
Operating System: Ubuntu 24.04 LTS
Architecture: x86-64
```

📸 **Screenshot**

* Terminal showing hostname

---

# Step 2 – Update Packages

```bash
sudo apt update
sudo apt upgrade -y
```

Why?

* Updates package index
* Installs latest security patches

📸 **Screenshot**

* Successful update

---

# Step 3 – Install Java

Jenkins requires Java.

Install OpenJDK 21:

```bash
sudo apt install fontconfig openjdk-21-jdk -y
```

Verify:

```bash
java -version
```

Expected:

```text
openjdk version "21"
```

📸 **Screenshot**

* Java Version

---

# Step 4 – Add Jenkins Repository

Import Jenkins GPG Key

```bash
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
```

Add Repository

```bash
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
https://pkg.jenkins.io/debian-stable binary/ | \
sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
```

Update Repository

```bash
sudo apt update
```

📸 **Screenshot**

* Repository added successfully

---

# Step 5 – Install Jenkins

```bash
sudo apt install jenkins -y
```

This installs:

* Jenkins
* Required dependencies
* Jenkins service

📸 **Screenshot**

* Installation completed

---

# Step 6 – Start Jenkins

```bash
sudo systemctl start jenkins
```

Enable Jenkins on boot

```bash
sudo systemctl enable jenkins
```

Check Status

```bash
sudo systemctl status jenkins
```

Expected:

```text
Active: active (running)
```

📸 **Screenshot**

* Jenkins running

---

# Step 7 – Verify Jenkins Port

```bash
sudo ss -tulpn | grep 8080
```

Expected:

```text
LISTEN
0.0.0.0:8080
```

📸 **Screenshot**

---

# Step 8 – Allow Port 8080

Go to

```
AWS Console

↓

EC2

↓

Security Groups

↓

Inbound Rules

↓

Add Rule
```

| Type       | Port |
| ---------- | ---- |
| Custom TCP | 8080 |

Source

```
0.0.0.0/0
```

(For learning only.)

📸 **Screenshot**

* Security Group

---

# Step 9 – Access Jenkins

Open

```
http://YOUR_PUBLIC_IP:8080
```

You'll see

```
Unlock Jenkins
```

📸 **Screenshot**

---

# Step 10 – Get Administrator Password

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Copy the password.

Paste it into the browser.

📸 **Screenshot**

---

# Step 11 – Install Suggested Plugins

Click

```
Install Suggested Plugins
```

Jenkins installs

* Git
* Pipeline
* Maven
* Docker
* SSH
* Credentials
* Blue Ocean (optional)

This may take a few minutes.

📸 **Screenshot**

---

# Step 12 – Create Admin User

Example

| Field     | Value                                   |
| --------- | --------------------------------------- |
| Username  | admin                                   |
| Password  | ********                                |
| Full Name | Your Name                               |
| Email     | [your@email.com](mailto:your@email.com) |

Click

```
Save and Continue
```

📸 **Screenshot**

---

# Step 13 – Jenkins Dashboard

After login

You'll see

```
Dashboard
```

📸 **Screenshot**

---

# Verification Commands

Check Jenkins status:

```bash
sudo systemctl status jenkins
```

Check Java version:

```bash
java -version
```

Check Jenkins version:

```bash
dpkg -l | grep jenkins
```

---

# Common Issues

### Jenkins not starting

```bash
sudo journalctl -u jenkins -f
```

---

### Java not found

```bash
java -version
```

Reinstall Java if necessary.

---

### Cannot access Jenkins

Check firewall:

```bash
sudo ufw status
```

Check port:

```bash
sudo ss -tulpn | grep 8080
```

Verify the EC2 Security Group allows TCP port 8080.

---

# Screenshots for GitHub

Capture these images:

* EC2 instance running
* Java version
* Jenkins installation
* Jenkins service status
* Security Group (8080)
* Unlock Jenkins page
* Initial admin password command
* Plugin installation
* Admin user creation
* Jenkins dashboard

Store them like this:

```text
images/
│
├── ec2-instance.png
├── java-version.png
├── jenkins-install.png
├── jenkins-status.png
├── security-group.png
├── unlock-jenkins.png
├── install-plugins.png
├── create-admin.png
└── jenkins-dashboard.png
```

---

# README Section

Once this phase is complete, your `README.md` will include:

* Objective
* Why Jenkins?
* Installation steps
* Commands used
* Verification
* Troubleshooting
* Screenshots
* Key learning points

---

## ✅ Phase 2 Complete

At this point, you'll have a fully functional Jenkins server ready to automate your software delivery pipeline.

### **Next Phase (Phase 3): Install Docker & Configure Jenkins to Build Docker Images**

In Phase 3, we'll:

* Install Docker.
* Add the `jenkins` user to the `docker` group.
* Verify Docker commands from Jenkins.
* Prepare the environment for building and pushing container images. This is where your CI/CD pipeline begins to take shape.
