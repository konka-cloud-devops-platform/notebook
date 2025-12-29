## terraform-review-checklist.md

---

## Terraform Code Review Checklist

Use this checklist **before merging** Terraform code or **while reviewing PRs**.

---

## 1️⃣ Repository Structure

* [ ] Clear separation between `modules/`, `live/`, and `environments/`
* [ ] No resource creation inside `live/`
* [ ] No environment-specific values inside modules
* [ ] `environments/` contains only `tfvars` and backend configs

---

## 2️⃣ State & Backend

* [ ] Remote backend configured (S3 / Terraform Cloud)
* [ ] State locking enabled (DynamoDB / remote locking)
* [ ] Backend config not hardcoded in code
* [ ] Backend values passed via environment-specific tfvars

---

## 3️⃣ Providers & Versions

* [ ] `required_providers` block present
* [ ] Provider versions are pinned
* [ ] Terraform version constraint defined
* [ ] Single provider configuration per root module

---

## 4️⃣ Variables & Inputs

* [ ] All inputs defined in `variables.tf`
* [ ] No magic values hardcoded
* [ ] Sensitive values marked `sensitive = true`
* [ ] Default values only where appropriate
* [ ] Variable names are meaningful and consistent

---

## 5️⃣ Validation

* [ ] Critical variables have validation blocks
* [ ] Environment values are validated (`dev`, `prod`, etc.)
* [ ] Lists/maps validated for non-empty where required
* [ ] Cross-variable validations used for safety rules

---

## 6️⃣ Locals Usage

* [ ] Locals used only for transformation or readability
* [ ] No resource creation logic inside locals
* [ ] Defensive patterns used (`lookup`, `contains`, `try`)
* [ ] Name → ID mappings handled via locals (not hardcoded)

---

## 7️⃣ Loops & Resource Creation

* [ ] `for_each` preferred over `count`
* [ ] Resource keys are stable and meaningful
* [ ] No mixing of `count` and `for_each`
* [ ] Changing input order will not recreate resources

---

## 8️⃣ Defensive Terraform Coding

* [ ] Optional attributes accessed safely
* [ ] No direct indexing on uncertain keys
* [ ] `null` used to disable optional arguments
* [ ] No `Invalid index` risks

---

## 9️⃣ Security Best Practices

* [ ] No `0.0.0.0/0` in prod unless explicitly required
* [ ] Least privilege IAM policies
* [ ] Public access explicitly controlled
* [ ] Sensitive outputs avoided or marked sensitive

---

## 🔟 Outputs

* [ ] Outputs are minimal and useful
* [ ] No full resource objects exposed
* [ ] Output names are clear and consistent
* [ ] Sensitive outputs marked correctly

---

## 1️⃣1️⃣ Naming & Tagging

* [ ] Resource names follow a standard pattern
* [ ] Tags applied consistently
* [ ] Mandatory tags present (Env, Project, Owner, Cost)
* [ ] Tag logic centralized via locals

---

## 1️⃣2️⃣ Plan & Apply Safety

* [ ] `terraform plan` reviewed before apply
* [ ] No unexpected destroy actions
* [ ] Drift checked if changes look suspicious
* [ ] Large changes documented in PR description

---

## 1️⃣3️⃣ Readability & Maintainability

* [ ] Files logically separated (`vpc.tf`, `sg.tf`, etc.)
* [ ] Code is easy to scan and understand
* [ ] Complex logic documented with comments
* [ ] No unnecessary abstraction

---

## 1️⃣4️⃣ Debugging & Observability

* [ ] Code is easy to inspect via `terraform console`
* [ ] Locals and variables can be debugged easily
* [ ] Temporary debug outputs removed before merge

---

## 1️⃣5️⃣ Git & Collaboration

* [ ] `.terraform/` not committed
* [ ] State files not committed
* [ ] Meaningful commit messages
* [ ] PR includes context for reviewers

---

## ⭐ Final Sanity Check (Before Merge)

Ask yourself:

* [ ] Can a new engineer understand this in 10 minutes?
* [ ] Will this scale to prod safely?
* [ ] Will future changes cause accidental recreation?
* [ ] Is failure behavior predictable?

If **YES** → ✅ merge
If **NO** → ❌ revise

---

## 🔥 Interview-Ready Line

> “I review Terraform code using a checklist that covers structure, state safety, defensive coding, validation, and change impact before merge.”

