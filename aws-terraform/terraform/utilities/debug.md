## debugging.md

---

## How to Debug Terraform (Real-World Guide)

Terraform debugging is about **finding wrong data early**, not guessing.

The goal:

> Identify **where** the value breaks:
> **tfvars → variables → locals → resources**

---

## 1️⃣ Read the Error Message Carefully (FIRST RULE)

Terraform errors usually tell you:

* **File name**
* **Line number**
* **Exact expression that failed**

Example:

```text
Invalid index
local.sg_map[rule.source_security_group_name]
```

This immediately means:

* Key is missing OR
* Value is null OR
* Map does not contain that key

---

## 2️⃣ Use `terraform validate` First

```bash
terraform validate
```

Catches:

* Syntax errors
* Type mismatches
* Missing required variables

Always run before `plan`.

---

## 3️⃣ Use `terraform console` (MOST POWERFUL TOOL)

When confused → **open console immediately**.

```bash
terraform console
```

### Inspect inputs

```hcl
var.sg_rules
```

### Inspect locals

```hcl
local.sg_map
local.resolved_sg_rules
```

### Test expressions

```hcl
local.sg_map["external_alb"]
```

If it fails here, it WILL fail in plan.

---

## 4️⃣ Debug “Invalid index” Errors (VERY COMMON)

### Step-by-step

1. Check keys:

```hcl
keys(local.sg_map)
```

2. Check value:

```hcl
var.sg_rules["http_alb"]
```

3. Test lookup:

```hcl
contains(keys(local.sg_map), "external_alb")
```

This avoids blind fixes.

---

## 5️⃣ Use Defensive Functions (Prevent Errors)

### Bad (unsafe)

```hcl
rule.cidr_blocks
```

### Good (safe)

```hcl
lookup(rule, "cidr_blocks", null)
```

### Bad (unsafe)

```hcl
local.sg_map[rule.source_sg]
```

### Good (safe)

```hcl
contains(keys(rule), "source_sg") && rule.source_sg != null
? local.sg_map[rule.source_sg]
: null
```

---

## 6️⃣ Print Data Using Outputs (Temporary Debugging)

```hcl
output "debug_sg_rules" {
  value = local.resolved_sg_rules
}
```

Use when:

* Console is not enough
* Complex locals involved

Remove after debugging.

---

## 7️⃣ Reduce Scope (Isolate the Problem)

Comment out:

* Modules
* Resources
* Locals

Then re-enable step by step.

This narrows the failure point.

---

## 8️⃣ Check State vs Config Mismatch

Sometimes state is the issue.

```bash
terraform state list
terraform state show aws_security_group.bastion
```

Used when:

* Resource exists but plan fails
* Drift suspected

---

## 9️⃣ Enable Terraform Logs (Rare but Useful)

```bash
export TF_LOG=TRACE
terraform plan
```

Use only when:

* Provider behavior unclear
* API issues suspected

Disable after use.

---

## 🔟 Common Terraform Errors & Meaning

| Error                            | Meaning                |
| -------------------------------- | ---------------------- |
| Invalid index                    | Missing key or null    |
| Unsupported attribute            | Wrong object type      |
| Invalid value                    | Wrong input            |
| Cycle detected                   | Circular dependency    |
| Reference to undeclared resource | Typo or missing module |

---

## 🧠 Debugging Workflow (Memorize This)

1. Read error
2. Open `terraform console`
3. Inspect `var.*`
4. Inspect `local.*`
5. Test expression
6. Fix input or logic
7. Run `plan` again

---

## 🔥 Golden Rule

> If Terraform crashes at plan time,
> **the data is wrong — not Terraform**.

---

## Interview-Ready Line

> “I debug Terraform by inspecting variables and locals in terraform console and validating expressions before apply.”

That’s a **strong signal**.
