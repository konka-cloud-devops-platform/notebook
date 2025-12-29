This is exactly what you use when migrating:

* Manual → Terraform
* Terraform v0.12 → latest
* On-prem → AWS
* One Terraform structure → another

You can **copy–paste directly**.

---

## terraform-migration-checklist.md

---

## Terraform Migration Checklist

Use this checklist when **moving existing infrastructure into Terraform** or **changing Terraform structure, versions, or environments**.

---

## 1️⃣ Migration Planning (BEFORE TOUCHING CODE)

* [ ] Identify **what is being migrated**

  * VMs
  * VPC
  * EKS
  * RDS
  * SGs
* [ ] Decide migration scope (partial vs full)
* [ ] Freeze infrastructure changes during migration
* [ ] Take backups (snapshots, DB backups, configs)
* [ ] Identify downtime requirements (if any)

---

## 2️⃣ State Strategy (CRITICAL)

* [ ] Decide where state will live (S3 / Terraform Cloud)
* [ ] One state per environment
* [ ] Enable state locking
* [ ] Enable state encryption
* [ ] Backup existing state before migration

🚨 **Never migrate without a state backup**

---

## 3️⃣ Provider & Version Readiness

* [ ] Pin Terraform version
* [ ] Pin provider versions
* [ ] Review breaking changes between versions
* [ ] Test migration in non-prod first

Example:

```hcl
terraform {
  required_version = "~> 1.6"
}
```

---

## 4️⃣ Code Structure Migration

* [ ] Separate `modules/`, `live/`, `environments/`
* [ ] Move reusable logic into modules
* [ ] Keep root module thin
* [ ] Remove hardcoded values
* [ ] Move environment-specific values to tfvars

---

## 5️⃣ Import Existing Resources (If Needed)

* [ ] Identify resources to import
* [ ] Match Terraform resource address correctly
* [ ] Import one resource at a time
* [ ] Run `plan` after each import

Example:

```bash
terraform import aws_security_group.bastion sg-012345
```

⚠️ Import does NOT create config — config must already exist.

---

## 6️⃣ Validate Resource Parity

After import or recreation:

* [ ] Resource names match
* [ ] Tags match
* [ ] Networking matches
* [ ] IAM permissions match
* [ ] No unintended drift

Use:

```bash
terraform plan
```

Expected result:
👉 **No changes**

---

## 7️⃣ Migration from `count` → `for_each`

* [ ] Identify index-based resources
* [ ] Replace `count` with `for_each`
* [ ] Use stable keys
* [ ] Expect resource address changes
* [ ] Plan carefully to avoid destroy/create

This is a **high-risk migration** — review carefully.

---

## 8️⃣ State Refactoring (Advanced)

If moving resources between modules or renaming:

* [ ] Use `terraform state mv`
* [ ] Move one resource at a time
* [ ] Verify state after each move

Example:

```bash
terraform state mv aws_sg.old aws_security_group.new
```

---

## 9️⃣ Environment Migration (dev → prod)

* [ ] Separate backend for prod
* [ ] Separate credentials
* [ ] Separate tfvars
* [ ] Validate prod-only restrictions
* [ ] Never reuse dev state for prod

---

## 🔟 Validation & Safety Checks

* [ ] Input validation added
* [ ] Unsafe defaults removed
* [ ] Public access restricted
* [ ] Destructive actions reviewed

Add validations to prevent mistakes:

```hcl
validation {
  condition     = var.env != "prod" || var.enable_public_access == false
  error_message = "Public access not allowed in prod"
}
```

---

## 1️⃣1️⃣ Dry Run & Plan Review

* [ ] Run `terraform plan`
* [ ] Review destroy actions carefully
* [ ] Ensure no accidental replacements
* [ ] Share plan output for review

🚨 **Never auto-apply during migration**

---

## 1️⃣2️⃣ Apply Strategy

* [ ] Apply in small steps
* [ ] Apply during low-traffic window
* [ ] Monitor resources during apply
* [ ] Rollback plan ready

---

## 1️⃣3️⃣ Post-Migration Verification

* [ ] Applications healthy
* [ ] Networking works
* [ ] Logs and monitoring active
* [ ] No drift after apply
* [ ] Team notified of completion

---

## 1️⃣4️⃣ Cleanup

* [ ] Remove unused resources
* [ ] Remove old scripts / manual configs
* [ ] Remove temporary debug outputs
* [ ] Update documentation

---

## ⭐ High-Risk Migration Areas (Extra Care)

🚨 Security Groups
🚨 IAM roles & policies
🚨 Databases (RDS)
🚨 Load balancers
🚨 EKS clusters
🚨 State moves

---

## 🧠 Golden Rule

> **Terraform migration is about state, not code.**
> If state is wrong, everything is wrong.

---

## 🔥 Interview-Ready Line

> “When migrating to Terraform, I focus on state safety first, import resources carefully, validate parity, and apply changes incrementally with plan reviews.”


