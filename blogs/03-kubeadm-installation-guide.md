
### 📄 `04-kubeadm-installation.md`

````markdown
# ⚙️ Kubeadm Installation Guide on AWS

**Author:** Shraddha Modhera  
**Date:** May 2025  
**Read Online:** [Hashnode Blog](https://kubeadm-installation-guide.hashnode.dev/kubeadm-installation-guide-on-aws)

---

If you're looking to build a production-like Kubernetes cluster from scratch on AWS, this guide is for you. Using `kubeadm`, containerd, and Calico, we’ll walk through setting up a fully functional cluster manually on EC2 instances.

---

## ☁️ AWS Setup: Security Group Configuration

Make sure all EC2 instances (master and worker) are in the **same Security Group**.

### 🔐 Create a Security Group

- Navigate to **EC2 > Security Groups > Create Security Group**
- Set:
  - **Name:** Kubernetes-Cluster-SG
  - **Description:** Allow SSH & Kubernetes API
  - **VPC:** Default or custom

#### ➕ Inbound Rules:

| Type       | Port | Source     | Description           |
|------------|------|------------|-----------------------|
| SSH        | 22   | 0.0.0.0/0  | SSH into EC2 instances |
| Custom TCP | 6443 | 0.0.0.0/0  | Kubernetes API Server |

---

## 🔁 On Master and Worker Nodes

### 1. Disable Swap

```bash
sudo swapoff -a
````

---

### 2. Load Required Kernel Modules

```bash
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF

sudo modprobe overlay
sudo modprobe br_netfilter
```

---

### 3. Set Sysctl Parameters

```bash
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF

sudo sysctl --system
```

---

### 4. Install containerd

```bash
sudo apt-get update
sudo apt-get install -y ca-certificates curl

sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
chmod a+r /etc/apt/keyrings/docker.asc

echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] \
https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo \"$VERSION_CODENAME\") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install -y containerd.io
```

---

### 5. Configure containerd

```bash
containerd config default | sed -e 's/SystemdCgroup = false/SystemdCgroup = true/' \
-e 's/sandbox_image = \"registry.k8s.io\\/pause:3.6\"/sandbox_image = \"registry.k8s.io\\/pause:3.9\"/' \
| sudo tee /etc/containerd/config.toml

sudo systemctl restart containerd
sudo systemctl status containerd
```

---

## 🧠 On the Master Node Only

### 1. Initialize Kubernetes Cluster

```bash
sudo kubeadm init
```

---

### 2. Set Up kubeconfig

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

---

### 3. Install Calico CNI

```bash
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.26.0/manifests/calico.yaml
```

---

### 4. Generate Join Command

```bash
kubeadm token create --print-join-command
```

Copy the output and move to the worker nodes.

---

## 🤖 On All Worker Nodes

### Optional: Reset Pre-flight Config

```bash
sudo kubeadm reset pre-flight checks
```

---

### Join the Cluster

Paste the copied command from the master node and add `sudo` and `--v=5`:

```bash
sudo kubeadm join <master-ip>:6443 --token <token> \
--discovery-token-ca-cert-hash sha256:<hash> \
--cri-socket unix:///run/containerd/containerd.sock --v=5
```

---

## ✅ Verify the Cluster

On the master node:

```bash
kubectl get nodes
```

If everything is correct, you'll see your master and worker nodes in the **Ready** state.

---

## 🎉 Done!

You've successfully installed a Kubernetes cluster using `kubeadm`!