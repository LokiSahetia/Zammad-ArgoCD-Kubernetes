# Zammad Deployment using ArgoCD and Kubernetes

This project demonstrates how to deploy **Zammad Helm Chart** using **ArgoCD** with a **custom `values.yaml`** stored in Git, locally.

---

## Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Setting Up ArgoCD](#setting-up-argocd)
- [Setting Up Git Repository](#setting-up-git-repository)
- [Deploying Zammad](#deploying-zammad)
- [Accessing ArgoCD Dashboard](#accessing-argocd-dashboard)

---

## Overview

We use ArgoCD to manage the deployment of Zammad on Kubernetes. The setup pulls a Helm chart remotely and applies custom configurations using a `values.yaml` stored inside this repository.

---

## Prerequisites

- Kubernetes Cluster
- kubectl installed and configured
- Git installed
- ArgoCD access

---

## Setting Up ArgoCD

1. **Create a Namespace for ArgoCD:**
   ```bash
   kubectl create namespace argocd
   ```

2. **Install ArgoCD:**
   ```bash
   kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
   ```

3. **Verify Pods and Services:**
   ```bash
   kubectl get pods -n argocd
   kubectl get svc -n argocd
   ```

4. **Access ArgoCD Dashboard Locally:**
   ```bash
   kubectl port-forward service/argocd-server 8080:80 -n argocd
   ```

5. **Retrieve Initial Admin Password:**
   ```bash
   kubectl get secret argocd-initial-admin-secret -o yaml -n argocd
   ```

6. **Decode the Password:**
   ```bash
   echo <foo> | base64 -d #replace foo with the password
   ```

---

## Setting Up Git Repository

1. **Create and Navigate to Directory:**
   ```bash
   mkdir gitzammad
   cd gitzammad
   ```

2. **Clone the Repository:**
   ```bash
   git clone https://github.com/LokiSahetia/Zammad-ArgoCD-Kubernetes.git #use your repo link
   cd Zammad-ArgoCD-Kubernetes #your repo folder name
   ```

3. **Create a Folder for Helm Values:**
   ```bash
   mkdir ZammadValuesFolder
   cd ZammadValuesFolder
   ```

4. **Create Custom `values.yaml`:**
   ```bash
   nano values.yaml
   ```
   Add your custom configurations inside.

5. **Create `application.yaml`:**
   Navigate back to the project root and create `application.yaml`.  
   (You can refer to the `application.yaml` example provided in the repo.)

6. **Push Changes to Git:**
   ```bash
   git add .
   git commit -m "Added values.yaml and application.yaml"
   git push origin main
   ```

---

## Deploying Zammad

1. **Create Namespace for Zammad:**
   ```bash
   kubectl create namespace zammad
   ```

2. **Apply the ArgoCD Application Manifest:**
   ```bash
   kubectl apply -f application.yaml
   ```

ArgoCD will now manage the Zammad deployment using the Helm chart with your custom `values.yaml`.

---

## Accessing ArgoCD Dashboard

After port-forwarding, you can access the ArgoCD dashboard at:

```
http://localhost:8080
```

Log in using the username `admin` and the password retrieved earlier.

---

## Notes

- Customize the `values.yaml` according to your environment (like database configuration, ingress setup, etc.). 
- Modify the `application.yaml` if you need to change the sync policy or Helm chart values path.

---

