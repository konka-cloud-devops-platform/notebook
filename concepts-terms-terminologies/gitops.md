# Gitops
Use Git as single source of truth the idea behind we version controlled application why can't we do infra code and configuration

You define Desired state in git then reconciliation tools make desired state into current state

- In Deployment no need to give extra priveleges to deploy application into k8s cluster
- git system applied to configuration like branching,reviews,PR's,auditing ...
- easy to rollback

# GitOps (Simple Explanation)

GitOps is the idea of using **Git as the single source of truth** not only for application code, but also for **infrastructure and configuration**.

Earlier, we version-controlled application code.
GitOps asks: **why not version-control infrastructure and configuration the same way?**

---

## How GitOps works

* You define the **desired state** of the system in Git
* A **reconciliation tool** continuously checks the cluster
* If the current state is different from Git, it automatically fixes it

---

## Key Benefits of GitOps

### 1️⃣ No direct deployment access to cluster

* CI/CD pipelines do not need admin access to Kubernetes
* GitOps tools pull changes from Git and apply them

✅ Better security
✅ Fewer human mistakes

---

### 2️⃣ Git becomes the control system

Configuration changes follow Git rules:

* Branching
* Pull requests
* Code reviews
* Audit history

✅ Every change is traceable
✅ Easy to know who changed what and why

---

### 3️⃣ Easy rollback

* Rollback = revert a Git commit
* GitOps tool automatically restores the previous state

✅ Fast recovery
✅ Predictable rollbacks

---

## Simple GitOps Definition (Interview Ready)

> GitOps is a deployment model where Git stores the desired state, and a controller continuously reconciles the live system to match Git.

---

## One-line Summary

* Git defines the state
* System enforces the state
* Reconciliation keeps everything in sync


## How you should say this in interviews (natural)

> “With GitOps, we treat infrastructure and configuration like code. Git becomes the single source of truth, and reconciliation tools continuously ensure the cluster matches what is defined in Git. This gives us better security, auditing, and easy rollbacks.”

---