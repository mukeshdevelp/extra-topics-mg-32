# Ansible – Complete Learning Guide (Week 1)

## Table of Contents

1. [Ansible Configuration File](#1-ansible-configuration-file)
2. [Ad Hoc Commands & Modules](#2-ad-hoc-commands--modules)
3. [YAML with Playbooks](#3-yaml-with-playbooks)
4. [Inventory + Hostvars / Groupvars](#4-inventory--hostvars--groupvars)
5. [SaltStack Basic Introduction](#5-saltstack-basic-introduction)

---

# 1. Ansible Configuration File

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Ansible configuration file controls how Ansible behaves globally.

Default configuration file:

```text
/etc/ansible/ansible.cfg
```

Priority Order:

```text
ANSIBLE_CONFIG
↓
./ansible.cfg
↓
~/.ansible.cfg
↓
/etc/ansible/ansible.cfg
```

Check active config:

```bash
ansible --version
```

---

## Common Parameters

Example:

```ini
[defaults]

inventory=inventory

remote_user=ubuntu

forks=10

host_key_checking=False

timeout=30
```

---

## Important Sections

### Defaults

Global settings.

### Privilege Escalation

```ini
[privilege_escalation]

become=True
```

### SSH Connection

```ini
[ssh_connection]

pipelining=True
```

---

## Layman Explanation

Think of `ansible.cfg` as system settings for Ansible.

Like changing settings in your phone.

---

## Best Practices

✔ Store config with project
✔ Avoid disabling security unnecessarily
✔ Configure inventory centrally

---

# 2. Ad Hoc Commands & Modules

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Ad Hoc Commands perform one-time operations without playbooks.

Syntax:

```bash
ansible HOST -m MODULE -a "ARGUMENT"
```

Structure:

```text
Host
↓
Module
↓
Arguments
```

---

## Common Commands

Ping:

```bash
ansible all -m ping
```

Run shell:

```bash
ansible all -m shell -a "uptime"
```

Copy file:

```bash
ansible all -m copy \
-a "src=test.txt dest=/tmp/"
```

Install package:

```bash
ansible web -m apt \
-a "name=nginx state=present"
```

Service:

```bash
ansible all -m service \
-a "name=nginx state=restarted"
```

---

## Common Modules

### ping

Connectivity.

### copy

File transfer.

### service

Service management.

### apt / yum

Package management.

### shell

Command execution.

### file

File operations.

---

## Layman Explanation

Ad Hoc = Quick command

Module = Tool performing work

---

## Best Practices

✔ Use for small tasks
✔ Avoid for production deployments
✔ Prefer playbooks for automation

---

# 3. YAML with Playbooks

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Playbooks define automation in YAML format.

Structure:

```text
Play
↓
Tasks
↓
Modules
```

Example:

```yaml
---
- hosts: web

  become: true

  tasks:

  - name: Install nginx

    apt:
      name: nginx
      state: present
```

Run:

```bash
ansible-playbook install.yml
```

---

## YAML Rules

### Key Value

```yaml
name: nginx
```

### List

```yaml
packages:
 - git
 - nginx
```

### Dictionary

```yaml
user:
 name: ubuntu
```

---

## Multiple Tasks

```yaml
tasks:

- name: Install

- name: Start

- name: Verify
```

---

## Layman Explanation

YAML = Recipe

Playbook = Complete cooking instructions.

---

## Best Practices

✔ Proper indentation
✔ Use variables
✔ Keep playbooks modular

---

# 4. Inventory + Hostvars / Groupvars

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Inventory defines target servers.

Example:

```ini
[web]

10.0.1.10

[db]

10.0.1.20
```

Run:

```bash
ansible all --list-hosts
```

---

## Host Variables

Assign variables per host.

Example:

```yaml
hostvars:

web1:
 env: prod
```

Usage:

```yaml
{{ hostvars['web1'].env }}
```

---

## Group Variables

Assign variables to groups.

Structure:

```text
inventory/

group_vars/

host_vars/
```

Example:

```yaml
group_vars/web.yml
```

```yaml
app_port: 8080
```

Use:

```yaml
{{ app_port }}
```

---

## Variable Flow

```text
Inventory
↓
Group Vars
↓
Host Vars
↓
Playbook
```

---

## Layman Explanation

Inventory = Contact list

Group Vars = Team settings

Host Vars = Individual settings

---

## Best Practices

✔ Keep inventory organized
✔ Separate environments
✔ Store variables properly

---

# 5. SaltStack Basic Introduction

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

SaltStack is configuration management and remote execution tool.

Architecture:

```text
Master
↓
Minions
```

Flow:

```text
Salt Master
↓
Commands
↓
Salt Minions
```

---

## Install

Master:

```bash
sudo apt install salt-master
```

Minion:

```bash
sudo apt install salt-minion
```

Start:

```bash
sudo systemctl start salt-master
```

---

## Basic Commands

Check:

```bash
salt '*' test.ping
```

Install package:

```bash
salt '*' pkg.install nginx
```

Service:

```bash
salt '*' service.start nginx
```

---

## Compare

| Feature   | Ansible  | SaltStack |
| --------- | -------- | --------- |
| Agent     | No       | Yes       |
| Transport | SSH      | ZeroMQ    |
| Speed     | Moderate | Fast      |

---

## Layman Explanation

Ansible = Remote control

SaltStack = Central command center

---

## Best Practices

✔ Use states for automation
✔ Secure master access
✔ Organize environments

---

# Summary

This guide covered:

✅ Ansible Configuration File
✅ Ad Hoc Commands
✅ Modules
✅ YAML & Playbooks
✅ Inventory
✅ Hostvars & Groupvars
✅ SaltStack Basics

Happy Learning 🚀
