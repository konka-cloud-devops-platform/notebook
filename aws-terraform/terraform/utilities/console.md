## terraform-console.md

---

## What is `terraform console`?

`terraform console` is an **interactive REPL** that lets you:

* Evaluate Terraform expressions
* Inspect variables, locals, outputs
* Test functions safely
* Debug complex logic **without running plan/apply**

👉 It works on the **current Terraform state and configuration**.

---

## How to Start Terraform Console

```bash
terraform console
```

Exit with:

```bash
exit
```

---

## 1️⃣ Inspect Variables

```hcl
var.environment
var.sg_rules
var.common_tags
```

Used for:

* Checking values from `tfvars`
* Debugging wrong inputs

---

## 2️⃣ Inspect Locals (VERY COMMON)

```hcl
local.sg_map
local.resolved_sg_rules
```

Best use case:

* Debugging name → ID mappings
* Checking transformed data

This would have helped you immediately during SG errors.

---

## 3️⃣ Test Functions (MOST IMPORTANT)

### `lookup`

```hcl
lookup(var.sg_rules["bastion_ssh"], "cidr_blocks", null)
```

### `length`

```hcl
length(var.subnets)
```

### `contains`

```hcl
contains(keys(var.sg_rules["bastion_ssh"]), "source_security_group_name")
```

### `try`

```hcl
try(var.not_existing, "default")
```

### `can`

```hcl
can(var.sg_rules["bastion_ssh"].cidr_blocks)
```

---

## 4️⃣ Test Conditionals

```hcl
var.env == "prod" ? "STRICT" : "OPEN"
```

```hcl
contains(keys(var.sg_rules["bastion_ssh"]), "source_security_group_name")
? "HAS SOURCE"
: "NO SOURCE"
```

---

## 5️⃣ Debug Invalid Index Errors (REAL CASE)

Instead of guessing, test:

```hcl
keys(var.sg_rules["http_alb"])
```

```hcl
contains(keys(var.sg_rules["http_alb"]), "source_security_group_name")
```

This tells you **why Terraform crashed**.

---

## 6️⃣ Test Maps and Indexing

```hcl
local.sg_map["bastion"]
```

```hcl
local.sg_map["external_alb"]
```

If it errors here → you know the problem instantly.

---

## 7️⃣ Explore Resource Attributes (State-Based)

```hcl
aws_security_group.bastion.id
aws_eks_cluster.this.endpoint
```

Only works **after apply**.

---

## 8️⃣ Inspect Outputs

```hcl
output.sg_ids
```

Useful for:

* Validating module outputs
* Debugging incorrect wiring

---

## 9️⃣ Test `for` Expressions

```hcl
{ for k, v in var.sg_rules : k => v.security_group_name }
```

```hcl
[ for rule in values(var.sg_rules) : rule.type ]
```

Great for understanding transformations.

---

## 🔟 Debugging Workflow (Recommended)

1. Run `terraform console`
2. Inspect `var.*`
3. Inspect `local.*`
4. Test one function at a time
5. Fix config
6. Run `terraform plan`

This saves **huge amounts of time**.

---

## ⭐ Most Common Console Commands (Memorize)

```hcl
var.<name>
local.<name>
lookup(...)
contains(...)
keys(...)
length(...)
try(...)
can(...)
```

---

## 🧠 Rule of Thumb

> If you’re guessing why Terraform failed —
> **open terraform console immediately**.

---

## 🔥 Interview-Ready Line

> “I frequently use terraform console to validate variables, locals, and expressions before running plan or apply.”

---
