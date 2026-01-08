## 🔹 High-level CI/CD Flow (Big Picture)

![Image](https://miro.medium.com/1%2AlT5Ue1D-AAx3B5orkwQdgw.png)

![Image](https://miro.medium.com/1%2APNGMfMfuB4Jidd-NhH3O8w.jpeg)

![Image](https://user-images.githubusercontent.com/119833411/243173534-59ed9dc5-c410-472f-b450-73e0dca83c94.jpg)

### Tools you chose

* **Jenkins** → CI (build, test, image)
* **Amazon EKS** → Runtime platform
* **Argo CD** → CD (deployment)

---

## 🔹 What Jenkins Does (CI part)

**Jenkins responsibility ends at image creation + Git update**

### Typical Jenkins pipeline steps

1. Developer pushes code to Git
2. Jenkins pipeline triggers
3. Jenkins:

   * Runs tests
   * Builds application
   * Builds Docker image
   * Pushes image to ECR
   * Updates Kubernetes/Helm repo with **new image tag**
4. Jenkins job finishes ✅

👉 **Jenkins NEVER deploys to EKS directly**

---

### Example Jenkinsfile (Realistic & Simple)

```groovy
pipeline {
  agent any

  environment {
    IMAGE_TAG = "${BUILD_NUMBER}"
    ECR_REPO  = "123456789012.dkr.ecr.ap-south-1.amazonaws.com/catalogue"
  }

  stages {
    stage('Build') {
      steps {
        sh 'mvn clean package'
      }
    }

    stage('Docker Build & Push') {
      steps {
        sh """
        docker build -t $ECR_REPO:$IMAGE_TAG .
        docker push $ECR_REPO:$IMAGE_TAG
        """
      }
    }

    stage('Update Helm values') {
      steps {
        sh """
        sed -i 's/tag:.*/tag: "$IMAGE_TAG"/' helm/catalogue/values.yaml
        git commit -am "Update image tag to $IMAGE_TAG"
        git push origin main
        """
      }
    }
  }
}
```

📌 Jenkins work ends here.

---

## 🔹 What Argo CD Does (CD part)

**Argo CD watches Git, not Jenkins**

![Image](https://miro.medium.com/0%2ABiRm9BiQMsZUj_Fv.png)

![Image](https://miro.medium.com/v2/resize%3Afit%3A1200/0%2AdCZakay_NJJqXTU3.png)

![Image](https://argo-cd.readthedocs.io/en/stable/assets/argocd_architecture.png)

### Argo CD responsibilities

* Watches Git repo (Helm or manifests)
* Detects change (new image tag)
* Syncs desired state to EKS
* Keeps cluster **always matching Git**

👉 This is **GitOps**

---

### Example Argo CD Application (Helm-based)

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: catalogue
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/konka/eks-helm-charts.git
    targetRevision: main
    path: catalogue
    helm:
      valueFiles:
        - values.yaml
  destination:
    server: https://kubernetes.default.svc
    namespace: catalogue
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

---

## 🔹 End-to-End Flow (Very Important)

```
Developer → Git Push
        ↓
Jenkins CI
  - Build
  - Test
  - Docker image
  - Push to ECR
  - Update Helm values
        ↓
Git Repo updated
        ↓
Argo CD detects change
        ↓
Argo CD deploys to EKS
```

---

## 🔹 Repo Structure (Best Practice)

### Application repo (used by Jenkins)

```
catalogue-service/
├── Dockerfile
├── pom.xml
├── src/
└── Jenkinsfile
```

### Deployment repo (used by Argo CD)

```
eks-helm-charts/
├── catalogue/
│   ├── Chart.yaml
│   ├── values.yaml
│   └── templates/
├── user/
├── payment/
```

📌 **CI repo ≠ CD repo (clean separation)**

---

## 🔹 Why This Is the Best Model

✅ Jenkins doesn’t need cluster access
✅ Argo CD handles rollback & drift detection
✅ Git is single source of truth
✅ Easy audit & compliance
✅ Scales well for many microservices

---

## 🔹 Common Interview One-liner (Use This 🔥)

> *“We use Jenkins for CI to build and push Docker images and update Helm values in Git. Argo CD follows GitOps and continuously syncs the desired state from Git to EKS.”*

---

## 🔹 Next Steps (Tell me what you want next)

I can help you with:
1️⃣ Jenkins shared library for all microservices
2️⃣ Multi-env (dev / stage / prod) Argo CD setup
3️⃣ Helm chart best practices
4️⃣ Blue-Green or Canary deployment using Argo Rollouts
5️⃣ Complete **production-grade reference architecture**

👉 **What do you want to build next?**
