# VPC

**VPC (Virtual Private Cloud)**
We can create a **virtually isolated private network** in **Amazon Web Services** to launch cloud resources.

VPC is a **logically isolated private network** where we control:

* IP address ranges
* Subnets
* Routing
* Network security
  to securely deploy cloud resources.

---

## CIDR Block

CIDR defines the **IP address range** of the VPC.
All resources inside the VPC get IPs from this range.

Example:
`10.0.0.0/16` → around **65,000 IP addresses**

---

## Subnets

Subnets divide a VPC into **smaller networks**.

Example:
`10.0.1.0/24`, `10.0.2.0/24`, etc.

### Public Subnet

* Exposed to the internet
* Uses Internet Gateway
* Used for:

  * Load Balancers
  * NAT Gateway
  * Bastion host

### Private Subnet

* Accessible only inside the VPC
* No direct internet access
* Used for:

  * Application servers
  * Databases
  * Caches

---

## Internet Gateway (IGW)

* Allows **public subnet resources** to communicate with the internet
* Without IGW, resources cannot access the internet

---

## NAT Gateway (NatGW)

* Allows **private subnet resources** to access the internet **outbound only**
* No inbound access from the internet

Example:
EC2 in private subnet can download updates but **cannot be accessed from the internet**

---

## Route Tables

* Define **where network traffic goes**
* Contains routing rules

Example:

* `0.0.0.0/0 → IGW` (public subnet)
* `0.0.0.0/0 → NAT Gateway` (private subnet)

### Subnet Association

* Each subnet must be associated with a route table

---

## Security Group (SG)

* Instance-level **stateful firewall**
* Controls inbound and outbound traffic
* Only **allow rules**
* No deny rules

Example:

* Allow port 80 from ALB
* Allow port 22 from bastion host

---

## Network ACL (NACL)

* Subnet-level **stateless firewall**
* Supports **allow and deny rules**
* Inbound and outbound rules must be defined separately

Example:

* Allow port 22 in inbound
* Allow port 22 in outbound

Mostly used for **extra security layer**

---

## VPC Endpoints

* Provide **private access to AWS services**
* Traffic does not go through internet
* No NAT Gateway required

Examples:

* S3
* DynamoDB

---

## VPC Peering

* Connects **two VPCs** privately
* CIDR ranges must **not overlap**
* One-to-one connection only

Example:

* A ↔ B (allowed)
* A ↔ C (not allowed unless peered separately)

---

## VPC Flow Logs

* Capture **network traffic metadata**
* Used for:

  * Troubleshooting
  * Security analysis
  * Monitoring traffic flow

---

## VPC Transit Gateway

* Works on **hub-and-spoke architecture**
* Central gateway to connect:

  * Multiple VPCs
  * On-premise networks
  * Other cloud networks

---
