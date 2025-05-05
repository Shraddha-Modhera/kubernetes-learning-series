
### 📄 `02-kubernetes-architecture.md`

````markdown
# 🧠 Understanding Kubernetes Architecture: A Beginner's Guide

**Author:** Shraddha Modhera  
**Date:** May 4, 2025  
**Blog Link:** [Hashnode Blog](https://k8s-architecture-devops.hashnode.dev/title-understanding-kubernetes-architecture-a-beginners-guide)

---

Kubernetes is a powerful system for managing containerized applications. But before diving into YAML files, pods, and deployments, it’s important to understand how Kubernetes is structured under the hood.

This blog breaks down the **Kubernetes architecture** in a simple, visual, and beginner-friendly way.

---

## 🌟 What is Kubernetes?

Kubernetes (also known as **K8s**) is an open-source platform for automating deployment, scaling, and management of containerized applications.

It groups containers into **logical units** and manages their lifecycle across a cluster of machines.

---

## 💡 Why is it called K8s?

The name “Kubernetes” contains 10 letters.  
**K8s** is a shorthand: K + 8 letters in between + s → Kubernetes.

(Just like `i18n` is for internationalization.)

---

## ⚓ Why is the logo a ship wheel?

The **Kubernetes logo** is a 7-spoke ship's wheel, representing its role as the **captain of containerized ships**.

- Containers = Shipping containers (Docker)
- Kubernetes = Greek word for *helmsman* or *pilot*
- The ship’s wheel = Steering the fleet (your containers)

---

## ⚖️ The Two Pillars of Kubernetes

Kubernetes architecture is divided into two major components:

### 🧠 Control Plane (The Brain)

Manages the overall cluster state and decisions.

**Components:**
- **API Server**: Gateway for all `kubectl` commands.
- **Scheduler**: Decides where pods will run.
- **Controller Manager**: Keeps everything aligned with the desired state.
- **etcd**: A distributed key-value store to hold cluster data.

---

### 💪 Worker Nodes (The Muscle)

Run the containerized apps.

**Components:**
- **Kubelet**: Ensures containers in a pod are running as expected.
- **Kube Proxy**: Handles networking and forwarding requests.
- **Container Runtime**: Software like Docker or containerd that actually runs the containers.

---

## 🔁 How It All Works: Flow Example

When you run:

```bash
kubectl run nginx --image=nginx
````

This is what happens behind the scenes:

1. The command goes to the **API Server**
2. The **Scheduler** assigns a node
3. The **Kubelet** on that node pulls the image
4. **Kube Proxy** enables network communication
5. The **Controller Manager** ensures the pod is running

---

## ✈️ Real-World Analogy: Air Traffic Control

Imagine Kubernetes as an airport:

* API Server → Control Tower
* Scheduler → Air Traffic Controller
* etcd → Flight Schedule Board
* Controller Manager → Flight Operations
* Nodes → Runways
* Pods → Airplanes
* Kubelet → Ground Staff
* Kube Proxy → Navigation & Gate Connections

---

## 📚 Bonus: Fun Facts

* Kubernetes originated at Google, inspired by their internal tool **Borg**
* Donated to CNCF (Cloud Native Computing Foundation)
* The word *Kubernetes* means *helmsman* in Greek

---

## 🧪 What’s Next?

In the next part, we’ll start applying YAML files using kubectl and build real pods, namespaces, and deployments.

You can try this on:

* Minikube (locally)
* KIND (Kubernetes IN Docker)
* Cloud-based clusters (AWS/GKE)

---

