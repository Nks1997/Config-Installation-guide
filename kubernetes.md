# Kubernetes Setup Guide (Linux)

This document contains the steps to install kubectl and Minikube on a Linux machine.

---

## Kubectl Installation (Binary Method)

### Step 1: Download kubectl binary
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```

### Step 2: Install kubectl
```bash
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

---

## Kubectl Installation (APT Method)

### Step 1: Update system and install prerequisites
```bash
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl gnupg
```

### Step 2: Create keyrings directory (optional)
```bash
sudo mkdir -p -m 755 /etc/apt/keyrings
```

### Step 3: Add Kubernetes signing key
```bash
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.35/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
```
### Step 4: Add Kubernetes apt repository
```bash
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.35/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
```

### Step 5: Update apt package index and install kubectl
```bash
sudo apt-get update
sudo apt-get install -y kubectl
```
---

# Minikube Installation (Linux) - debian pkg

## Step 1: Install dependencies
```bash
sudo apt update  
sudo apt install -y curl apt-transport-https conntrack
```

## Step 2: Download Minikube binary
```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
```

## Step 3: Install Minikube
```bash
sudo dpkg -i minikube_latest_amd64.deb
```

## Step 4: Verify Minikube installation
```bash
minikube version
```

## Step 5: Start Minikube cluster
```bash
minikube start
```

## Step 6: Verify Kubernetes cluster nodes
```bash
kubectl get nodes
```



