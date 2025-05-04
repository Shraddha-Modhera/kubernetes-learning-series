Here's the `.md` (Markdown) content for your blog titled **“Getting Started with Kubernetes Using KIND (Kubernetes in Docker)”** based on the published [Hashnode blog link](https://k8s-kind-setup.hashnode.dev/title-getting-started-with-kubernetes-using-kind-kubernetes-in-docker):

---

### 📄 `01-kind-setup.md`

````markdown
# 🚀 Getting Started with Kubernetes Using KIND (Kubernetes in Docker)

**Published by Shraddha Modhera on May 4, 2025**  
🔗 [Read on Hashnode](https://k8s-kind-setup.hashnode.dev/title-getting-started-with-kubernetes-using-kind-kubernetes-in-docker)

---

If you're just starting out with Kubernetes and want a quick, local, and resource-friendly setup, **KIND (Kubernetes IN Docker)** is a perfect tool. This guide will walk you through installing KIND, creating a cluster, and verifying it's working.

---

## ✨ Why Use KIND?

- 🐳 It runs Kubernetes clusters inside Docker containers — no VMs needed.
- 🧪 Ideal for local testing, development, or CI pipelines.
- 💨 Fast to spin up, easy to destroy.

---

## ✅ Prerequisites

- Ubuntu/Linux system
- Docker installed
- Basic shell knowledge

---

## 🛠 Step 1: Install KIND and kubectl

Create an installation script:

```bash
nano ./install_kind.sh
````

Paste the following:

```bash
#!/bin/bash

[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.27.0/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind

VERSION="v1.30.0"
URL="https://dl.k8s.io/release/${VERSION}/bin/linux/amd64/kubectl"
INSTALL_DIR="/usr/local/bin"

curl -LO "$URL"
chmod +x kubectl
sudo mv kubectl $INSTALL_DIR/
kubectl version --client

rm -f kubectl
rm -rf kind

echo "kind & kubectl installation complete."
```

Then run:

```bash
chmod +x install_kind.sh
./install_kind.sh
sudo apt-get update
```

Install Docker if not already:

```bash
sudo apt-get install docker.io
sudo usermod -aG docker $USER && newgrp docker
```

---

## ⚙️ Step 2: Create KIND Cluster Configuration

Create a directory and config file:

```bash
mkdir kind-cluster
cd kind-cluster
nano kind-cluster-config.yaml
```

Paste the following:

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4

nodes:
- role: control-plane
  image: kindest/node:v1.31.2
- role: worker
  image: kindest/node:v1.31.2
- role: worker
  image: kindest/node:v1.31.2
  extraPortMappings:
  - containerPort: 80
    hostPort: 80
    protocol: TCP 
  - containerPort: 443
    hostPort: 443   
    protocol: TCP 
```

Now create the cluster:

```bash
kind create cluster --name=tws-cluster --config=kind-cluster-config.yaml
```

Verify it:

```bash
kubectl cluster-info --context kind-tws-cluster
kubectl get nodes
```

---

## ⚠️ Common Error: Connection Refused

If you see:

```
kubectl get nodes
E0504... connect: connection refused
```

Run this fix:

```bash
kubectl config use-context kind-tws-cluster
```

---

## 🧪 What’s Next?

Try deploying something simple:

```bash
kubectl run nginx --image=nginx
```

Want to learn how Kubernetes really works under the hood?
👉 Check out my next blog on [Kubernetes Architecture](https://k8s-kind-setup.hashnode.dev)

