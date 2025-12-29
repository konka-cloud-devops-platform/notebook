Perfect 👍
Below is **clean, focused content ONLY on Terraform functions** — **no operators**, **no conditionals**, **no loops**.
This is exactly what you should keep in `functions.md`.

You can **copy–paste this directly**.

---

## functions.md

### 1️⃣ `length()`

Returns number of elements in a list, map, or characters in a string.

```hcl
length(value)
```

Examples:

```hcl
length(var.subnets)
length(var.tags)
length("terraform")
```

Used for:

* Checking empty lists
* Validations
* Conditional resource creation

---

### 2️⃣ `lookup()`

Safely read a value from a map.

```hcl
lookup(map, key, default)
```

Example:

```hcl
lookup(rule, "cidr_blocks", null)
```

Used for:

* Optional attributes
* Avoiding `Invalid index` errors

---

### 3️⃣ `try()`

Returns first expression that does not error.

```hcl
try(expr1, expr2, ...)
```

Example:

```hcl
try(var.instance_type, "t3.micro")
```

Used for:

* Fallback values
* Handling missing attributes

---

### 4️⃣ `merge()`

Merges multiple maps into one.

```hcl
merge(map1, map2, ...)
```

Example:

```hcl
merge(
  var.common_tags,
  { Name = "eks-cluster" }
)
```

Used for:

* Tags
* Combining defaults with overrides

---

### 5️⃣ `contains()`

Checks if a list contains a value.

```hcl
contains(list, value)
```

Example:

```hcl
contains(keys(rule), "cidr_blocks")
```

Used for:

* Checking key existence (with `keys()`)

---

### 6️⃣ `keys()`

Returns all keys of a map.

```hcl
keys(map)
```

Example:

```hcl
keys(var.sg_rules)
```

Used for:

* Key validation
* Safe lookups

---

### 7️⃣ `values()`

Returns all values of a map.

```hcl
values(map)
```

Example:

```hcl
values(var.tags)
```

Used for:

* Iteration
* Data inspection

---

### 8️⃣ `flatten()`

Flattens a list of lists into a single list.

```hcl
flatten(list)
```

Example:

```hcl
flatten([var.public_subnets, var.private_subnets])
```

Used for:

* Subnet lists
* Security group rules
* Combined inputs

---

### 9️⃣ `distinct()`

Removes duplicate values from a list.

```hcl
distinct(list)
```

Example:

```hcl
distinct(var.cidr_blocks)
```

Used for:

* Cleanup lists
* Avoid duplicate rules

---

### 🔟 `sort()`

Sorts a list in ascending order.

```hcl
sort(list)
```

Example:

```hcl
sort(var.availability_zones)
```

Used for:

* Stable plans
* Consistent ordering

---

### 1️⃣1️⃣ `join()`

Joins list elements into a string.

```hcl
join(separator, list)
```

Example:

```hcl
join(",", var.subnet_ids)
```

Used for:

* Names
* Outputs
* Logging

---

### 1️⃣2️⃣ `split()`

Splits a string into a list.

```hcl
split(separator, string)
```

Example:

```hcl
split(",", "dev,qa,prod")
```

Used for:

* Parsing inputs
* Environment lists

---

### 1️⃣3️⃣ `can()`

Checks if an expression can be evaluated safely.

```hcl
can(expr)
```

Example:

```hcl
can(rule.source_security_group_name)
```

Used for:

* Defensive coding
* Avoiding runtime errors

---

### 1️⃣4️⃣ `jsonencode()`

Converts Terraform object to JSON string.

```hcl
jsonencode(value)
```

Example:

```hcl
jsonencode({
  Version = "2012-10-17"
})
```

Used for:

* IAM policies
* ECS task definitions

---

### 1️⃣5️⃣ `jsondecode()`

Converts JSON string to Terraform object.

```hcl
jsondecode(string)
```

Example:

```hcl
jsondecode(file("policy.json"))
```

Used for:

* External JSON files
* Policy reuse

---

## ⭐ MOST USED IN DAY-TO-DAY (Top Priority)

1. `lookup`
2. `length`
3. `merge`
4. `try`
5. `contains`
6. `keys`
7. `flatten`
8. `distinct`
9. `can`
10. `jsonencode`

---

## 🧠 Rule of Thumb

> If input **might be missing or optional**,
> use `lookup`, `try`, or `can`.

---

If you want next:

* `operators.md`
* `loops.md`
* `validation.md`
* `terraform-console-practice.md`

Just tell me 👍

---
Perfect 👍
Your explanation is **conceptually correct**. I’ll **simplify it**, polish the language, and give you a **clean version you can remember and say easily**.

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