# ☸️ Kubernetes (K8s) – Learning Journey (Class 33–38)

This README captures everything I’ve learned about **Kubernetes (K8s)** — from setting up local clusters with Minikube to managing cloud-native infrastructure on AWS with KOPS. Includes deployment strategies, replicas, services, and monitoring with Prometheus and Grafana.

---

## 🌐 Kubernetes vs Docker Overview

| Docker                          | Kubernetes (K8s)                   |
|----------------------------------|------------------------------------|
| Cluster → Node → Container → App | Cluster → Node → Pod → Container → App |
| Docker handles containers        | K8s orchestrates multiple containers |
| Limited scaling                  | Auto-scaling, rolling updates      |

> 🔥 **Note:** Kubernetes **doesn't communicate directly with containers**. It interacts with **Pods**, which wrap containers.

---

## 🧱 Core Kubernetes Components

| Component     | Description                                      |
|---------------|--------------------------------------------------|
| **Cluster**   | Group of master and worker nodes                 |
| **Node**      | VM or physical machine inside a cluster          |
| **Pod**       | Smallest unit in K8s, runs 1+ containers          |
| **ReplicaSet**| Ensures a set number of pod replicas             |
| **Deployment**| Manages updates and rollbacks of ReplicaSets     |
| **Service**   | Exposes application (NodePort, LoadBalancer, etc)|

---

## ⚙️ Minikube Setup (Local Cluster)

Install Minikube:
```bash
sudo curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo mv minikube-linux-amd64 /usr/local/bin/minikube
sudo chmod +x /usr/local/bin/minikube
```

Start cluster:
```bash
minikube start
kubectl get nodes
```

---

## ✍️ Pod Creation – Two Methods

1. **Imperative**:
```bash
kubectl run nginx --image=nginx
```

2. **Declarative** (YAML file):
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
    - name: mycontainer
      image: nginx
```
```bash
kubectl apply -f pod.yaml
```

---

## 🔁 ReplicaSet – High Availability

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-rs
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
        - name: nginx
          image: nginx
```

💡 Auto-heals if any pod is deleted.

---

## 🚀 Deployments – Version Control + Rollbacks

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deploy
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
        - name: nginx
          image: nginx:latest
```

Commands:
```bash
kubectl rollout status deployment myapp-deploy
kubectl rollout undo deployment myapp-deploy
```

---

## 🌍 Services – Expose Your App

### NodePort (manual port mapping)
```yaml
type: NodePort
ports:
  - port: 80
    targetPort: 80
    nodePort: 30011
```

### LoadBalancer (production-grade, DNS)
```yaml
type: LoadBalancer
ports:
  - port: 80
    targetPort: 80
```

> ✅ LoadBalancer handles port conflicts and distributes traffic efficiently.

---

## ☁️ KOPS – Kubernetes on AWS

KOPS is used to automate K8s cluster setup in AWS cloud.

### 🔧 Setup Steps

1. Configure AWS CLI:
```bash
aws configure
```

2. Create KOPS script:
```bash
vim kops.sh   # Paste from all-setup repo
```

3. Update `.bashrc`:
```bash
source ~/.bashrc
export KOPS_STATE_STORE=s3://your-k8s-bucket
sh kops.sh
```

KOPS automatically sets up:
- EC2 Instances
- VPC, Subnets
- Security Groups
- IAM Roles
- Routing tables
- Internet Gateways

### 🔁 Scaling:

To scale worker nodes:
- Edit the instance type
- KOPS will create new instances and remove old ones one by one

---

## 🧹 Cleanup

To delete a cluster:
```bash
kops delete cluster --name <your-cluster-name> --yes
```

---

## 📊 Monitoring with Prometheus & Grafana (Class 38)

Set up Prometheus + Grafana using script from your **All-Setup GitHub Repo**.

- Prometheus: metrics collector
- Grafana: visualization dashboard

Install and expose services to access UI dashboards.

---

## 📚 Summary of Classes

| Class | Topic                                 |
|-------|----------------------------------------|
| 33    | Kubernetes Basics: Pods, Nodes         |
| 35    | ReplicaSet, Deployment                 |
| 36    | KOPS AWS Setup                         |
| 37    | KOPS Automation (bash, scripts)        |
| 38    | Monitoring: Prometheus + Grafana       |

---

## 🏁 Final Thoughts

With Kubernetes, I now understand how to:
- Run scalable apps
- Auto-recover from failures
- Perform rolling updates
- Automate entire infra setup on AWS

☸️ K8s is the backbone of modern DevOps.  
📌 Let’s go — next up: **Terraform or Helm!**

⭐ If you found this helpful, star the repo and keep building 🚀
