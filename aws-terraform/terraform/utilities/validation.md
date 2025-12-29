## validation.md

---

## What is Validation in Terraform?

Validation is used to **fail early** with a clear error message
when input values are **wrong, missing, or unsafe**.

👉 Validation happens at **plan time**, not apply time.

---

## 1️⃣ Variable Validation (MOST COMMON)

Used inside `variable` blocks.

### Basic example

```hcl
variable "environment" {
  type = string

  validation {
    condition     = contains(["dev", "staging", "prod"], var.environment)
    error_message = "environment must be dev, staging, or prod"
  }
}
```

Use when:

* Fixed allowed values
* Environment checks

---

## 2️⃣ Validate Non-Empty Values

```hcl
variable "subnet_ids" {
  type = list(string)

  validation {
    condition     = length(var.subnet_ids) > 0
    error_message = "subnet_ids cannot be empty"
  }
}
```

Very common for:

* Subnets
* Security groups
* CIDR blocks

---

## 3️⃣ Validate Map Contains Required Keys

```hcl
variable "sg_rules" {
  type = map(any)

  validation {
    condition     = contains(keys(var.sg_rules), "bastion_ssh")
    error_message = "sg_rules must contain bastion_ssh rule"
  }
}
```

Use when:

* Certain keys are mandatory
* Prevent runtime surprises

---

## 4️⃣ Validate String Pattern (Regex)

```hcl
variable "cidr_block" {
  type = string

  validation {
    condition     = can(cidrnetmask(var.cidr_block))
    error_message = "Invalid CIDR block"
  }
}
```

Used for:

* CIDR
* ARNs
* Naming rules

---

## 5️⃣ Validate Optional Variables (Null-safe)

```hcl
variable "instance_type" {
  type    = string
  default = null

  validation {
    condition     = var.instance_type == null || length(var.instance_type) > 0
    error_message = "instance_type cannot be empty string"
  }
}
```

Use when:

* Variable is optional
* But must be valid if provided

---

## 6️⃣ Validate Boolean Combinations

```hcl
variable "enable_public_access" {
  type = bool
}

variable "environment" {
  type = string
}

validation {
  condition     = !(var.environment == "prod" && var.enable_public_access)
  error_message = "Public access is not allowed in prod"
}
```

Used for:

* Security enforcement
* Environment policies

---

## 7️⃣ Validate Maps with Required Attributes (Advanced)

```hcl
variable "sg_rules" {
  type = map(object({
    type                = string
    from_port           = number
    to_port             = number
    protocol            = string
    security_group_name = string
  }))

  validation {
    condition = alltrue([
      for _, rule in var.sg_rules :
      rule.from_port <= rule.to_port
    ])
    error_message = "from_port must be less than or equal to to_port"
  }
}
```

Very powerful for:

* SG rules
* IAM policies
* Complex objects

---

## 8️⃣ Validate Allowed Protocols

```hcl
validation {
  condition     = contains(["tcp", "udp", "-1"], rule.protocol)
  error_message = "protocol must be tcp, udp, or -1"
}
```

Used in:

* Security groups
* Network rules

---

## 9️⃣ Using `can()` in Validation

```hcl
validation {
  condition     = can(regex("^sg-", var.sg_id))
  error_message = "Invalid security group ID"
}
```

Use when:

* Expression may fail
* Avoid crashes in validation itself

---

## 🔟 Cross-Variable Validation (Common Pattern)

```hcl
validation {
  condition     = !(var.env == "prod" && var.instance_count < 2)
  error_message = "Prod must have at least 2 instances"
}
```

Used for:

* HA enforcement
* Cost rules
* Policy checks

---

## ⭐ MOST USED VALIDATION PATTERNS (MEMORIZE)

### Required value

```hcl
length(var.value) > 0
```

### Allowed values

```hcl
contains(["a","b","c"], var.value)
```

### Safe optional value

```hcl
var.value == null || length(var.value) > 0
```

### Map key exists

```hcl
contains(keys(var.map), "key")
```

### Prevent unsafe config

```hcl
!(condition_that_should_not_happen)
```

---

## 🧠 Rule of Thumb

> If a wrong input can **break infra**,
> **validate it** instead of debugging later.

---

## 🔥 Interview-Ready Line

> “I use Terraform validations to fail fast at plan time, preventing invalid or unsafe configurations from reaching apply.”

