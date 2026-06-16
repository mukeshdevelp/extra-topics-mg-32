# Ansible – Complete Learning Guide (Week 2)

## Table of Contents

1. [Variable Precedence](#1-variable-precedence)
2. [Handlers + Facts](#2-handlers--facts)
3. [Jinja2](#3-jinja2)
4. [Ansible Roles](#4-ansible-roles)
5. [Ansible Strategies](#5-ansible-strategies)

---

# 1. Variable Precedence

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Variable precedence defines which variable value Ansible uses when the same variable exists in multiple places.

Higher precedence overrides lower precedence.

Order (low → high):

```text
Role Defaults
↓
Inventory Variables
↓
Inventory Group Variables
↓
Inventory Host Variables
↓
Playbook Variables
↓
Task Variables
↓
Extra Variables (-e)
```

Example:

Inventory:

```yaml
app_env: dev
```

Playbook:

```yaml
vars:
 app_env: prod
```

Run:

```bash
ansible-playbook playbook.yml
```

Output:

```text
prod
```

---

## Extra Variables Example

```bash
ansible-playbook deploy.yml \
-e "app_env=uat"
```

Highest priority.

---

## Layman Explanation

Imagine multiple managers giving instructions.

The highest authority wins.

---

## Best Practices

✔ Keep defaults in roles
✔ Override via inventory
✔ Use `-e` carefully

---

# 2. Handlers + Facts

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Handlers execute only when notified.

Facts collect system information.

---

## Handlers

Example:

```yaml
tasks:

- name: Install nginx
  apt:
    name: nginx
    state: present
  notify:
    - restart nginx

handlers:

- name: restart nginx
  service:
    name: nginx
    state: restarted
```

Flow:

```text
Task Changed
↓
Notify
↓
Handler Runs
```

---

## Facts

Gather system details.

View facts:

```bash
ansible all -m setup
```

Access facts:

```yaml
{{ ansible_hostname }}

{{ ansible_distribution }}
```

Example:

```yaml
- debug:
    msg: "{{ ansible_os_family }}"
```

---

## Layman Explanation

Handler = Electrician called only if repair needed.

Facts = Information collected before work starts.

---

## Best Practices

✔ Use handlers for restart
✔ Disable facts if not needed
✔ Reuse gathered facts

---

# 3. Jinja2

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Jinja2 is Ansible’s templating engine.

Used for:

* Dynamic values
* Conditional logic
* Loops
* Configuration generation

Syntax:

```yaml
{{ variable }}
```

---

## Variable Example

```yaml
message: "{{ env }}"
```

---

## Condition Example

```yaml
{% if env == "prod" %}
production
{% endif %}
```

---

## Loop Example

```yaml
{% for item in servers %}
{{ item }}
{% endfor %}
```

---

## Template Example

Create:

```text
nginx.conf.j2
```

Content:

```text
server_name {{ domain }};
```

Deploy:

```yaml
- template:
    src: nginx.conf.j2
    dest: /etc/nginx/nginx.conf
```

---

## Layman Explanation

Jinja2 = Fill blanks in a document automatically.

---

## Best Practices

✔ Use templates for configs
✔ Avoid complex logic
✔ Keep templates reusable

---

# 4. Ansible Roles

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Roles organize automation into reusable components.

Structure:

```text
roles/

common/

tasks/

handlers/

templates/

files/

vars/

defaults/
```

Create Role:

```bash
ansible-galaxy init nginx
```

---

## Example

Playbook:

```yaml
- hosts: web

roles:
 - nginx
```

Role Task:

```yaml
- name: Install nginx
  apt:
    name: nginx
```

---

## Flow

```text
Playbook
↓
Role
↓
Tasks
↓
Handlers
```

---

## Layman Explanation

Role = Ready-made project template.

---

## Best Practices

✔ One purpose per role
✔ Use defaults
✔ Keep reusable

---

# 5. Ansible Strategies

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Strategy controls task execution across hosts.

Default:

```text
linear
```

Execute host-by-host.

---

## Linear Strategy

```yaml
strategy: linear
```

Flow:

```text
Task1
↓↓↓
Host1 Host2 Host3
```

---

## Free Strategy

Hosts execute independently.

```yaml
strategy: free
```

---

## Serial Deployment

Deploy in batches.

```yaml
serial: 2
```

Example:

```text
Host1 Host2
↓
Host3 Host4
```

---

## Forks

Parallel workers.

```ini
forks=10
```

Config:

```text
/etc/ansible/ansible.cfg
```

---

## Example Playbook

```yaml
- hosts: web

strategy: free

tasks:

- shell: hostname
```

---

## Layman Explanation

Strategy = Traffic management.

Linear → One lane

Free → Multiple lanes

Serial → Group movement

---

## Best Practices

✔ Use linear for safety
✔ Use serial for production
✔ Increase forks carefully

---

# Summary

This guide covered:

✅ Variable Precedence
✅ Handlers
✅ Facts
✅ Jinja2
✅ Ansible Roles
✅ Strategies

Happy Learning 🚀
