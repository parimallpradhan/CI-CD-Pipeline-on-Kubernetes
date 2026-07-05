# 📘 Phase 11 – Monitoring Kubernetes & EC2 using Amazon CloudWatch

## 🎯 Objective

Monitor the deployed application and underlying infrastructure using Amazon CloudWatch by:

* Installing the CloudWatch Agent on the EC2 instance
* Collecting system metrics (CPU, Memory, Disk, Network)
* Collecting application and system logs
* Creating CloudWatch Dashboards
* Configuring CloudWatch Alarms
* Sending SNS email notifications
* Monitoring Kubernetes node health

---

# 🏗️ Architecture

```text
                  GitHub
                     │
                     ▼
                  Jenkins
                     │
                     ▼
              Kubernetes Cluster
                     │
          ┌──────────┴──────────┐
          │                     │
      Spring Boot Pods      EC2 Instance
                                    │
                                    ▼
                         CloudWatch Agent
                                    │
               ┌────────────────────┼────────────────────┐
               │                    │                    │
            Metrics              Logs              Alarms
               │                    │                    │
               └────────────────────┼────────────────────┘
                                    │
                                    ▼
                          Amazon CloudWatch
                                    │
                                    ▼
                           SNS Email Notification
```

---

# AWS Services Used

| Service              | Purpose                     |
| -------------------- | --------------------------- |
| Amazon EC2           | Jenkins & Kubernetes Server |
| Amazon CloudWatch    | Monitoring                  |
| CloudWatch Agent     | Metrics Collection          |
| CloudWatch Dashboard | Visualization               |
| CloudWatch Alarm     | Alerting                    |
| Amazon SNS           | Email Notifications         |
| IAM Role             | CloudWatch Permissions      |

---

# Step 1 – Create an IAM Role

Create an IAM Role for EC2.

Attach:

* CloudWatchAgentServerPolicy

Attach the role to your EC2 instance.

📸 Screenshot

* IAM Role
* EC2 IAM Attachment

---

# Step 2 – Install CloudWatch Agent

```bash
sudo apt update

wget https://s3.amazonaws.com/amazoncloudwatch-agent/ubuntu/amd64/latest/amazon-cloudwatch-agent.deb

sudo dpkg -i amazon-cloudwatch-agent.deb
```

Verify:

```bash
/opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -m ec2 -a status
```

📸 Screenshot

---

# Step 3 – Create CloudWatch Agent Configuration

```bash
sudo mkdir -p /opt/aws/amazon-cloudwatch-agent/etc/
```

Create:

```bash
sudo nano /opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json
```

Example:

```json
{
  "metrics": {
    "namespace": "Enterprise-CICD",
    "append_dimensions": {
      "InstanceId": "${aws:InstanceId}"
    },
    "metrics_collected": {
      "cpu": {
        "measurement": [
          "cpu_usage_idle",
          "cpu_usage_user",
          "cpu_usage_system"
        ]
      },
      "disk": {
        "measurement": [
          "used_percent"
        ],
        "resources": [
          "*"
        ]
      },
      "mem": {
        "measurement": [
          "mem_used_percent"
        ]
      }
    }
  },
  "logs": {
    "logs_collected": {
      "files": {
        "collect_list": [
          {
            "file_path": "/var/log/syslog",
            "log_group_name": "jenkins-syslog",
            "log_stream_name": "{instance_id}"
          },
          {
            "file_path": "/var/log/jenkins/jenkins.log",
            "log_group_name": "jenkins-log",
            "log_stream_name": "{instance_id}"
          }
        ]
      }
    }
  }
}
```

📸 Screenshot

---

# Step 4 – Start CloudWatch Agent

```bash
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
-a fetch-config \
-m ec2 \
-c file:/opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json \
-s
```

Check status:

```bash
sudo systemctl status amazon-cloudwatch-agent
```

📸 Screenshot

---

# Step 5 – Verify Metrics

Open:

```
CloudWatch

↓

Metrics

↓

Custom Namespace

↓

Enterprise-CICD
```

You should see:

* CPU Utilization
* Memory Usage
* Disk Usage

📸 Screenshot

---

# Step 6 – Monitor Logs

CloudWatch

↓

Logs

↓

Log Groups

Expected:

```
jenkins-log

jenkins-syslog
```

📸 Screenshot

---

# Step 7 – Create Dashboard

Dashboard Name:

```
Enterprise-CICD-Dashboard
```

Widgets:

* CPU Usage
* Memory Usage
* Disk Usage
* Network In
* Network Out
* Status Check

📸 Screenshot

---

# Step 8 – Create CloudWatch Alarm

Example:

CPU Utilization

Condition:

```
Greater than 80%
```

Period:

```
5 Minutes
```

Action:

```
Send SNS Notification
```

📸 Screenshot

---

# Step 9 – Create SNS Topic

Create:

```
DevOps-Alerts
```

Protocol:

```
Email
```

Endpoint:

```
your-email@gmail.com
```

Confirm the subscription from your email.

📸 Screenshot

---

# Step 10 – Test Alarm

Generate CPU load:

```bash
sudo apt install stress -y

stress --cpu 2 --timeout 300
```

CloudWatch should trigger the alarm and send an email.

📸 Screenshot

---

# Step 11 – Monitor Kubernetes

Check node status:

```bash
kubectl top nodes

kubectl top pods -A
```

If the Metrics Server isn't installed yet:

```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

Verify:

```bash
kubectl top nodes
```

📸 Screenshot

---

# Verification Checklist

* IAM Role attached
* CloudWatch Agent installed
* Metrics collected
* Logs streaming
* Dashboard created
* Alarm configured
* SNS notification received
* Kubernetes metrics available

---

# Screenshots

```
images/

cloudwatch-agent.png
cloudwatch-metrics.png
cloudwatch-dashboard.png
cloudwatch-alarm.png
sns-topic.png
sns-email.png
kubernetes-top-nodes.png
kubernetes-top-pods.png
```

---

# README Update

```markdown
## Phase 11 – Monitoring with Amazon CloudWatch

### Objective

Monitor the infrastructure and application using Amazon CloudWatch.

### AWS Services Used

- Amazon CloudWatch
- CloudWatch Agent
- Amazon SNS
- IAM

### Features

- EC2 Monitoring
- CPU Monitoring
- Memory Monitoring
- Disk Monitoring
- Jenkins Log Collection
- CloudWatch Dashboard
- CloudWatch Alarm
- Email Notifications

### Verification

- Metrics visible
- Logs streaming
- Dashboard working
- Alarm triggered
- Email notification received

📸 Screenshots
```

---

# 🎉 Final Project Architecture

```text
GitHub
   │
   ▼
GitHub Webhook
   │
   ▼
Jenkins
   │
   ├─────────────── Maven Build
   │
   ├─────────────── Unit Test
   │
   ├─────────────── Docker Build
   │
   ├─────────────── Trivy Scan
   │
   ├─────────────── Push Docker Image
   │
   ▼
Kubernetes (kubeadm)
   │
   ├────────────── Deployment
   ├────────────── Service
   ├────────────── Rolling Update
   └────────────── Health Checks
   │
   ▼
Spring Boot Application
   │
   ▼
CloudWatch Agent
   │
   ├────────────── Metrics
   ├────────────── Logs
   ├────────────── Dashboard
   ├────────────── Alarms
   └────────────── SNS Email Alerts
```

This makes your GitHub project a complete **Enterprise End-to-End CI/CD, DevSecOps, Kubernetes, and Monitoring** solution that closely mirrors what many organizations implement in AWS.
