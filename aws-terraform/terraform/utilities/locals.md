## locals-patterns.md

---

## What are `locals` in Terraform?

`locals` are used to:

* Transform input data
* Avoid duplication
* Improve readability
* Centralize logic

👉 Locals **do not create resources**.
👉 They only compute values.

Think of locals as **Terraform variables with logic**.

---

## 1️⃣ Simple Constant Values

Used to avoid repeating literals.

```hcl
locals {
  project = "moneylag"
  owner   = "devops-team"
}
```

Use when:

* Same value used in many places
* Avoid copy-paste

---

## 2️⃣ Naming / Label Pattern (VERY COMMON)

```hcl
locals {
  name_prefix = "${var.environment}-${var.project}"
}
```

Use when:

* Resource naming
* Tags
* Outputs

---

## 3️⃣ Tags Pattern (MOST USED)

```hcl
locals {
  common_tags = merge(
    var.common_tags,
    {
      Environment = var.environment
      Project     = var.project
    }
  )
}
```

Use when:

* Centralized tagging
* Enforcing standards

---

## 4️⃣ Name → ID Mapping Pattern (CRITICAL)

This is **exactly what you used**.

```hcl
locals {
  sg_map = {
    bastion = module.bastion.sg_id
    rds     = module.rds.sg_id
  }
}
```

Use when:

* Inputs use logical names
* AWS needs IDs

This decouples **configuration intent** from **AWS implementation**.

---

## 5️⃣ Data Transformation Pattern (`for` expression)

```hcl
locals {
  transformed = {
    for k, v in var.input :
    k => upper(v)
  }
}
```

Use when:

* Reshaping data
* Making AWS-ready inputs

---

## 6️⃣ Optional Attribute Handling Pattern

```hcl
locals {
  cidr = lookup(var.rule, "cidr_blocks", null)
}
```

Use when:

* Optional inputs
* Avoid crashes

---

## 7️⃣ Defensive Locals Pattern (NULL-SAFE)

```hcl
locals {
  source_sg_id = (
    contains(keys(var.rule), "source_sg") &&
    var.rule.source_sg != null
  ) ? local.sg_map[var.rule.source_sg] : null
}
```

Use when:

* Indexing maps
* Prevent `Invalid index` errors

---

## 8️⃣ Environment Switch Pattern

```hcl
locals {
  replica_count = var.environment == "prod" ? 3 : 1
}
```

Use when:

* Dev vs prod behavior
* Cost optimization

---

## 9️⃣ Filtering Pattern

```hcl
locals {
  public_subnets = [
    for s in var.subnets : s if s.public
  ]
}
```

Use when:

* Selecting subsets
* Conditional logic

---

## 🔟 Flattening Pattern

```hcl
locals {
  all_subnets = flatten([
    var.public_subnets,
    var.private_subnets
  ])
}
```

Use when:

* Nested lists
* Combined inputs

---

## 1️⃣1️⃣ Complex Transformation Pattern (REAL WORLD)

This is your **SG rules case**.

```hcl
locals {
  resolved_sg_rules = {
    for name, rule in var.sg_rules :
    name => {
      type              = rule.type
      from_port         = rule.from_port
      to_port           = rule.to_port
      protocol          = rule.protocol
      security_group_id = local.sg_map[rule.security_group_name]
    }
  }
}
```

Use when:

* Human-friendly input
* AWS-ready output

---

## 1️⃣2️⃣ Locals for Readability (Underrated)

```hcl
locals {
  is_prod = var.environment == "prod"
}
```

Use when:

* Repeated conditions
* Cleaner expressions

---

## ⭐ MOST IMPORTANT LOCALS PATTERNS (MEMORIZE)

1. Name → ID mapping
2. Tag merging
3. Defensive null-safe lookup
4. Data transformation with `for`
5. Environment-based switches

---

## 🧠 Rule of Thumb

> If logic repeats more than once → move it to locals.

---

## 🔥 Interview-Ready Line

> “I use locals to centralize transformations and mappings, keeping root modules clean and avoiding duplication.”

---

## 🚫 What NOT to Do with Locals

❌ Don’t put resource creation
❌ Don’t hide complex business logic
❌ Don’t over-nest locals

Keep locals **simple and readable**.

