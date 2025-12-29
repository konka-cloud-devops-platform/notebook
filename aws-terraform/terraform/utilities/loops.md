Perfect 👍
Below is **clean, practical, day-to-day Terraform loops content** for **`loops.md`**.
Only things you’ll actually use in real projects.

You can **copy–paste this directly**.

---

## loops.md

---

## 1️⃣ `for_each` (MOST IMPORTANT LOOP)

Creates multiple resources or modules from a map or set.

```hcl
for_each = map_or_set
```

### Example (resources)

```hcl
resource "aws_security_group_rule" "this" {
  for_each = var.sg_rules

  type              = each.value.type
  from_port         = each.value.from_port
  to_port           = each.value.to_port
  protocol          = each.value.protocol
  security_group_id = each.value.security_group_id
}
```

### How it works

* `each.key`   → rule name (`bastion_ssh`)
* `each.value` → rule object

Used for:

* SG rules
* IAM policies
* Route tables
* Anything dynamic

---

## 2️⃣ `count` (simple loop)

Creates multiple copies using a number.

```hcl
count = number
```

### Example

```hcl
resource "aws_instance" "web" {
  count = 2
}
```

Access with:

```hcl
aws_instance.web[0].id
```

⚠️ Avoid when possible — `for_each` is safer.

---

## 3️⃣ `for` Expression (map/list transformation)

Transforms lists or maps into new structures.

### Map → Map

```hcl
{ for k, v in var.tags : k => upper(v) }
```

### List → List

```hcl
[ for subnet in var.subnets : subnet.id ]
```

Used heavily in:

* `locals`
* Data reshaping
* Name → ID translation

---

## 4️⃣ `for` Expression with Condition (filtering)

```hcl
[ for s in var.subnets : s if s.public ]
```

Keeps only matching items.

Used for:

* Public vs private subnets
* Enabled features
* Environment filtering

---

## 5️⃣ Nested `for` (advanced but common)

```hcl
flatten([
  for sg, ports in var.sg_ports : [
    for port in ports : {
      sg   = sg
      port = port
    }
  ]
])
```

Used for:

* SG rules
* Matrix-style configs

---

## 6️⃣ `dynamic` block (inside resources)

Used when a block itself must repeat.

```hcl
dynamic "ingress" {
  for_each = var.ingress_rules
  content {
    from_port = ingress.value.from
    to_port   = ingress.value.to
    protocol  = ingress.value.protocol
  }
}
```

Used for:

* Security group inline rules
* IAM statements

---

## 7️⃣ Looping Modules with `for_each`

```hcl
module "sg" {
  for_each = var.security_groups
  source   = "../modules/sg"

  sg_name = each.key
}
```

Creates **multiple module instances**.

---

## 8️⃣ `each.key` vs `each.value`

```hcl
for_each = {
  bastion = 22
  vpn     = 443
}
```

* `each.key`   → `"bastion"`
* `each.value` → `22`

Very important distinction.

---

## 9️⃣ Common Loop Mistakes (AVOID)

❌ Mixing `count` and `for_each`
❌ Using `count` with maps
❌ Changing keys in `for_each` (causes destroy/create)
❌ Indexing `count` when order matters

---

## ⭐ MOST USED LOOP PATTERNS (MEMORIZE)

### Map loop

```hcl
for_each = var.items
```

### Transform map

```hcl
{ for k, v in var.map : k => v }
```

### Transform list

```hcl
[ for v in var.list : v ]
```

### Filter list

```hcl
[ for v in var.list : v if v.enabled ]
```

---

## 🧠 Rule of Thumb

* Use **`for_each`** → when keys matter
* Use **`count`** → only for simple numeric repetition
* Use **`for` expressions** → in `locals`
* Use **`dynamic` blocks** → only when block itself must repeat

---

## 🔥 Real-World Example (SG rules)

```hcl
resolved_sg_rules = {
  for name, rule in var.sg_rules :
  name => rule
}
```

This pattern appears **everywhere** in real Terraform code.

---
## Simple Explanation: `count` vs `for_each`

### `count` (index-based)

* `count` creates resources using **index numbers**: `0, 1, 2`
* Terraform tracks resources by **index position**, not by meaning

Example:

```hcl
count = 3
```

Resources:

```
resource[0]
resource[1]
resource[2]
```

#### Problem with `count`

If the count or order changes:

* Index values shift
* Terraform **destroys existing resources**
* New resources are created with new indexes

So even unchanged resources may get recreated.

---

### `for_each` (key-based)

* `for_each` creates resources using **keys**
* Each key represents **one specific resource**

Example:

```hcl
for_each = {
  bastion = 22
  vpn     = 443
  alb     = 80
}
```

Resources:

```
resource["bastion"]
resource["vpn"]
resource["alb"]
```

#### Advantage of `for_each`

* Terraform tracks resources by **key**
* If you remove one key:

  * Only that resource is destroyed
  * Other resources remain untouched

No index shifting. No unnecessary recreation.

---

## One-Line Comparison (Easy to Remember)

> **`count` tracks resources by position,
> `for_each` tracks resources by identity.**

---

## When to Use What

| Use case                                 | Prefer     |
| ---------------------------------------- | ---------- |
| Fixed number of identical resources      | `count`    |
| Named resources (SG rules, subnets, IAM) | `for_each` |
| Dynamic infrastructure                   | `for_each` |

---

## Interview-Ready Line

> “`count` is index-based and can cause resource recreation when indexes change, while `for_each` is key-based and updates only the affected resources.”

---
