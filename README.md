
```
wordpress-mysql-kubernetes-aws/
├── README.md
├── architecture/
│   └── Project Reference Architecture.png
├── screenshots/
│   
├── mysql/
│   ├── secret.yaml
│   ├── mysql-sc.yaml
│   ├── mysql-pvc.yaml
│   ├── mysql-app.yaml
│   └── mysql-svc.yaml
├── wordpress/
│   ├── wordpress-sc.yaml
│   ├── wordpress-pv.yaml
│   ├── wordpress-pvc.yaml
│   ├── wordpress-app.yaml
│   └── wordpress-svc.yaml
└── LICENSE
```


## 🚀 WordPress & MySQL on Kubernetes using AWS (EBS & EFS)

### 📌 Project Overview

This project demonstrates deploying a **production-like, highly available WordPress application** on a **self-managed Kubernetes cluster (kubeadm)** running on **AWS infrastructure**.

The project focuses on **real-world storage integration**, **AWS IAM**, and **Kubernetes CSI drivers**, following best practices used in cloud-native environments.

---

## 🧱 Architecture Overview

**Core Components:**

* Self-managed Kubernetes cluster (kubeadm)
* MySQL database with persistent storage on **Amazon EBS**
* WordPress application with shared storage on **Amazon EFS**
* AWS LoadBalancer service for external access
* AWS CSI Drivers (EBS & EFS)

📷 *Architecture Diagram*
*`Architecture/Project Reference Architecture.png`*

---

## 🔐 AWS IAM & Security Design

### IAM Configuration

* IAM User created with permissions:

  * `AmazonEBSCSIDriverPolicy`
  * `AmazonElasticFileSystemFullAccess`
* IAM Role attached to Kubernetes nodes for CSI drivers
* Custom IAM policy used for **EFS Access Points**

### Kubernetes Secrets

* MySQL root password stored as Kubernetes Secret
* AWS credentials stored securely in `kube-system` namespace

---

## 📂 Storage Architecture

| Component | AWS Service | Access Mode         | Purpose                     |
| --------- | ----------- | ------------------- | --------------------------- |
| MySQL     | Amazon EBS  | ReadWriteOnce (RWO) | Database persistent storage |
| WordPress | Amazon EFS  | ReadWriteMany (RWX) | Shared content storage      |

✔ Dynamic provisioning via **CSI Drivers**
✔ Persistent storage survives pod restarts
✔ EFS enables horizontal scaling of WordPress

---

## ⚙️ Deployment Flow (Step-by-Step)

### 1️⃣ Verify CSI Drivers

```bash
kubectl get csidriver
kubectl get csinode
```

---

### 2️⃣ Deploy MySQL (Amazon EBS)

```bash
kubectl apply -f mysql/secret.yaml
kubectl apply -f mysql/mysql-sc.yaml
kubectl apply -f mysql/mysql-pvc.yaml
kubectl apply -f mysql/mysql-app.yaml
kubectl apply -f mysql/mysql-svc.yaml
```

✔ EBS volume is dynamically created on AWS
✔ PVC transitions from `Pending` → `Bound`

---

### 3️⃣ Deploy WordPress (Amazon EFS)

```bash
kubectl apply -f wordpress/wordpress-sc.yaml
kubectl apply -f wordpress/wordpress-pv.yaml
kubectl apply -f wordpress/wordpress-pvc.yaml
kubectl apply -f wordpress/wordpress-app.yaml
kubectl apply -f wordpress/wordpress-svc.yaml
```

✔ EFS Access Point used
✔ Shared RWX storage across replicas

---

## 📈 High Availability & Scalability

* WordPress deployed with multiple replicas(Optional)

```bash
kubectl scale deploy wordpress --replicas=2
```

* Shared EFS ensures:

  * Same files across all pods
  * No data inconsistency
* AWS LoadBalancer distributes traffic automatically

---

## 🌐 Application Access

```bash
kubectl get svc wordpress
```

Access WordPress via:

```
http://<NODE-IP>:<NODE-PORT>
```

---

## 🛠️ Technologies Used

* Kubernetes (kubeadm)
* AWS (EC2, EBS, EFS, IAM)
* CSI Drivers (EBS & EFS)
* Docker
* Linux (Ubuntu)
* WordPress & MySQL

---

## 📜 License

This project is licensed under the **MIT License**.

---

## 👤 Author

**Mohanad Faisal**
DevOps Engineer | Kubernetes | AWS

