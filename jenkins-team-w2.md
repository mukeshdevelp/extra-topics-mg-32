# Jenkins – Advanced Learning Guide (Week 2)

## Table of Contents

1. [Configuring Agents (RHEL, Ubuntu)](#1-configuring-agents-rhel-ubuntu)
2. [GitOps](#2-gitops)
3. [Shared Libraries](#3-shared-libraries)
4. [Jenkins Backup](#4-jenkins-backup)
5. [Continuous Deployment](#5-continuous-deployment)

---

# 1. Configuring Agents (RHEL, Ubuntu)

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Jenkins Agents (formerly called Slaves) execute jobs on separate machines.

Benefits:

* Distributed builds
* Faster execution
* Environment isolation
* Horizontal scaling

Architecture:

```text
Jenkins Controller
        ↓
-------------------
↓                 ↓
Ubuntu Agent   RHEL Agent
```

---

## Agent Connection Methods

### SSH Agent

Most common.

```text
Controller
↓
SSH
↓
Agent
```

---

### JNLP Agent

Inbound connection.

```text
Agent
↓
Connect
↓
Controller
```

---

## Ubuntu Setup

Install Java:

```bash
sudo apt update

sudo apt install openjdk-17-jdk
```

Create Jenkins user:

```bash
sudo useradd -m jenkins
```

---

## RHEL Setup

Install Java:

```bash
sudo yum install java-17
```

Create Agent:

```bash
sudo useradd jenkins
```

---

## Required Plugins

### SSH Build Agents Plugin

SSH connectivity.

### Node and Label Parameter Plugin

Agent selection.

### Monitoring Plugin

Agent health.

### Docker Plugin

Containerized agents.

---

## Layman Explanation

Controller = Manager

Agents = Workers

Manager gives tasks → Workers execute.

---

# 2. GitOps

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

GitOps manages infrastructure and deployments through Git.

Flow:

```text
Developer
 ↓
Git Repository
 ↓
Jenkins
 ↓
Deployment
```

Principles:

* Git as Source of Truth
* Pull-based deployment
* Declarative configs
* Automated reconciliation

---

## Jenkins GitOps Flow

```text
Git Push
↓
Webhook
↓
Pipeline
↓
Deploy
```

Example:

```groovy
pipeline {
 agent any

 stages {

 stage('Deploy'){
   steps{
     sh 'kubectl apply -f deployment.yaml'
   }
 }

}
}
```

---

## Required Plugins

### Git Plugin

Repository integration.

### GitHub Plugin

GitHub events.

### Pipeline Plugin

Automation.

### Kubernetes Plugin

Cluster integration.

### Generic Webhook Trigger Plugin

Webhook automation.

---

## Layman Explanation

GitOps = Store instructions in Git and let automation do deployments.

---

# 3. Shared Libraries

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Shared Libraries store reusable pipeline code.

Structure:

```text
shared-library

vars/

src/

resources/
```

Configure:

```text
Manage Jenkins
↓
System
↓
Global Pipeline Libraries
```

Example:

```groovy
@Library('common') _

deploy()
```

---

## Library Layout

```text
vars/
deploy.groovy

src/
com/company/
```

---

## Required Plugins

### Pipeline Plugin

Pipeline execution.

### Pipeline Shared Groovy Libraries

Library support.

### Git Plugin

Library repository.

---

## Layman Explanation

Shared Libraries = Common code package used across projects.

---

# 4. Jenkins Backup

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Backup protects Jenkins state.

Backup Includes:

* Jobs
* Plugins
* Credentials
* Configurations
* Build History

Location:

```bash
/var/lib/jenkins
```

---

## Manual Backup

```bash
sudo tar -czf backup.tar.gz \
/var/lib/jenkins
```

Restore:

```bash
tar -xzf backup.tar.gz
```

---

## Automated Backup

```text
Jenkins
↓
Backup Plugin
↓
S3
```

---

## Required Plugins

### ThinBackup Plugin

Backup automation.

### S3 Plugin

Cloud backup.

### Configuration as Code Plugin

Config recovery.

### Job Import Plugin

Restore jobs.

---

## Layman Explanation

Backup = Save game progress.

Restore = Continue from previous checkpoint.

---

# 5. Continuous Deployment

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Continuous Deployment automatically releases after successful validation.

Pipeline:

```text
Code
↓
Build
↓
Test
↓
Deploy
```

Difference:

| CI         | CD     |
| ---------- | ------ |
| Build/Test | Deploy |

---

## Example Pipeline

```groovy
pipeline {

agent any

stages {

stage('Build'){
steps{
sh 'mvn package'
}
}

stage('Deploy'){
steps{
sh 'kubectl apply -f deploy.yaml'
}
}

}

}
```

---

## Deployment Strategies

### Rolling Deployment

Gradual replacement.

### Blue Green Deployment

Switch traffic.

### Canary Deployment

Partial release.

---

## Required Plugins

### Pipeline Plugin

Automation.

### Kubernetes Plugin

K8s deployment.

### Docker Plugin

Container deployment.

### Blue Ocean Plugin

Visualization.

### Ansible Plugin

Server deployment.

---

## Layman Explanation

Continuous Deployment = Code pushed → automatically reaches users.

---



Happy Learning 🚀
