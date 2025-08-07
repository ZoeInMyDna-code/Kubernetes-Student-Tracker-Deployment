
# Student Tracker App - Kubernetes Deployment

This project contains the complete Kubernetes configuration for deploying a sample **Student Tracker** application using:

- Deployments
- Services
- ConfigMaps
- Secrets
- Ingress Controller (NGINX)
- Persistent Volumes and Claims

## 📁 Project Structure

```
.
├── kind-cluster/                      # KIND cluster configuration
│   └── kind-config.yaml              # KIND cluster config
│
├── my-app/                           # Student Tracker App configs
│   ├── myapp.yaml                    # Deployment + Service
│   ├── my-app-service.yaml           # (Optional) Separate Service definition
│   ├── vault-secret.yaml             # Secret object (Vault or static secret)
│   └── student-tracker.yaml          # Complete deployment (includes config)
│
├── ingress/                          # Ingress configuration
│   ├── ingress-nginx-controller-deployment.yaml  # NGINX ingress controller
│   └── student-tracker-ingress.yaml # Ingress rule for the app
```

---

## 🚀 Getting Started

### 1. Create the KIND Cluster

```bash
kind create cluster --config=kind-cluster/kind-config.yaml
```

> Ensure Docker is running before creating the cluster.

---

### 2. Deploy the Student Tracker App

```bash
kubectl apply -f my-app/student-tracker.yaml
```

This includes:
- Deployment
- Service
- ConfigMap (if defined)
- Volume mounts

---

### 3. Set Up Secrets

```bash
kubectl apply -f my-app/vault-secret.yaml
```

---

### 4. Deploy NGINX Ingress Controller

```bash
kubectl apply -f ingress/ingress-nginx-controller-deployment.yaml
```

Wait for the controller pod to be ready:

```bash
kubectl get pods -n ingress-nginx
```

---

### 5. Apply Ingress Rule

```bash
kubectl apply -f ingress/student-tracker-ingress.yaml
```

---

## 🌐 Accessing the App

Get the ingress controller's NodePort or external IP:

```bash
kubectl get service -n ingress-nginx
```

Then access the app via browser or `curl`:

```bash
curl http://<EXTERNAL-IP>:<PORT>
```

---

## 🧠 Notes

- The field `spec.selector` in deployments is immutable. If you make a mistake, delete and re-apply:
  ```bash
  kubectl delete -f ingress/ingress-nginx-controller-deployment.yaml
  kubectl apply -f ingress/ingress-nginx-controller-deployment.yaml
  ```

- Ensure the namespace is created if you’re using one.

---

## ✅ Todo

- [ ] Add TLS support to ingress
- [ ] Configure a real database (PostgreSQL, MongoDB)
- [ ] Add Helm chart support
- [ ] Integrate with CI/CD (GitHub Actions)

---

## 🙌 Author

Atanda Oluwatimileyin  
Cloud Engineer | DevOps Enthusiast | Kubernetes Learner
