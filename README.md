# 📌 Kubernetes Manifests for Microservices Deployment

### This repository contains Kubernetes manifests and Helm charts to deploy a microservices-based application, including authentication, trading, and frontend services. It also includes MySQL as the database and uses External Secrets Operator to manage secrets securely.

### 🏗️ Project Structure

```
.
├── auth-service
│   └── helm
│       ├── Chart.yaml
│       ├── templates
│       │   ├── deployment.yaml
│       │   ├── hpa.yaml
│       │   └── service.yaml
│       └── values.yaml
├── cluster-secret-store.yaml
├── deploy_all_services.sh
├── frontend
│   └── helm
│       ├── Chart.yaml
│       ├── templates
│       │   ├── deployment.yaml
│       │   ├── hpa.yaml
│       │   ├── ingress.yaml
│       │   └── service.yaml
│       └── values.yaml
├── mysql
│   └── helm
│       ├── Chart.yaml
│       ├── files
│       │   └── init.sql
│       ├── templates
│       │   ├── db-external-secret.yaml
│       │   ├── init-configmap.yaml
│       │   ├── service.yaml
│       │   ├── statefulset.yaml
│       │   └── storage-class.yaml
│       └── values.yaml
├── README.md
└── trade_service
    └── helm
        ├── Chart.yaml
        ├── templates
        │   ├── deployment.yaml
        │   ├── hpa.yaml
        │   └── service.yaml
        └── values.yaml
```

## 🚀 Deployment Steps
**1️⃣Store Database Credentials in AWS Secrets Manager:**

    ```
    aws secretsmanager create-secret --name db-secret --secret-string '{
    "DB_USER": "trading_user",
    "DB_PASSWORD": "trading_pass",
    "DB_DATABASE": "trading_db",
    "ROOT_PASSWORD": "securepassword"
    }' --region us-east-1
    ```

**2️⃣Setup External Secrets Operator**
```
kubectl apply -f cluster-secret-store.yaml
```

    kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.3.0/deploy/static/provider/cloud/deploy.yaml
    ```