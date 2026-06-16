# AWS – Week 1 Team Sessions (Batch-34)

## Total Topics Covered: 5

## Table of Contents

1. [Instance Pricing Model & AWS CLI](#1-instance-pricing-model--aws-cli)
2. [Load Balancers & Types | ALB vs NLB | Routing Types](#2-load-balancers--types--alb-vs-nlb--routing-types)
3. [S3 (Types of S3 & Lifecycle Policies)](#3-s3-types-of-s3--lifecycle-policies)
4. [ASG & Policies](#4-asg--policies)
5. [Launch Template vs Launch Configuration](#5-launch-template-vs-launch-configuration)

---

# 1. Instance Pricing Model & AWS CLI

## Technical Explanation

AWS provides multiple EC2 pricing options depending on workload patterns.

### On-Demand

* Pay per usage
* No commitment
* Flexible

Use Cases:

* Testing
* Short-term projects

---

### Reserved Instances

* Commit for 1–3 years
* Lower cost

Use Cases:

* Stable production

---

### Spot Instances

* Use unused AWS capacity
* Cheapest option
* Can terminate anytime

Use Cases:

* Batch jobs
* CI/CD

---

### Savings Plan

Flexible discount model.

---

## Pricing Comparison

| Type      | Cost   | Reliability |
| --------- | ------ | ----------- |
| On-Demand | High   | High        |
| Reserved  | Medium | High        |
| Spot      | Low    | Low         |
| Savings   | Medium | High        |

---

## AWS CLI

### What is AWS CLI?

Command-line tool to manage AWS.

Install:

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o awscliv2.zip
```

Configure:

```bash
aws configure
```

Verify:

```bash
aws sts get-caller-identity
```

Create EC2:

```bash
aws ec2 run-instances
```

---

## Layman Explanation

AWS pricing is like:

Taxi → On-demand
Monthly Pass → Reserved
Discount Ride → Spot

CLI = Controlling AWS without opening browser.

---

# 2. Load Balancers & Types | ALB vs NLB | Routing Types

## Technical Explanation

Load Balancer distributes incoming traffic.

Architecture:

```text
Users
 ↓
Load Balancer
 ↓
Application Servers
```

---

## Types

### ALB

Layer 7

Supports:

* HTTP
* HTTPS
* Path routing
* Host routing

Example:

```text
/api → Backend
/ui → Frontend
```

---

### NLB

Layer 4

Supports:

* TCP
* UDP

Features:

* Static IP
* Ultra-low latency

---

### Gateway LB

Used with:

* Firewalls
* Security appliances

---

## Routing Types

### Path Based

```text
/app → Server1
/api → Server2
```

### Host Based

```text
shop.example.com
blog.example.com
```

### Weighted

Traffic split.

```text
80% → V1
20% → V2
```

---

## Comparison

| Feature  | ALB      | NLB   |
| -------- | -------- | ----- |
| Layer    | 7        | 4     |
| Protocol | HTTP     | TCP   |
| Routing  | Advanced | Basic |

---

## Layman Explanation

Receptionist distributing visitors.

---

# 3. S3 (Types of S3 & Lifecycle Policies)

## Technical Explanation

S3 = Object Storage.

Stores:

* Images
* Videos
* Backups
* Logs

Architecture:

```text
User
 ↓
S3 Bucket
 ↓
Objects
```

---

## Storage Classes

### Standard

Frequent access.

### Standard IA

Less frequent.

### One Zone IA

Single AZ.

### Glacier Instant

Archive.

### Glacier Flexible

Long-term.

### Deep Archive

Lowest cost.

---

## Lifecycle Policy

Automatically move/delete objects.

Example:

```text
30 Days → Standard IA
90 Days → Glacier
365 Days → Delete
```

Example:

```json
{
 "Expiration": {
   "Days":365
 }
}
```

Use Cases:

* Cost optimization
* Retention

---

## Layman Explanation

Move old files to cheaper storage automatically.

---

# 4. ASG & Policies

## Technical Explanation

Auto Scaling Group automatically adjusts EC2 count.

Architecture:

```text
Traffic
 ↓
ASG
 ↓
Instances
```

---

## Scaling Policies

### Target Tracking

```text
CPU > 70%
```

Auto scale.

---

### Step Scaling

Different actions for thresholds.

---

### Scheduled Scaling

Scale based on time.

---

### Predictive Scaling

Forecast traffic.

---

## Demo Flow

```text
High Traffic
 ↓
CloudWatch Alarm
 ↓
ASG
 ↓
Add Instances
```

Commands:

```bash
aws autoscaling describe-auto-scaling-groups
```

---

## Layman Explanation

Open extra billing counters during rush.

---

# 5. Launch Template vs Launch Configuration

## Technical Explanation

Both define EC2 settings.

---

## Launch Configuration

Legacy.

Example:

```text
AMI
Instance Type
Security Group
```

---

## Launch Template

Modern.

Supports:

* Versioning
* Spot
* Multiple configurations

---

## Comparison

| Feature        | Launch Template | Launch Config |
| -------------- | --------------- | ------------- |
| Versioning     | Yes             | No            |
| Spot           | Yes             | No            |
| Recommendation | Yes             | No            |

Architecture:

```text
ASG
 ↓
Launch Template
 ↓
EC2
```

Commands:

```bash
aws ec2 create-launch-template
```

---

## Layman Explanation

Launch Configuration = Old machine blueprint

Launch Template = Modern reusable blueprint

---

# Terraform & CDN – Week 2 Team Sessions (Batch-34)

## Total Topics Covered: 5

## Table of Contents

1. [Content Delivery Network (CDN)](#1-content-delivery-network-cdn)
2. [Terraform Meta-Arguments (depends_on, count, for_each, provider, lifecycle)](#2-terraform-meta-arguments-depends_on-count-for_each-provider-lifecycle)
3. [Terraform Modules vs Static](#3-terraform-modules-vs-static)
4. [Terraform State Management + Data Source + Dynamic Block](#4-terraform-state-management--data-source--dynamic-block)
5. [Terraform Commands Workflow](#5-terraform-commands-workflow)

---

# 1. Content Delivery Network (CDN)

## Technical Explanation

A **Content Delivery Network (CDN)** is a globally distributed network of edge servers that caches and delivers content closer to users.

Purpose:

* Reduce latency
* Improve website performance
* Reduce origin server load
* Improve availability

Examples:

* CloudFront
* Cloudflare
* Akamai

Architecture:

```text
User
 ↓
Nearest Edge Location
 ↓
Cached Content
 ↓
Origin Server
```

---

## CDN Working Flow

```text
User Request
 ↓
Edge Cache Check
 ↓
Cache Hit → Return Content
 ↓
Cache Miss
 ↓
Origin Fetch
 ↓
Store Cache
 ↓
Return Response
```

---

## Components

### Edge Location

Content delivery server.

### Origin

Actual application server.

### Cache

Temporary storage.

### TTL

Controls cache expiry.

---

## Benefits

* Faster delivery
* Reduced bandwidth
* Better user experience
* DDoS resistance

---

## Use Cases

* Streaming platforms
* Static websites
* APIs
* Global applications

---

## Layman Explanation

Instead of ordering a product from another country every time, you pick it from a nearby warehouse.

---

# 2. Terraform Meta-Arguments (depends_on, count, for_each, provider, lifecycle)

## Technical Explanation

Meta-arguments control Terraform resource behavior.

---

## depends_on

Explicit dependency.

Example:

```hcl
resource "aws_instance" "app" {
 depends_on = [
   aws_vpc.main
 ]
}
```

Use:
Ensure creation order.

Flow:

```text
VPC
 ↓
EC2
```

---

## count

Create multiple identical resources.

Example:

```hcl
resource "aws_instance" "server" {
 count=3
}
```

Result:

```text
server[0]
server[1]
server[2]
```

Use:
Fixed resource quantity.

---

## for_each

Create resources from collections.

Example:

```hcl
resource "aws_s3_bucket" "bucket" {
 for_each=toset([
 "dev",
 "prod"
 ])
}
```

Use:
Unique resources.

---

## provider

Configure cloud provider.

Example:

```hcl
provider "aws" {
 region="us-east-1"
}
```

Multiple Providers:

```hcl
provider "aws" {
 alias="prod"
}
```

Use:
Multi-region deployments.

---

## lifecycle

Control resource replacement.

Example:

```hcl
lifecycle {
 create_before_destroy=true
}
```

Options:

* ignore_changes
* prevent_destroy
* create_before_destroy

---

## Comparison

| Meta Argument | Purpose     |
| ------------- | ----------- |
| depends_on    | Ordering    |
| count         | Replication |
| for_each      | Collections |
| provider      | Cloud       |
| lifecycle     | Behavior    |

---

## Layman Explanation

Meta-arguments are instructions attached to work orders.

---

# 3. Terraform Modules vs Static

## Technical Explanation

Terraform supports two approaches.

---

## Static Configuration

Infrastructure written repeatedly.

Example:

```hcl
resource "aws_instance" "dev" {}
resource "aws_instance" "prod" {}
```

Problems:

* Duplication
* Maintenance overhead

---

## Modules

Reusable infrastructure blocks.

Example:

```hcl
module "ec2" {
 source="./modules/ec2"
}
```

Architecture:

```text
Main Config
 ↓
Module
 ↓
Resources
```

---

## Module Sources

### Local

```hcl
source="./modules"
```

### Registry

```hcl
source="terraform-aws-modules/vpc/aws"
```

### Git

```hcl
source="git::repo"
```

---

## Comparison

| Feature      | Static | Module |
| ------------ | ------ | ------ |
| Reusable     | No     | Yes    |
| Scalable     | Low    | High   |
| Maintainable | Low    | High   |

---

## Best Practices

* Use modules
* Version control modules
* Keep modules small

---

## Layman Explanation

Static = Build each house manually.

Module = Use reusable house blueprint.

---

# 4. Terraform State Management + Data Source + Dynamic Block

## Technical Explanation

Terraform stores infrastructure information in **state files**.

File:

```text
terraform.tfstate
```

---

## State Management

Purpose:

* Track resources
* Detect drift
* Enable updates

Flow:

```text
Terraform
 ↓
State
 ↓
Cloud
```

Remote State Example:

```hcl
backend "s3" {
 bucket="terraform-state"
}
```

Benefits:

* Collaboration
* Locking
* Recovery

---

## Data Source

Reads existing infrastructure.

Example:

```hcl
data "aws_vpc" "main" {
 default=true
}
```

Difference:

```text
resource → Create
data → Read
```

Use Cases:

* Existing VPC
* Existing Security Group

---

## Dynamic Block

Generate nested configurations.

Example:

```hcl
dynamic "ingress" {

 for_each=var.rules

 content {
   from_port=ingress.value
 }
}
```

Flow:

```text
Input
 ↓
Loop
 ↓
Generated Blocks
```

Use Cases:

* Security Groups
* Repeated configuration

---

## Layman Explanation

State = Project register
Data Source = Reading old records
Dynamic Block = Auto-generating forms

---

# 5. Terraform Commands Workflow

## Technical Explanation

Terraform workflow follows a standard lifecycle.

Architecture:

```text
Write Code
 ↓
Init
 ↓
Validate
 ↓
Plan
 ↓
Apply
 ↓
State
```

---

## terraform init

Initialize project.

```bash
terraform init
```

Downloads:

* Providers
* Modules

---

## terraform validate

Validate syntax.

```bash
terraform validate
```

---

## terraform fmt

Format files.

```bash
terraform fmt
```

---

## terraform plan

Preview changes.

```bash
terraform plan
```

---

## terraform apply

Create resources.

```bash
terraform apply
```

---

## terraform destroy

Delete resources.

```bash
terraform destroy
```

---

## terraform output

View outputs.

```bash
terraform output
```

---

## terraform state

Inspect state.

```bash
terraform state list
```

---

## terraform import

Import existing resources.

```bash
terraform import
```

---

## Workflow Example

```bash
terraform init
terraform validate
terraform fmt
terraform plan
terraform apply
terraform output
```

---

## Layman Explanation

Blueprint → Review → Build → Manage → Remove.

---



