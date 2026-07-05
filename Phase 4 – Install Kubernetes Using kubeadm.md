# 📘 Phase 4 – Install Kubernetes Using kubeadm

## 🎯 Objective

Set up a single-node Kubernetes cluster using `kubeadm` and prepare it for application deployments from Jenkins.

By the end of this phase, you'll have:

* ✅ containerd installed and configured
* ✅ kubeadm installed
* ✅ kubelet installed
* ✅ kubectl installed
* ✅ Kubernetes cluster initialized
* ✅ Networking plugin installed
* ✅ Single-node cluster ready for workloads

---

# 🏗️ Architecture

```text
                AWS EC2 Ubuntu
                      │
     ┌─────────────────────────────────┐
     │ Kubernetes Control Plane Node   │
     │                                 │
     │  kubeadm                        │
     │  kubelet                        │
     │  kubectl                        │
     │  containerd                     │
     │                                 │
     │ Jenkins                         │
     │ Docker                          │
     │ Trivy                           │
     └─────────────────────────────────┘
```

---

# Step 1 – Verify System Requirements

Check RAM:

```bash
free -h
```

Check CPU:

```bash
lscpu
```

Check hostname:

```bash
hostnamectl
```

📸 **Screenshot**

---

# Step 2 – Disable Swap

Kubernetes requires swap to be disabled.

Temporarily disable:

```bash
sudo swapoff -a
```

Verify:

```bash
free -h
```

You should see:

```text
Swap: 0B
```

Disable swap permanently:

```bash
sudo sed -i '/ swap / s/^/#/' /etc/fstab
```

Verify:

```bash
cat /etc/fstab
```

📸 **Screenshot**

---

# Step 3 – Enable Required Kernel Modules

```bash
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF
```

Load them:

```bash
sudo modprobe overlay
sudo modprobe br_netfilter
```

Verify:

```bash
lsmod | grep overlay
lsmod | grep br_netfilter
```

📸 **Screenshot**

---

# Step 4 – Configure Kubernetes Networking

Create:

```bash
sudo tee /etc/sysctl.d/k8s.conf <<EOF
net.bridge.bridge-nf-call-iptables=1
net.bridge.bridge-nf-call-ip6tables=1
net.ipv4.ip_forward=1
EOF
```

Apply:

```bash
sudo sysctl --system
```

Verify:

```bash
sysctl net.ipv4.ip_forward
```

Expected:

```text
net.ipv4.ip_forward = 1
```

📸 **Screenshot**

---

# Step 5 – Install containerd

```bash
sudo apt update
sudo apt install -y containerd
```

Generate the default configuration:

```bash
sudo mkdir -p /etc/containerd
containerd config default | sudo tee /etc/containerd/config.toml
```

Enable `SystemdCgroup`:

```bash
sudo nano /etc/containerd/config.toml
```

Find:

```toml
SystemdCgroup = false
```

Change it to:

```toml
SystemdCgroup = true
```

Restart and enable the service:

```bash
sudo systemctl restart containerd
sudo systemctl enable containerd
```

Verify:

```bash
sudo systemctl status containerd
```

📸 **Screenshot**

---

# Step 6 – Add the Kubernetes Repository

Update packages:

```bash
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl gpg
```

Create the keyring directory:

```bash
sudo mkdir -p /etc/apt/keyrings
```

Download the signing key:

```bash
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.31/deb/Release.key \
| sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
```

Add the repository:

```bash
echo "deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] \
https://pkgs.k8s.io/core:/stable:/v1.31/deb/ /" \
| sudo tee /etc/apt/sources.list.d/kubernetes.list
```

Update:

```bash
sudo apt update
```

📸 **Screenshot**

---

# Step 7 – Install Kubernetes Components

```bash
sudo apt install -y kubelet kubeadm kubectl
```

Prevent automatic upgrades:

```bash
sudo apt-mark hold kubelet kubeadm kubectl
```

Verify:

```bash
kubeadm version
kubectl version --client
kubelet --version
```

📸 **Screenshot**

---

# Step 8 – Initialize the Cluster

Run:

```bash
sudo kubeadm init --pod-network-cidr=192.168.0.0/16
```

This takes a few minutes.

Save the `kubeadm join` command shown at the end. It's needed if you add worker nodes later.

📸 **Screenshot**

---

# Step 9 – Configure kubectl

For the `ubuntu` user:

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

Verify:

```bash
kubectl get nodes
```

Initially, you'll likely see:

```text
NotReady
```

That's expected until a network plugin is installed.

📸 **Screenshot**

---

# Step 10 – Install a CNI Plugin (Calico)

Apply Calico:

```bash
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.29.0/manifests/calico.yaml
```

Wait a few minutes, then verify:

```bash
kubectl get pods -n kube-system
```

📸 **Screenshot**

---

# Step 11 – Allow Scheduling on the Control Plane

Since this is a single-node cluster, remove the control-plane taint:

```bash
kubectl taint nodes --all node-role.kubernetes.io/control-plane-
```

Verify:

```bash
kubectl describe node
```

📸 **Screenshot**

---

# Step 12 – Verify the Cluster

Check the node:

```bash
kubectl get nodes
```

Expected:

```text
NAME          STATUS   ROLES           AGE   VERSION
jenkins-k8s   Ready    control-plane   ...
```

Check namespaces:

```bash
kubectl get ns
```

Check system pods:

```bash
kubectl get pods -A
```

📸 **Screenshots**

* `kubectl get nodes`
* `kubectl get pods -A`
* `kubectl get ns`

---

# Verification Checklist

* Swap disabled
* Kernel modules loaded
* `containerd` running
* `kubeadm`, `kubectl`, and `kubelet` installed
* Cluster initialized
* Calico installed
* Node status is `Ready`
* System pods are `Running`

---

# Screenshots for GitHub

Store these in your repository:

```text
images/
├── swap-disabled.png
├── containerd-status.png
├── kubeadm-version.png
├── kubeadm-init.png
├── kubectl-nodes.png
├── calico-pods.png
├── kube-system-pods.png
└── cluster-ready.png
```

---

# What Happens Next?

Your environment is now ready for the CI/CD pipeline.

## Phase 5 – Configure Jenkins for Kubernetes

We'll:

* Install Maven.
* Configure JDK and Maven in Jenkins.
* Install the Kubernetes CLI plugin.
* Create Docker Hub and Kubernetes credentials in Jenkins.
* Connect Jenkins to your Kubernetes cluster.
* Verify Jenkins can run `kubectl` commands.

After that, we'll start building the actual pipeline that compiles your application, creates a Docker image, scans it with Trivy, pushes it to Docker Hub, and deploys it to Kubernetes automatically. This is where all the pieces come together into a complete end-to-end CI/CD workflow.
