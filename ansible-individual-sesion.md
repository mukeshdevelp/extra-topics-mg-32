# Ansible – Modules & Advanced Concepts Complete Learning Guide

## Table of Contents

1. [Utility Modules](#1-utility-modules)
2. [Package & Repository Management](#2-package--repository-management)
3. [System Administration Modules](#3-system-administration-modules)
4. [File Management Modules](#4-file-management-modules)
5. [Variables & Facts](#5-variables--facts)
6. [Execution Control](#6-execution-control)
7. [Plugins](#7-plugins)
8. [Performance & Optimization](#8-performance--optimization)

---

# 1. Utility Modules

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Utility modules execute general automation tasks.

---

## telnet

Check remote connectivity.

```yaml
- name: Test port
  telnet:
    host: 10.0.0.10
    port: 22
```

---

## wget

Download files.

```yaml
- get_url:
    url: https://example.com/app.zip
    dest: /tmp
```

---

## mail

Send email.

```yaml
- mail:
    host: smtp.gmail.com
    to: dev@test.com
    subject: Build Success
```

---

## fetch

Copy remote → local.

```yaml
- fetch:
    src: /tmp/test.log
    dest: ./logs
```

---

## expect

Interactive automation.

```yaml
- expect:
    command: passwd user
```

---

## debug

Print output.

```yaml
- debug:
    msg: "Deployment complete"
```

---

## command

Execute commands.

```yaml
- command: uptime
```

---

## raw

Execute without Python.

```yaml
- raw: apt update
```

---

## script

Execute local script.

```yaml
- script: deploy.sh
```

---

## setup

Collect facts.

```yaml
- setup:
```

---

## git

Clone repository.

```yaml
- git:
    repo: url
```

---

## stat

File information.

```yaml
- stat:
    path: /etc/passwd
```

---

## pip

Install Python packages.

```yaml
- pip:
    name: boto3
```

---

## tempfile

Create temp files.

```yaml
- tempfile:
    state: file
```

---

## fail

Force failure.

```yaml
- fail:
    msg: Invalid deployment
```

---

## package

Universal package installer.

```yaml
- package:
    name: nginx
```

---

## Layman Explanation

These modules are your toolbox.

---

# 2. Package & Repository Management

[⬆ Back to Table of Contents](#table-of-contents)

## apt_key

Add GPG key.

```yaml
- apt_key:
    url: key_url
```

---

## apt_repository

Add repository.

```yaml
- apt_repository:
    repo: ppa:test
```

---

## yum_repository

RHEL repository.

```yaml
- yum_repository:
    name: app
```

---

## yum_key

Import key.

```yaml
- rpm_key:
```

---

## apk

Alpine package.

```yaml
- apk:
    name: nginx
```

---

## unarchive

Extract archive.

```yaml
- unarchive:
    src: app.tar.gz
```

---

## Layman Explanation

Package modules install software automatically.

---

# 3. System Administration Modules

[⬆ Back to Table of Contents](#table-of-contents)

## user_modify

Manage users.

```yaml
- user:
    name: deploy
```

---

## timezone

Configure timezone.

```yaml
- timezone:
    name: Asia/Kolkata
```

---

## systemd

Control services.

```yaml
- systemd:
    name: nginx
```

---

## service

Service management.

```yaml
- service:
    state: restarted
```

---

## group

Create groups.

```yaml
- group:
    name: admins
```

---

## firewalld

Firewall config.

```yaml
- firewalld:
    port: 80/tcp
```

---

## cron

Schedule jobs.

```yaml
- cron:
    minute: "0"
```

---

## Layman Explanation

Admin operations automated.

---

# 4. File Management Modules

[⬆ Back to Table of Contents](#table-of-contents)

## blockinfile

Insert blocks.

```yaml
- blockinfile:
```

---

## lineinfile

Edit single line.

```yaml
- lineinfile:
```

---

## find

Search files.

```yaml
- find:
```

---

## Layman Explanation

Automated text editor.

---

# 5. Variables & Facts

[⬆ Back to Table of Contents](#table-of-contents)

## set_facts

Create runtime variables.

```yaml
- set_fact:
    env: prod
```

---

## meta

Execution control.

```yaml
- meta: flush_handlers
```

---

## Layman Explanation

Temporary memory.

---

# 6. Execution Control

[⬆ Back to Table of Contents](#table-of-contents)

## Async and Poll

Background execution.

```yaml
async: 300
poll: 10
```

---

## Conditionals

```yaml
when: env=="prod"
```

---

## Tags

```yaml
tags:
 - deploy
```

Run:

```bash
ansible-playbook play.yml --tags deploy
```

---

## Delegate_to

Execute elsewhere.

```yaml
delegate_to: localhost
```

---

## Parallelism & Forks

```ini
forks=10
```

---

## Layman Explanation

Traffic controller for automation.

---

# 7. Plugins

[⬆ Back to Table of Contents](#table-of-contents)

## Callback Plugins

Format output.

## Lookup Plugins

Read external data.

## Filter Plugins

Transform variables.

Example:

```yaml
{{ name | upper }}
```

---

## Layman Explanation

Plugins extend capabilities.

---

# 8. Performance & Optimization

[⬆ Back to Table of Contents](#table-of-contents)

## Ansible Lint

Validate playbooks.

Install:

```bash
pip install ansible-lint
```

Run:

```bash
ansible-lint playbook.yml
```

---

## Playbook Optimization

Best Practices:

✔ Disable unnecessary facts
✔ Use handlers
✔ Use roles
✔ Increase forks
✔ Prefer modules over shell

Example:

```yaml
gather_facts: false
```

---

# Summary

This guide covered:

✅ Utility Modules
✅ Package Management
✅ System Administration
✅ File Modules
✅ Variables
✅ Execution Control
✅ Plugins
✅ Optimization

Happy Learning 🚀
