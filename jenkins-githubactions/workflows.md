## GitHub Workflows

**Workflows**

* Automate **build, test, and deploy**
* Defined using YAML
* No need to manage a separate CI server

---

## Reusable Workflows (simple)

**Reusable workflows**

* Centralize common CI/CD logic
* Define jobs once, reuse everywhere
* Used as **one job** in other workflows

---

## Key Benefits

1️⃣ **Avoid duplication**
→ No copy-paste of YAML across repos

2️⃣ **Enforce best practices**
→ Same standards for all projects

3️⃣ **Centralized control**
→ Can enforce:

* PR checks
* Reviews
* Auditing

4️⃣ **Faster onboarding**
→ New projects use complex CI/CD with **one line of YAML**

---

## Access Rules (very simple)

* **Private repo** → can reuse **private + public** workflows
* **Public repo** → can reuse **public only** workflows

---

## Limitations of Reusable Workflows (short)

1️⃣ **Max nesting**
→ Up to **10 levels**

2️⃣ **Max reusable workflows**
→ **50** per workflow file (including nested)

3️⃣ **Caller env not shared**
→ Caller `env` ❌ not available in called workflow

4️⃣ **Called env not shared back**
→ Called `env` ❌ not available in caller
→ Use **outputs**

5️⃣ **Sharing common variables**
→ Use `vars` from:

* Org level
* Repo level
* Environment level

6️⃣ **Where they can be called**
→ **Only at job level**, not inside steps

7️⃣ **No `GITHUB_ENV` passing**
→ `GITHUB_ENV` ❌ cannot pass data back to caller

---

## 🧠 One-line interview answer

> “Reusable workflows centralize CI/CD logic, reduce duplication, enforce standards, but have limits on nesting, workflow count, and variable sharing.”

---
Perfect 👍
I’ll give you **both examples**, **simple**, **copy-paste ready**, and **interview-friendly**.

No extra theory. Just **what recruiters want to see**.

---

# 1️⃣ Sample **Jenkinsfile** using Shared Library

### 📁 Your platform repo

```
ci-cd-platform/
└── jenkins/shared-libraries/
    └── vars/
        └── terraformPipeline.groovy
```

### 📄 `terraformPipeline.groovy` (shared library)

```groovy
def call(Map config = [:]) {
    pipeline {
        agent any

        environment {
            AWS_REGION = config.awsRegion ?: 'ap-south-1'
            ENV        = config.environment ?: 'dev'
        }

        stages {
            stage('Checkout') {
                steps {
                    checkout scm
                }
            }

            stage('Terraform Init') {
                steps {
                    sh 'terraform init'
                }
            }

            stage('Terraform Plan') {
                steps {
                    sh "terraform plan -var-file=${ENV}.tfvars"
                }
            }

            stage('Terraform Apply') {
                when {
                    branch 'main'
                }
                steps {
                    sh "terraform apply -auto-approve -var-file=${ENV}.tfvars"
                }
            }
        }
    }
}
```

---

### 📄 Jenkinsfile (application repo)

```groovy
@Library('ci-cd-platform') _

terraformPipeline(
    environment: 'dev',
    awsRegion: 'ap-south-1'
)
```

✅ That’s it
➡ No pipeline logic in app repo
➡ All logic lives in **ci-cd-platform**

---

## 🎤 Interview line

> “Application Jenkinsfiles are very small. They just call shared libraries, which contain all CI/CD logic.”

---

# 2️⃣ Sample **GitHub Actions workflow** calling reusable workflow

---

### 📁 Platform repo

```
ci-cd-platform/
└── github-actions/reusable-workflows/
    └── terraform-plan.yml
```

### 📄 `terraform-plan.yml` (reusable workflow)

```yaml
name: Terraform Plan

on:
  workflow_call:
    inputs:
      environment:
        required: true
        type: string
    secrets:
      AWS_ACCESS_KEY_ID:
        required: true
      AWS_SECRET_ACCESS_KEY:
        required: true

jobs:
  plan:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - uses: hashicorp/setup-terraform@v3

      - name: Terraform Init
        run: terraform init

      - name: Terraform Plan
        run: terraform plan -var-file=${{ inputs.environment }}.tfvars
```

---

### 📄 Application repo workflow

```yaml
name: Terraform CI

on:
  push:
    branches:
      - main

jobs:
  terraform-plan:
    uses: org-name/ci-cd-platform/.github/workflows/terraform-plan.yml@v1.0.0
    with:
      environment: dev
    secrets:
      AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
      AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
```

✅ One line to reuse
✅ Versioned
✅ Secure (secrets stay in app repo)

---

## 🎤 Interview line

> “GitHub repos call reusable workflows using `workflow_call`. The CI logic is centralized and versioned.”

---

## 🧠 Ultra-short comparison (memory note)

| Jenkins        | GitHub Actions    |
| -------------- | ----------------- |
| Shared Library | Reusable Workflow |
| `@Library()`   | `uses:`           |
| Groovy         | YAML              |
| Centralized    | Centralized       |


Reusable workflows must explicitly declare workflow_call. Without it, they cannot be invoked using uses.”