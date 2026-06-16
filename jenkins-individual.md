# Jenkins – Complete Learning Guide

## Table of Contents

1. [License Scanning](#1-license-scanning)
2. [Types of Job (DSL, Freestyle, etc)](#2-types-of-job-dsl-freestyle-etc)
3. [Secret Manager](#3-secret-manager)
4. [Backup Plugins](#4-backup-plugins)
5. [Build Triggers](#5-build-triggers)
6. [Build Environment](#6-build-environment)
7. [Build Steps](#7-build-steps)
8. [Post Build Actions](#8-post-build-actions)
9. [Global Tool Configuration](#9-global-tool-configuration)
10. [Manage Credentials](#10-manage-credentials)
11. [Configure Global Security](#11-configure-global-security)
12. [Build Monitor Plugin](#12-build-monitor-plugin)
13. [Extended Choice Parameters](#13-extended-choice-parameters)
14. [Active Choices Parameters](#14-active-choices-parameters)
15. [Git Branches Plugin](#15-git-branches-plugin)
16. [Upstream / Downstream Job](#16-upstream--downstream-job)
17. [Credentials Binding](#17-credentials-binding)
18. [Google SSO](#18-google-sso)
19. [Plugins](#19-plugins)
20. [Jenkins Configuration File](#20-jenkins-configuration-file)
21. [Authorization Strategies](#21-authorization-strategies)
22. [Email Notification](#22-email-notification)
23. [Maven Project / Job](#23-maven-project--job)
24. [Monitoring & Metrics Plugins](#24-monitoring--metrics-plugins)
25. [Reporting Plugins](#25-reporting-plugins)
26. [Credential Scanning](#26-credential-scanning)
27. [Build / Input Parameters](#27-build--input-parameters)
28. [Multibranch Pipeline](#28-multibranch-pipeline)

---

# 1. License Scanning

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

License scanning identifies open-source licenses used in dependencies.

Purpose:

* Detect GPL, MIT, Apache licenses
* Prevent legal violations
* Generate compliance reports

Common Tools:

* OWASP Dependency Check
* Sonatype Nexus IQ
* BlackDuck

Example Pipeline:

```groovy
stage('License Scan'){
 steps{
   dependencyCheck()
 }
}
```

---

## Layman Explanation

Think of license scanning like checking whether every ingredient used in your restaurant is legally approved.

---

# 2. Types of Job (DSL, Freestyle, etc)

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

### Freestyle Job

GUI-based build configuration.

### Pipeline Job

Code-based CI/CD.

```groovy
pipeline {
 agent any
 stages {
   stage('Build'){
     steps{
       echo 'Build'
     }
   }
 }
}
```

### Multibranch Pipeline

Automatically builds branches.

### Job DSL

Create jobs using Groovy.

---

## Layman Explanation

Freestyle = Manual driving

Pipeline = Auto driving

DSL = Factory automation

---

# 3. Secret Manager

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Stores sensitive data securely.

Examples:

* Passwords
* Tokens
* SSH Keys

Providers:

* Jenkins Credentials
* AWS Secrets Manager
* Vault

Example:

```groovy
withCredentials([
string(
credentialsId:'aws',
variable:'TOKEN'
)
])
```

---

## Layman Explanation

Secret Manager = Locker storing passwords.

---

# 4. Backup Plugins

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Backup protects:

* Jobs
* Plugins
* Credentials
* Jenkins Config

Popular:

* ThinBackup

Backup Location:

```bash
/var/lib/jenkins
```

---

## Layman Explanation

Backup = Taking a copy before system failure.

---

# 5. Build Triggers

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Automatically starts jobs.

Types:

* Poll SCM
* Webhook
* CRON
* Upstream Job

Example:

```text
H/5 * * * *
```

---

## Layman Explanation

Like setting an alarm clock for builds.

---

# 6. Build Environment

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Defines execution environment.

Examples:

* Variables
* Workspace
* Agent

```groovy
environment {
 APP=dev
}
```

---

## Layman Explanation

Kitchen setup before cooking.

---

# 7. Build Steps

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Actual commands executed.

Examples:

```bash
npm install
mvn package
docker build .
```

---

## Layman Explanation

Recipe steps.

---

# 8. Post Build Actions

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Executed after build.

Examples:

* Archive
* Notification
* Trigger jobs

```groovy
post{
 success{
  echo 'Done'
 }
}
```

---

## Layman Explanation

Things done after food is cooked.

---

# 9. Global Tool Configuration

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Configure tools globally.

Examples:

* Git
* Maven
* Docker
* JDK

Location:

```text
Manage Jenkins
→ Tools
```

---

# 10. Manage Credentials

[⬆ Back to Table of Contents](#table-of-contents)

## Credential Types

* Secret Text
* Username Password
* SSH Key
* Secret File

---

# 11. Configure Global Security

[⬆ Back to Table of Contents](#table-of-contents)

## Features

* Authentication
* Authorization
* CSRF
* Agent Security

---

# 12. Build Monitor Plugin

[⬆ Back to Table of Contents](#table-of-contents)

## Purpose

Visual dashboard.

Shows:

* Success
* Failure
* Queue

---

# 13. Extended Choice Parameters

[⬆ Back to Table of Contents](#table-of-contents)

Allows:

* Multi Select
* Checkboxes
* Dynamic Values

---

# 14. Active Choices Parameters

[⬆ Back to Table of Contents](#table-of-contents)

Dynamic parameters using Groovy.

Example:

```groovy
return ["dev","prod"]
```

---

# 15. Git Branches Plugin

[⬆ Back to Table of Contents](#table-of-contents)

Manage:

* Branch discovery
* PR builds
* Filtering

---

# 16. Upstream / Downstream Job

[⬆ Back to Table of Contents](#table-of-contents)

```text
Build
 ↓
Test
 ↓
Deploy
```

---

## Layman Explanation

Domino effect.

---

# 17. Credentials Binding

[⬆ Back to Table of Contents](#table-of-contents)

Inject secrets.

Example:

```groovy
withCredentials()
```

---

# 18. Google SSO

[⬆ Back to Table of Contents](#table-of-contents)

Login flow:

```text
User
↓
Google
↓
Jenkins
```

---

# 19. Plugins

[⬆ Back to Table of Contents](#table-of-contents)

Install:

```text
Manage Jenkins
→ Plugins
```

---

# 20. Jenkins Configuration File

[⬆ Back to Table of Contents](#table-of-contents)

Main File:

```bash
config.xml
```

Location:

```bash
/var/lib/jenkins/
```

---

# 21. Authorization Strategies

[⬆ Back to Table of Contents](#table-of-contents)

Types:

* Matrix
* Role Based
* Project Based

---

# 22. Email Notification

[⬆ Back to Table of Contents](#table-of-contents)

SMTP Setup.

Example:

```text
smtp.gmail.com
```

---

# 23. Maven Project / Job

[⬆ Back to Table of Contents](#table-of-contents)

Build:

```bash
mvn clean package
```

---

# 24. Monitoring & Metrics Plugins

[⬆ Back to Table of Contents](#table-of-contents)

Examples:

* Prometheus
* Grafana

Metrics:

* CPU
* Memory
* Queue

---

# 25. Reporting Plugins

[⬆ Back to Table of Contents](#table-of-contents)

Generate:

* JUnit
* HTML
* Coverage

---

# 26. Credential Scanning

[⬆ Back to Table of Contents](#table-of-contents)

Scan:

* Tokens
* Passwords
* Secrets

Tools:

* Gitleaks
* Trufflehog

---

# 27. Build / Input Parameters

[⬆ Back to Table of Contents](#table-of-contents)

Types:

```text
String
Choice
Boolean
Password
```

---

# 28. Multibranch Pipeline

[⬆ Back to Table of Contents](#table-of-contents)

Automatically discovers:

```text
feature/*
release/*
main
```

Example:

```text
repo
 ├── Jenkinsfile
 ├── feature
 └── main
```

---

# Summary

This guide covered:

✅ License Scanning
✅ Jenkins Jobs
✅ Secrets
✅ Security
✅ Plugins
✅ Monitoring
✅ Parameters
✅ Multibranch Pipelines

Happy Learning 🚀
