
### 📄 `03-minikube-setup.md`

````markdown
# 🐳 Minikube Installation Guide on Ubuntu (2025 Edition)

**Author:** Shraddha Modhera  
**Date:** May 2025  
**Type:** Beginner Tutorial  
**Source Blog:** [Hashnode](https://minikube-k8s-ubuntu-installation.hashnode.dev/minikube-installation-guide-on-ubuntu)

---

Minikube makes it super simple to set up a Kubernetes cluster locally. In this guide, you’ll learn how to install and use Minikube on an Ubuntu system with Docker as the container runtime.

---

## 🔧 Step 1: Update System Packages

```bash
sudo apt update
````

---

## 📦 Step 2: Install Required Dependencies

```bash
sudo apt install -y curl wget apt-transport-https
```

---

## 🐋 Step 3: Install Docker

Docker is the driver for running Kubernetes nodes locally:

```bash
sudo apt install -y docker.io
sudo systemctl enable --now docker
sudo usermod -aG docker $USER && newgrp docker
```

Verify Docker installation:

```bash
docker version
```

---

## 📥 Step 4: Install Minikube

Download and install the Minikube binary:

```bash
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
chmod +x minikube
sudo mv minikube /usr/local/bin/
```

Verify:

```bash
minikube version
```

---

## 🔧 Step 5: Install kubectl

Install `kubectl` to interact with your cluster:

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```

Check version:

```bash
kubectl version --client
```

---

## 🚀 Step 6: Start Your Minikube Cluster

Start the single-node Kubernetes cluster using Docker:

```bash
minikube start --driver=docker --vm=true
```

---

## 🔍 Step 7: Verify Cluster Status

Check if everything’s running:

```bash
minikube status
kubectl get nodes
```

---

## 🛑 Step 8: Stop or Delete Cluster

To stop the cluster:

```bash
minikube stop
```

To delete it completely:

```bash
minikube delete
```

---
