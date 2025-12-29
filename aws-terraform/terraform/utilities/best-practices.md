## best-practices.md

---

## Terraform Best Practices (Production Ready)

These practices help you build **stable, readable, and safe** Terraform code that scales with teams and environments.

---

## 1️⃣ Separate Code by Responsibility

**Recommended structure**

```
modules/        # Reusable building blocks
live/           # Root module (only orchestration)
environments/   # env-specific values & backend config
```

Rules:

* `modules/` → create resources
* `live/` → call modules only
* `environments/` → values only (no logic)

---

## 2️⃣ Keep `live/` Thin (Very Important)

`live/` should:

* Call modules
* Wire outputs → inputs
* Contain minimal logic

Avoid:

* Hardcoding values
* Business logic
* Inline resources

---

## 3️⃣ Use Modules for Reusability

If something is used more than once → make it a module.

Examples:

* VPC
* Security Groups
* IAM
* EKS
* ALB

Benefits:

* DRY code
* Easier updates
* Clear ownership

---

## 4️⃣ Prefer `for_each` Over `count`

* `count` → index-based → fragile
* `for_each` → key-based → stable

Use `for_each` when:

* Resources are named
* Order should not matter
* Deletions should be safe

---

## 5️⃣ Never Hardcode Environment-Specific Values

❌ Bad

```hcl
cidr_blocks = ["0.0.0.0/0"]
```

✅ Good

```hcl
cidr_blocks = var.allowed_cidrs
```

Environment differences must live in:

```
environments/dev/dev.tfvars
environments/prod/prod.tfvars
```

---

## 6️⃣ Use `locals` for Transformation, Not Creation

Use locals to:

* Transform data
* Map names → IDs
* Merge tags
* Improve readability

Never use locals to:

* Create resources
* Hide complex logic

---

## 7️⃣ Always Write Defensive Terraform

Terraform fails **hard** on missing keys.

Use:

* `lookup()`
* `contains()`
* `try()`
* `can()`

Example:

```hcl
lookup(rule, "cidr_blocks", null)
```

This prevents `Invalid index` errors.

---

## 8️⃣ Validate Inputs Early

Use variable validation to fail fast.

```hcl
validation {
  condition     = contains(["dev","prod"], var.environment)
  error_message = "Invalid environment"
}
```

Benefits:

* Fewer runtime errors
* Safer applies
* Better UX

---

## 9️⃣ Use Remote Backend Always

Never store state locally for shared projects.

Recommended:

* S3 backend
* DynamoDB lock

Benefits:

* Team collaboration
* State safety
* Locking

---

## 🔟 One Provider Block Per Root Module

Keep provider configuration **centralized**.

```hcl
provider "aws" {
  region = var.aws_region
}
```

Avoid:

* Provider blocks inside modules (unless required)

---

## 1️⃣1️⃣ Output What Others Need (Nothing More)

Outputs should:

* Be useful
* Be minimal
* Expose IDs, ARNs, endpoints

Avoid:

* Outputting entire resources
* Sensitive data (unless required)

---

## 1️⃣2️⃣ Name Resources Predictably

Use consistent naming:

```hcl
${var.environment}-${var.project}-${var.component}
```

Helps with:

* Debugging
* Cost tracking
* Audits

---

## 1️⃣3️⃣ Use Tags Everywhere

Always apply tags via locals.

```hcl
tags = local.common_tags
```

Required tags usually include:

* Environment
* Project
* Owner
* CostCenter

---

## 1️⃣4️⃣ Use `terraform console` for Debugging

Before guessing:

```bash
terraform console
```

Inspect:

* `var.*`
* `local.*`

This saves hours.

---

## 1️⃣5️⃣ Keep State Clean

Avoid:

* Manual state edits
* `terraform import` without documentation

If needed:

* Backup state
* Understand resource address fully

---

## 1️⃣6️⃣ Lock Versions Explicitly

```hcl
terraform {
  required_version = "~> 1.6"

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}
```

Prevents:

* Breaking upgrades
* Unexpected behavior

---

## 1️⃣7️⃣ Avoid Over-Engineering

Don’t:

* Over-nest modules
* Add logic for hypothetical future
* Make configs unreadable

Prefer:

* Clear
* Simple
* Incremental improvements

---

## ⭐ Golden Rules (Memorize These)

* Values go in `tfvars`
* Logic goes in `locals`
* Resources go in `modules`
* Root module orchestrates only
* `for_each` > `count`
* Defensive coding always
* Validate early, debug early

---

## 🔥 Interview-Ready Line

> “I follow Terraform best practices by keeping root modules thin, using reusable modules, validating inputs early, and writing defensive, environment-driven configurations.”


