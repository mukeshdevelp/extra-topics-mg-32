# Terraform – Complete Learning Guide

## Table of Contents

1. [Terraform Variable Precedence](#1-terraform-variable-precedence)
2. [Cost Calculation for Terraform Resources](#2-cost-calculation-for-terraform-resources)
3. [Data Block in Terraform](#3-data-block-in-terraform)
4. [Data Types for Variables](#4-data-types-for-variables)
5. [depends_on](#5-depends_on)
6. [Different Commands in Terraform and Their Use](#6-different-commands-in-terraform-and-their-use)
7. [DynamoDB State Locking in Terraform](#7-dynamodb-state-locking-in-terraform)
8. [Dynamic Blocks](#8-dynamic-blocks)
9. [Error Handling in Terraform](#9-error-handling-in-terraform)
10. [Local Values](#10-local-values)
11. [Loops in Terraform – count & for_each](#11-loops-in-terraform--count--for_each)
12. [Managing Secrets and Sensitive Data](#12-managing-secrets-and-sensitive-data)
13. [Null Resource](#13-null-resource)
14. [Output Values](#14-output-values)
15. [Provisioner vs Provider](#15-provisioner-vs-provider)
16. [Remote State](#16-remote-state)
17. [Terraform Workspace](#17-terraform-workspace)
18. [Terraform Best Practices & Security](#18-terraform-best-practices--security)
19. [Terraform Dependencies](#19-terraform-dependencies)
20. [Terraform Desired & Current State](#20-terraform-desired--current-state)
21. [Terraform Import Command](#21-terraform-import-command)
22. [Terraform Lifecycle](#22-terraform-lifecycle)
23. [Terraform Module with Different Sources](#23-terraform-module-with-different-sources)
24. [Terraform Providers](#24-terraform-providers)
25. [Terraform Provisioner](#25-terraform-provisioner)
26. [Terraform Refresh Command](#26-terraform-refresh-command)
27. [Terraform Registry](#27-terraform-registry)
28. [Terraform Remote Execution](#28-terraform-remote-execution)
29. [Terraform Resource Tagging](#29-terraform-resource-tagging)
30. [Terraform Static vs Module](#30-terraform-static-vs-module)
31. [Terraform Taint & Untaint Command](#31-terraform-taint--untaint-command)
32. [Terraform tfvars](#32-terraform-tfvars)
33. [Terragrunt](#33-terragrunt)

---

# 1. Terraform Variable Precedence

[⬆ Back to Table of Contents](#table-of-contents)

## What is Variable Precedence?

Terraform decides which variable value to use when multiple values exist.

Priority (Highest → Lowest):

```text
CLI -var
↓
terraform.tfvars
↓
terraform.tfvars.json
↓
Environment Variables
↓
default value
```

Example:

```hcl
variable "instance_type" {
 default="t2.micro"
}
```

Run:

```bash
terraform apply -var="instance_type=t3.medium"
```

Use Cases:

* Environment overrides
* CI/CD pipelines

Layman:
Think of multiple people giving instructions; Terraform follows the highest priority instruction.

---

# 2. Cost Calculation for Terraform Resources

[⬆ Back to Table of Contents](#table-of-contents)

## Technical

Terraform itself does not calculate cloud cost.

Approaches:

* Resource estimation
* Cost analysis tools
* Plan analysis

Example:

```bash
terraform plan
```

Factors:

* Instance type
* Storage
* Networking
* Data transfer

Layman:
Before constructing a building, estimate cost from blueprint.

---

# 3. Data Block in Terraform

## Purpose

Read existing infrastructure.

Example:

```hcl
data "aws_vpc" "existing" {
 default=true
}
```

Use Cases:

* Reference existing resources
* Avoid recreation

Difference:

```text
resource → create
data → read
```

---

# 4. Data Types for Variables

Supported:

```hcl
string
number
bool
list
map
object
tuple
set
```

Example:

```hcl
variable "ports" {
 type=list(number)
}
```

---

# 5. depends_on

Explicit dependency.

Example:

```hcl
resource "aws_instance" "app" {
 depends_on=[
   aws_db_instance.db
 ]
}
```

Use Case:

* Resource ordering

---

# 6. Different Commands in Terraform and Their Use

Initialize:

```bash
terraform init
```

Plan:

```bash
terraform plan
```

Apply:

```bash
terraform apply
```

Destroy:

```bash
terraform destroy
```

Validate:

```bash
terraform validate
```

Format:

```bash
terraform fmt
```

State:

```bash
terraform state list
```

---

# 7. DynamoDB State Locking in Terraform

Purpose:
Prevent concurrent execution.

Architecture:

```text
Terraform
↓
S3
↓
DynamoDB Lock
```

Example:

```hcl
backend "s3" {
 bucket="tf-state"

 dynamodb_table="tf-lock"
}
```

---

# 8. Dynamic Blocks

Generate nested blocks dynamically.

Example:

```hcl
dynamic "ingress" {
 for_each=var.rules
}
```

Use:

* Reusable configs

---

# 9. Error Handling in Terraform

Functions:

```hcl
try()
can()
```

Example:

```hcl
locals{
 value=try(var.x,"default")
}
```

---

# 10. Local Values

Reusable expressions.

Example:

```hcl
locals {
 env="dev"
}
```

Access:

```hcl
local.env
```

---

# 11. Loops in Terraform – count & for_each

count:

```hcl
count=3
```

for_each:

```hcl
for_each=var.users
```

Difference:

| count  | for_each   |
| ------ | ---------- |
| Number | Collection |

---

# 12. Managing Secrets and Sensitive Data

Techniques:

* sensitive=true
* Vault
* Secrets Manager

Example:

```hcl
variable "password"{
 sensitive=true
}
```

---

# 13. Null Resource

Execute actions.

Example:

```hcl
resource "null_resource" "deploy" {}
```

---

# 14. Output Values

Expose values.

```hcl
output "ip" {
 value=aws_instance.app.public_ip
}
```

---

# 15. Provisioner vs Provider

| Provider     | Provisioner     |
| ------------ | --------------- |
| Create infra | Configure infra |

---

# 16. Remote State

Store state remotely.

Example:

```hcl
backend "s3" {}
```

Benefits:

* Collaboration
* Locking

---

# 17. Terraform Workspace

Multiple environments.

Commands:

```bash
terraform workspace new dev
terraform workspace select prod
```

---

# 18. Terraform Best Practices & Security

* Use modules
* Enable versioning
* Lock state
* Scan IaC
* Least privilege

---

# 19. Terraform Dependencies

Implicit:

```hcl
vpc_id=aws_vpc.main.id
```

Explicit:

```hcl
depends_on
```

---

# 20. Terraform Desired & Current State

Current:
Actual infra.

Desired:
Terraform code.

Apply → sync both.

---

# 21. Terraform Import Command

Import existing infra.

```bash
terraform import
```

---

# 22. Terraform Lifecycle

Options:

```hcl
create_before_destroy
ignore_changes
prevent_destroy
```

---

# 23. Terraform Module with Different Sources

Sources:

```text
Local
Git
Registry
S3
```

Example:

```hcl
source="./modules"
```

---

# 24. Terraform Providers

Examples:

```hcl
provider "aws" {}
provider "google" {}
```

---

# 25. Terraform Provisioner

Types:

```text
local-exec
remote-exec
```

---

# 26. Terraform Refresh Command

Sync state.

```bash
terraform refresh
```

---

# 27. Terraform Registry

Repository for modules/providers.

Examples:

* AWS modules
* Azure modules

---

# 28. Terraform Remote Execution

Execute remotely.

Examples:

* Terraform Cloud
* CI/CD

---

# 29. Terraform Resource Tagging

Example:

```hcl
tags={
 Environment="dev"
}
```

---

# 30. Terraform Static vs Module

Static:
Repeated code

Module:
Reusable

---

# 31. Terraform Taint & Untaint Command

Mark recreate:

```bash
terraform taint
```

Undo:

```bash
terraform untaint
```

---

# 32. Terraform tfvars

Store variables.

Example:

```hcl
region="us-east-1"
```

Run:

```bash
terraform apply -var-file=prod.tfvars
```

---

# 33. Terragrunt

Wrapper over Terraform.

Features:

* DRY
* Remote state
* Environment management

Example:

```bash
terragrunt apply
```

---

