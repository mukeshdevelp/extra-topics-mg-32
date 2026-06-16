# AWS – Complete Learning Guide

## Total Topics Covered: 40

## Table of Contents

1. [ALB vs NLB](#1-alb-vs-nlb)
2. [Amazon CloudTrail](#2-amazon-cloudtrail)
3. [Amazon CloudWatch](#3-amazon-cloudwatch)
4. [Amazon EBS](#4-amazon-ebs)
5. [Amazon EFS](#5-amazon-efs)
6. [AMI & Its Types](#6-ami--its-types)
7. [Application Load Balancer](#7-application-load-balancer)
8. [AWS Inspector](#8-aws-inspector)
9. [AWS Shield](#9-aws-shield)
10. [Calculate Cost Difference](#10-calculate-cost-difference)
11. [Cross Account Access of S3](#11-cross-account-access-of-s3)
12. [Dedicated Hosts](#12-dedicated-hosts)
13. [IAM Monitoring](#13-iam-monitoring)
14. [EC2 Instances – Reserved vs Spot vs On-Demand](#14-ec2-instances--reserved-vs-spot-vs-on-demand)
15. [Elastic Beanstalk](#15-elastic-beanstalk)
16. [Elastic IP](#16-elastic-ip)
17. [Gateway Load Balancer](#17-gateway-load-balancer)
18. [How to Select the Right CIDR](#18-how-to-select-the-right-cidr)
19. [IAM Permission Boundary](#19-iam-permission-boundary)
20. [Lambda Function](#20-lambda-function)
21. [Launch Templates vs Launch Configuration](#21-launch-templates-vs-launch-configuration)
22. [MFA](#22-mfa)
23. [NACL vs Security Group](#23-nacl-vs-security-group)
24. [Network Load Balancer](#24-network-load-balancer)
25. [OSI Model](#25-osi-model)
26. [Policy Condition & IAM Best Practices](#26-policy-condition--iam-best-practices)
27. [Predictive Scaling](#27-predictive-scaling)
28. [Roles & AWS Account](#28-roles--aws-account)
29. [Route53](#29-route53)
30. [S3 Global vs Regional Namespace](#30-s3-global-vs-regional-namespace)
31. [S3 Lifecycle Rule](#31-s3-lifecycle-rule)
32. [S3 Replication](#32-s3-replication)
33. [S3 Static Website Hosting](#33-s3-static-website-hosting)
34. [S3 Storage Classes](#34-s3-storage-classes)
35. [Scheduled Auto Scaling](#35-scheduled-auto-scaling)
36. [SES](#36-ses)
37. [Snapshot vs AMI](#37-snapshot-vs-ami)
38. [SNS](#38-sns)
39. [Trust Policy](#39-trust-policy)
40. [Web Identity & Classic Load Balancer](#40-web-identity--classic-load-balancer)

---

# 1. ALB vs NLB

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

AWS provides different load balancers for different traffic patterns.

**ALB (Application Load Balancer)**

* Operates at Layer 7
* Supports HTTP/HTTPS
* Path-based routing
* Host-based routing
* WebSocket support

**NLB (Network Load Balancer)**

* Operates at Layer 4
* Supports TCP/UDP/TLS
* Ultra-low latency
* Handles millions of requests

Architecture:

```text
Client
 ↓
Load Balancer
 ↓
Target Group
 ↓
Instances
```

## Layman Explanation

ALB → Smart receptionist.

NLB → Highway traffic controller.

## Comparison

| Feature   | ALB   | NLB  |
| --------- | ----- | ---- |
| Layer     | 7     | 4    |
| Routing   | Smart | Fast |
| Protocol  | HTTP  | TCP  |
| Static IP | No    | Yes  |

Use Cases:

* Microservices → ALB
* Gaming → NLB

---

# 2. Amazon CloudTrail

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

CloudTrail records AWS API activity.

Tracks:

* Console actions
* CLI activity
* SDK requests

Example:

```text
Create EC2
Delete S3
Modify IAM
```

Architecture:

```text
User
 ↓
AWS API
 ↓
CloudTrail
 ↓
Logs
```

## Layman Explanation

CloudTrail is AWS CCTV camera.

Use Cases:

* Auditing
* Compliance
* Security

Command:

```bash
aws cloudtrail describe-trails
```

---

# 3. Amazon CloudWatch

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Monitoring and observability service.

Monitors:

* Metrics
* Logs
* Events
* Alarms

Architecture:

```text
Application
 ↓
Metrics
 ↓
CloudWatch
 ↓
Alarm
```

Example Alarm:

```text
CPU > 80%
```

Layman:
Doctor monitoring patient vitals.

Commands:

```bash
aws cloudwatch list-metrics
```

Use Cases:

* Monitoring
* Alerts
* Dashboards

---

# 4. Amazon EBS

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Elastic Block Store.

Persistent block storage.

Types:

* gp3
* io2
* st1
* sc1

Attach:

```text
EC2
↓
EBS
```

Commands:

```bash
aws ec2 create-volume
```

Layman:
External hard disk.

Use Cases:

* Databases
* Persistent storage

---

# 5. Amazon EFS

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Managed network file system.

Supports:

* Shared access
* Multi-AZ

Architecture:

```text
EC2
 ↘
  EFS
 ↗
EC2
```

Comparison:

| EBS    | EFS    |
| ------ | ------ |
| Single | Shared |
| Block  | File   |

Layman:
Google Drive for servers.

---

# 6. AMI & Its Types

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Amazon Machine Image.

Contains:

* OS
* Packages
* Configurations

Types:

* Public
* Private
* Marketplace

Create:

```bash
aws ec2 create-image
```

Layman:
Laptop backup image.

---

# 7. Application Load Balancer

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Layer 7 load balancing.

Features:

* Host routing
* Path routing
* SSL termination

Example:

```text
/api → Backend
/web → Frontend
```

Use Cases:

* Kubernetes
* Microservices

Layman:
Reception desk.

---

# 8. AWS Inspector

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Automated vulnerability scanning.

Scans:

* EC2
* Containers
* Lambda

Architecture:

```text
Resource
 ↓
Inspector
 ↓
Findings
```

Use Cases:

* Security audits
* CVE detection

Layman:
Security guard inspecting rooms.

---

# 9. AWS Shield

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

DDoS protection service.

Types:

* Shield Standard
* Shield Advanced

Protects:

* CloudFront
* Route53
* ALB

Architecture:

```text
Internet
 ↓
AWS Shield
 ↓
Application
```

Layman:
Security gate stopping crowd attacks.

---

# 10. Calculate Cost Difference

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Cost comparison depends on:

* Compute
* Storage
* Data transfer
* Reservations

Formula:

```text
Total Cost =
Compute +
Storage +
Network
```

Tools:

* Cost Explorer
* Pricing Calculator
* Budgets

Example:

```text
On Demand > Reserved > Spot
```

Layman:
Compare electricity bill before buying appliances.

---

# End of Part 1 (Topics 1–10)

Next: Topics 11–20
# AWS – Complete Learning Guide (Part 2)

---

# 11. Cross Account Access of S3

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Cross-account access allows one AWS account to access S3 resources owned by another account.

Methods:

* Bucket Policy
* IAM Role
* ACL (legacy)

Architecture:

```text
Account A
(User)
   ↓
IAM Role
   ↓
Assume Role
   ↓
Account B
S3 Bucket
```

Example Bucket Policy:

```json
{
 "Effect":"Allow",
 "Principal":{
   "AWS":"arn:aws:iam::123456789012:root"
 }
}
```

Use Cases:

* Centralized logging
* Shared storage
* Multi-account architecture

Layman:
Allowing another company's employee to access your office using permission.

---

# 12. Dedicated Hosts

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Dedicated Host provides a physical server fully dedicated to your AWS account.

Features:

* Physical server visibility
* License compliance
* Hardware control

Comparison:

| Dedicated Host | Dedicated Instance |
| -------------- | ------------------ |
| Entire Host    | Isolated Instance  |

Use Cases:

* Oracle licensing
* Regulatory compliance

Layman:
Renting the entire apartment building instead of a room.

Command:

```bash
aws ec2 allocate-hosts
```

---

# 13. IAM Monitoring

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Monitor IAM activity using:

* CloudTrail
* CloudWatch
* Access Analyzer
* IAM Last Accessed

Monitor:

* Login attempts
* API calls
* Policy changes

Commands:

```bash
aws iam get-account-summary
```

Architecture:

```text
IAM
 ↓
CloudTrail
 ↓
Monitoring
```

Layman:
Attendance system for AWS users.

Use Cases:

* Audit
* Compliance
* Security

---

# 14. EC2 Instances – Reserved vs Spot vs On-Demand

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

### On-Demand

Pay per usage.

### Reserved

Commit for 1–3 years.

### Spot

Unused capacity at lower price.

Comparison:

| Type      | Cost   | Stability |
| --------- | ------ | --------- |
| On-Demand | High   | High      |
| Reserved  | Medium | High      |
| Spot      | Low    | Low       |

Use Cases:

On-Demand:

* Short-term apps

Reserved:

* Production

Spot:

* Batch jobs

Layman:

Taxi → On-demand
Monthly pass → Reserved
Discount ticket → Spot

---

# 15. Elastic Beanstalk

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Managed deployment platform.

Supports:

* Java
* Python
* Node.js
* .NET

Architecture:

```text
Code
 ↓
Beanstalk
 ↓
EC2
 ↓
Application
```

Deploy:

```bash
eb deploy
```

Features:

* Auto scaling
* Monitoring

Layman:
Upload code → AWS handles infrastructure.

---

# 16. Elastic IP

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Static public IPv4 address.

Features:

* Reassignable
* Persistent

Commands:

```bash
aws ec2 allocate-address
```

Attach:

```bash
aws ec2 associate-address
```

Use Cases:

* Stable public access

Layman:
Permanent house address.

---

# 17. Gateway Load Balancer

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Deploy and scale third-party appliances.

Examples:

* Firewall
* IDS
* Security appliances

Architecture:

```text
Traffic
 ↓
GWLB
 ↓
Firewall
 ↓
Application
```

Features:

* Layer 3
* Layer 4

Layman:
Security checkpoint before entering building.

---

# 18. How to Select the Right CIDR

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

CIDR determines IP range.

Formula:

```text
2^(32-prefix)
```

Examples:

| CIDR | Hosts |
| ---- | ----- |
| /24  | 256   |
| /16  | 65536 |

Selection:

* Future growth
* Isolation
* Environment separation

Layman:
Choosing apartment size before moving.

Example:

```text
10.0.0.0/16
```

---

# 19. IAM Permission Boundary

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Permission Boundary limits maximum permissions.

Flow:

```text
IAM Policy
 +
Boundary
 =
Effective Access
```

Example:

```json
{
 "Action":"ec2:*"
}
```

Use Cases:

* Delegated administration
* Prevent privilege escalation

Layman:
Parents set maximum spending limit.

---

# 20. Lambda Function

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Serverless compute service.

Flow:

```text
Event
 ↓
Lambda
 ↓
Execution
```

Triggers:

* API Gateway
* S3
* EventBridge
* CloudWatch

Example:

```python
def handler(event, context):
 return "Hello"
```

Deploy:

```bash
aws lambda create-function
```

Features:

* Pay per execution
* Auto scaling

Layman:
Order food only when hungry.

Use Cases:

* APIs
* Automation
* ETL
* Notifications

---

# End of Part 2 (Topics 11–20)

Next:
Topics 21–30
# AWS – Complete Learning Guide (Part 3)

---

# 21. Launch Templates vs Launch Configuration

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Launch Templates and Launch Configurations define EC2 instance settings for Auto Scaling.

### Launch Configuration

* Legacy option
* One version only
* Limited features

### Launch Template

* Recommended
* Supports versioning
* Supports advanced EC2 features

Comparison:

| Feature           | Launch Template | Launch Configuration |
| ----------------- | --------------- | -------------------- |
| Versioning        | Yes             | No                   |
| Spot Support      | Yes             | Limited              |
| Multiple Versions | Yes             | No                   |

Example:

```text
Auto Scaling
     ↓
Launch Template
     ↓
EC2
```

Use Cases:

* Auto Scaling
* Standardized deployments

Layman:
Launch Template = Saved machine blueprint.

---

# 22. MFA (Multi-Factor Authentication)

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

MFA adds another verification layer.

Authentication Factors:

* Password
* Mobile OTP
* Hardware Token

Architecture:

```text
User
 ↓
Password
 ↓
OTP
 ↓
Access
```

Enable:

```bash
aws iam enable-mfa-device
```

Use Cases:

* Secure IAM users
* Root account protection

Layman:
ATM card + PIN.

Best Practice:
Always enable MFA for root account.

---

# 23. NACL vs Security Group

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

### Security Group

* Stateful
* Instance level

### NACL

* Stateless
* Subnet level

Comparison:

| Feature   | SG       | NACL   |
| --------- | -------- | ------ |
| Stateful  | Yes      | No     |
| Deny Rule | No       | Yes    |
| Level     | Instance | Subnet |

Architecture:

```text
Internet
 ↓
NACL
 ↓
Security Group
 ↓
EC2
```

Layman:

Security Group → House door

NACL → Society gate

---

# 24. Network Load Balancer

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Layer 4 Load Balancer.

Supports:

* TCP
* UDP
* TLS

Features:

* Static IP
* High performance
* Low latency

Architecture:

```text
Client
 ↓
NLB
 ↓
EC2
```

Use Cases:

* Real-time systems
* Gaming
* Financial applications

Layman:
Traffic signal routing cars.

---

# 25. OSI Model

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

OSI = Open Systems Interconnection

Layers:

```text
7 Application
6 Presentation
5 Session
4 Transport
3 Network
2 Data Link
1 Physical
```

Examples:

| Layer | Protocol |
| ----- | -------- |
| 7     | HTTP     |
| 4     | TCP      |
| 3     | IP       |

Layman:

Sending parcel:

Application → Writing letter
Transport → Courier
Physical → Delivery

Mnemonic:

```text
Please Do Not Throw Sausage Pizza Away
```

---

# 26. Policy Condition & IAM Best Practices

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Conditions restrict access.

Example:

```json
{
 "Condition":{
  "IpAddress":{
   "aws:SourceIp":"10.0.0.0/16"
  }
 }
}
```

Best Practices:

* Least privilege
* Use roles
* Rotate credentials
* MFA
* Avoid root account

Layman:
Office entry only from approved locations.

---

# 27. Predictive Scaling

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Automatically predicts future demand.

Flow:

```text
Historical Data
 ↓
Forecast
 ↓
Auto Scaling
```

Use Cases:

* E-commerce
* High traffic applications

Benefits:

* Cost optimization
* Better performance

Layman:
Preparing extra seats before guests arrive.

---

# 28. Roles & AWS Account

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

### AWS Account

Container for AWS resources.

### IAM Role

Temporary permissions.

Architecture:

```text
User
 ↓
Assume Role
 ↓
AWS Resource
```

Use Cases:

* Cross-account access
* EC2 permissions

Layman:
Temporary office visitor pass.

Command:

```bash
aws sts assume-role
```

---

# 29. Route53

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Managed DNS service.

Routing Types:

* Simple
* Weighted
* Latency
* Geolocation
* Failover

Architecture:

```text
Domain
 ↓
Route53
 ↓
Target
```

Example:

```text
example.com
↓
ALB
```

Layman:
Phone contact list for internet.

Command:

```bash
aws route53 list-hosted-zones
```

---

# 30. S3 Global vs Regional Namespace

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

### S3 Bucket Namespace

Bucket names must be globally unique.

Example:

```text
my-company-data
```

Cannot exist again globally.

Regional Storage:
Objects physically stored in chosen region.

Comparison:

| Feature   | Bucket | Objects  |
| --------- | ------ | -------- |
| Namespace | Global | Regional |

Use Cases:

* Global accessibility
* Regional compliance

Layman:
Website domain unique globally but files stored locally.

---

# End of Part 3 (Topics 21–30)

Next:
Topics 31–40 + Final Summary
# AWS – Complete Learning Guide (Part 4)

---

# 31. S3 Lifecycle Rule

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

S3 Lifecycle automates object transition and deletion.

Actions:

* Transition objects between storage classes
* Expire objects
* Delete old versions

Flow:

```text
Upload
 ↓
S3
 ↓
Lifecycle Rule
 ↓
Archive/Delete
```

Example:

```text
30 Days → Standard IA
90 Days → Glacier
365 Days → Delete
```

Use Cases:

* Log archival
* Cost optimization
* Backup retention

Layman:
Move old clothes from room → storage → dispose.

---

# 32. S3 Replication

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Automatically copies objects.

Types:

### CRR

Cross Region Replication

### SRR

Same Region Replication

Architecture:

```text
Source Bucket
 ↓
Replication
 ↓
Destination Bucket
```

Requirements:

* Versioning enabled
* IAM permissions

Use Cases:

* Disaster recovery
* Compliance

Layman:
Photocopying documents to another locker.

---

# 33. S3 Static Website Hosting

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Host static websites using S3.

Supports:

* HTML
* CSS
* JS

Steps:

```text
Create Bucket
↓
Upload Website
↓
Enable Hosting
↓
Access URL
```

Example:

```text
index.html
error.html
```

Use Cases:

* Portfolio
* Frontend apps

Layman:
Upload files and make them public.

---

# 34. S3 Storage Classes

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Storage classes optimize cost.

Types:

| Storage Class    | Use             |
| ---------------- | --------------- |
| Standard         | Frequent access |
| Standard IA      | Less frequent   |
| One Zone IA      | Single AZ       |
| Glacier Instant  | Archive         |
| Glacier Flexible | Long-term       |
| Deep Archive     | Cheapest        |

Selection Criteria:

* Access frequency
* Recovery speed

Layman:
Choose different warehouse types.

---

# 35. Scheduled Auto Scaling

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Scale resources based on schedule.

Example:

```text
9AM → Scale Up
11PM → Scale Down
```

Architecture:

```text
Schedule
 ↓
Auto Scaling
 ↓
Instances
```

Use Cases:

* Office apps
* Business-hour workloads

Layman:
Open more billing counters during peak hours.

---

# 36. SES (Simple Email Service)

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Email sending service.

Supports:

* Transactional emails
* Marketing emails
* SMTP

Architecture:

```text
Application
 ↓
SES
 ↓
User Inbox
```

Use Cases:

* OTP
* Notifications
* Reports

Layman:
Courier service for emails.

Example:

```bash
aws ses send-email
```

---

# 37. Snapshot vs AMI

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

### Snapshot

Backup of storage volume.

### AMI

Template for launching instances.

Comparison:

| Feature     | Snapshot | AMI     |
| ----------- | -------- | ------- |
| Backup      | Yes      | Partial |
| Launch EC2  | No       | Yes     |
| Contains OS | No       | Yes     |

Architecture:

```text
EBS
 ↓
Snapshot

Snapshot
 ↓
AMI
 ↓
EC2
```

Layman:

Snapshot → Photo

AMI → Complete cloned machine

---

# 38. SNS (Simple Notification Service)

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Pub/Sub messaging service.

Protocols:

* Email
* SMS
* Lambda
* HTTP

Architecture:

```text
Publisher
 ↓
SNS Topic
 ↓
Subscribers
```

Use Cases:

* Alerts
* Notifications
* Event systems

Layman:
One message sent to many people.

Command:

```bash
aws sns create-topic
```

---

# 39. Trust Policy

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Trust policy defines who can assume IAM roles.

Example:

```json
{
 "Principal":{
  "Service":"ec2.amazonaws.com"
 }
}
```

Flow:

```text
User
 ↓
Assume Role
 ↓
Temporary Access
```

Difference:

| Trust Policy   | Permission Policy |
| -------------- | ----------------- |
| Who can access | What can access   |

Layman:
Visitor approval list.

---

# 40. Web Identity & Classic Load Balancer

[⬆ Back to Table of Contents](#table-of-contents)

## Web Identity

### Technical

Authenticate external users.

Examples:

* Google
* Facebook
* OIDC

Architecture:

```text
User
 ↓
Identity Provider
 ↓
AWS
```

Use Cases:

* Mobile apps
* Web login

Layman:
Login with Google.

---

## Classic Load Balancer (CLB)

### Technical

Legacy AWS load balancer.

Supports:

* HTTP
* HTTPS
* TCP

Architecture:

```text
Client
 ↓
CLB
 ↓
EC2
```

Difference:

| CLB    | ALB     | NLB     |
| ------ | ------- | ------- |
| Legacy | Layer 7 | Layer 4 |

Use Cases:

* Legacy applications

Layman:
Old traffic controller.

---

# Final AWS Summary

This guide covered:

## Compute

✅ EC2
✅ Lambda
✅ Beanstalk
✅ Launch Templates

## Networking

✅ Route53
✅ Load Balancers
✅ CIDR
✅ NACL

## Storage

✅ EBS
✅ EFS
✅ S3
✅ Snapshots

## Security

✅ IAM
✅ Shield
✅ Inspector
✅ MFA

## Monitoring

✅ CloudTrail
✅ CloudWatch

## Scaling

✅ Auto Scaling
✅ Predictive Scaling

## Integration

✅ SNS
✅ SES

---

# Total Topics Completed: 40

Happy Learning 🚀
