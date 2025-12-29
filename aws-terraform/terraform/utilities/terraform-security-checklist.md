## terraform-security-checklist.md

---

## Terraform Security Checklist

Use this checklist to ensure Terraform code follows **security best practices** before deploying to any environment (especially prod).

---

## 1️⃣ State File Security (CRITICAL)

* [ ] Remote backend enabled (S3 / Terraform Cloud)
* [ ] State encryption enabled at rest
* [ ] State bucket access restricted (IAM + bucket policy)
* [ ] DynamoDB state locking enabled (if using S3)
* [ ] State files NOT committed to Git
* [ ] `.terraform/` directory ignored in Git

👉 **State files may contain secrets** — protect them like credentials.

---

## 2️⃣ Secrets & Sensitive Data

* [ ] No hardcoded secrets in Terraform code
* [ ] No secrets in `tfvars` committed to Git
* [ ] Sensitive variables marked `sensitive = true`
* [ ] Secrets sourced from:

  * AWS Secrets Manager
  * SSM Parameter Store
  * CI/CD secret store

❌ Never store:

* Passwords
* Tokens
* Private keys
* DB credentials

---

## 3️⃣ IAM Security (Least Privilege)

* [ ] IAM policies grant minimum required permissions
* [ ] No wildcard actions (`*`) unless justified
* [ ] No wildcard resources (`*`) in prod
* [ ] Separate roles for different services
* [ ] IAM roles used instead of access keys where possible

---

## 4️⃣ Provider & Credentials Handling

* [ ] No AWS credentials hardcoded in provider block
* [ ] Credentials injected via:

  * Environment variables
  * IAM roles
  * CI/CD OIDC
* [ ] Separate credentials per environment
* [ ] Provider versions pinned

---

## 5️⃣ Network Security (VPC / SG / NACL)

* [ ] No `0.0.0.0/0` ingress in prod unless required
* [ ] SG rules scoped to:

  * CIDR ranges
  * Other security groups
* [ ] Egress rules restricted in prod where possible
* [ ] Public subnets explicitly defined
* [ ] Private resources not accidentally exposed

---

## 6️⃣ Security Group Rules

* [ ] SG rules defined using `for_each` (stable)
* [ ] No duplicate or overlapping rules
* [ ] CIDR-based rules reviewed carefully
* [ ] Source SG rules preferred over CIDR where possible
* [ ] Self-referencing rules used intentionally

---

## 7️⃣ Validation for Security Enforcement

* [ ] Validation blocks prevent unsafe configs
* [ ] Public access blocked in prod via validation
* [ ] Environment-specific rules enforced
* [ ] Required security inputs validated (CIDRs, ports)

Example:

```hcl
validation {
  condition     = !(var.environment == "prod" && var.allow_public_access)
  error_message = "Public access is not allowed in prod"
}
```

---

## 8️⃣ Outputs Security

* [ ] No sensitive values exposed in outputs
* [ ] Outputs marked `sensitive = true` if required
* [ ] No secrets in module outputs
* [ ] Outputs limited to what consumers actually need

---

## 9️⃣ Logging & Monitoring

* [ ] CloudTrail enabled
* [ ] VPC Flow Logs enabled where required
* [ ] Access logs enabled for:

  * ALB
  * CloudFront
  * S3 (if public or critical)
* [ ] Logs stored in restricted buckets

---

## 🔟 Resource Protection

* [ ] `prevent_destroy` used for critical resources
* [ ] Destructive changes reviewed carefully
* [ ] Lifecycle rules documented
* [ ] Accidental deletion safeguards in place

Example:

```hcl
lifecycle {
  prevent_destroy = true
}
```

---

## 1️⃣1️⃣ Environment Isolation

* [ ] Separate state per environment
* [ ] Separate backends per environment
* [ ] No shared prod/dev state
* [ ] Separate IAM roles per environment

---

## 1️⃣2️⃣ Module Security

* [ ] Modules do not expose sensitive outputs
* [ ] Modules accept only required inputs
* [ ] Secure defaults used in modules
* [ ] Modules documented clearly

---

## 1️⃣3️⃣ CI/CD Security

* [ ] Terraform runs in CI/CD (not local laptops for prod)
* [ ] Plan reviewed before apply
* [ ] Apply restricted to trusted roles
* [ ] OIDC used instead of long-lived credentials
* [ ] Secrets masked in CI logs

---

## 1️⃣4️⃣ Dependency & Version Security

* [ ] Terraform version pinned
* [ ] Provider versions pinned
* [ ] No unreviewed provider upgrades
* [ ] Breaking changes reviewed before upgrade

---

## ⭐ High-Risk Red Flags (STOP & REVIEW)

🚨 `0.0.0.0/0` on SSH/RDP
🚨 Wildcard IAM policies
🚨 Secrets in Git
🚨 Shared prod/dev state
🚨 Unreviewed destroy plans
🚨 Public S3 buckets without justification

---

## 🧠 Golden Rule

> If Terraform can **create it**, Terraform can **expose it**.
> Always assume misconfiguration is possible.

---

## 🔥 Interview-Ready Line

> “I follow a Terraform security checklist covering state protection, secrets handling, IAM least privilege, network exposure, and environment isolation.”


