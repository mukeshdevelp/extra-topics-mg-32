# Ansible Interview Questions & Answers

Based on your notes — organized by topic, with scenario-based questions mixed in.

---

## 1. Fundamentals & Concepts

**Q1. What is Ansible and what category of tool is it?**
Ansible is an open-source **SCM (Software Configuration Management)** / IT automation tool used to manage, build, and deploy infrastructure through code rather than manual processes. It falls under the broader category of Infrastructure as Code (IaC).

**Q2. What core problems does SCM/Ansible solve?**
- **Mass configuration** — deploying the same app/config across hundreds of servers manually is error-prone and slow.
- **Consistency** — differences between test and production environments cause apps to behave inconsistently; SCM enforces the same config everywhere.
- **Idempotency** — manual changes are hard to track or roll back; SCM tools can reapply a desired state safely and repeatedly, and roll back to a known-good version on failure.

**Q3. What is Infrastructure as Code (IaC)?**
IaC is the automation of IT operations (build, deploy, manage) by provisioning through code, rather than manual processes — e.g., provisioning Dev, Test, and Prod environments from a centralized, version-controlled codebase.

**Q4. What are the 5 phases of what an SCM tool generally does?**
Identify → Prepare → Design → Execute → Sustain (with continuous Monitoring throughout). Sometimes abbreviated as **IPDESM**:
1. Identify the changes/config needed
2. Prepare — plan how to handle changes, define roles, timelines
3. Design — high-level and low-level design, identify risks/vulnerabilities
4. Execute the design
5. Sustain — documentation creation (RCA, post-coup analysis)
6. Monitor the changes continuously

**Q5. What are the benefits of using a configuration management tool?**
Increased efficiency, stability & control; cost reduction (less tribal knowledge needed); enhanced system reliability; faster problem identification; decreased risk & greater security; faster restoration; better team coordination.

**Q6. Name some tools in the configuration management landscape.**
Ansible, SaltStack, Chef, Puppet, Puppet Labs.

**Q7. Why choose Ansible specifically — what are its core characteristics?**
Simple, powerful, agentless, open-source, versatile. It's push-based (stateless) rather than pull-based, works over SSH, and requires no agent software on target machines.

**Q8. What is Ansible used for (its use cases)?**
Orchestration, provisioning, automation, configuration management, application deployment.

---

## 2. Ansible vs Other Tools (Puppet, Chef, SaltStack)

**Q9. How does Ansible's architecture differ from Puppet/Chef (agent-based tools)?**
Puppet and Chef use a **master-agent** model — an agent must be installed and running on every client machine, and the client initiates a "pull" from the master.
Ansible is **agentless** — it only needs SSH access to the target and Python installed there; there's no daemon running on the client. This makes setup significantly easier.

**Q10. Compare Ansible, Puppet, Chef, and SaltStack across scalability, ease of setup, availability, management, and interoperability.**

| Factor | Puppet | SaltStack | Ansible | Chef |
|---|---|---|---|---|
| Scalability | ✔ | ✘ | ✘ | ✔ |
| Ease of Setup | ✘ (master-agent) | ✘ (master-agent) | ✔ (agentless, SSH) | ✘ (master-agent) |
| Availability | ✔ | ✔ | ✔ | ✘ |
| Management | ✘ | ✔ | ✔ | ✔ |
| Interoperability | Windows & Unix/Linux | — | — | — |

**Q11. What language does each tool use for its configuration?**
- Puppet — DSL (its own Puppet DSL)
- Chef — DSL (Ruby-based)
- SaltStack — YAML
- Ansible — YAML

**Q12. What other factors should you weigh when picking a CM tool?**
Configuration language, GitHub community activity, enterprise licensing cost, popularity, and proven success stories in production.

---

## 3. Architecture & Components

**Q13. Describe Ansible's high-level architecture.**
A **Control Machine/Node** (your machine, where Ansible is installed) connects to **Target/Managed Nodes** over SSH via a **Connection Plugin**. The control node holds the **Host Inventory**, **Playbooks**, **Core/Custom Modules**, and **Plugins** (email, logging, etc.), and pushes the desired configuration out to the managed hosts.

**Q14. What are the four main components of Ansible?**
1. **Modules** — the reusable units of work Ansible executes on hosts.
2. **Playbooks** — YAML files listing tasks to automate.
3. **Inventory** — list of managed hosts/groups.
4. **Plugins** — extend Ansible's core functionality (connection, filter, callback, lookup plugins, etc.).

**Q15. What are the prerequisites to start using Ansible?**
Basic Linux knowledge, SSH connectivity between control and target machines, YAML familiarity, a control machine and target servers, internet connectivity, and Python installed on the managed nodes.

**Q16. Steps to set up Ansible?**
Install Ansible on the control node (via apt/yum or pip/source) → verify the installation → configure `ansible.cfg` → configure the inventory.

---

## 4. Inventory

**Q17. What is an inventory file in Ansible?**
A file (default: `/etc/ansible/hosts`) that lists the managed hosts and how Ansible should connect to and treat them. It can be static (INI/YAML) or dynamic.

**Q18. What are the 4 things you can define in an inventory file?**
1. **Behavioral parameters** — how Ansible will talk to a host (e.g., `ansible_host`, `ansible_port`, `ansible_user`, `ansible_ssh_private_key_file`).
2. **Groups** — logical grouping of hosts, e.g. `[web]`.
3. **Groups of groups** — nesting groups using `:children`, e.g. `[east:children]`.
4. **Variables** — assigned per host or per group (`[db:vars]`).

**Q19. Give an example of grouping hosts and assigning group variables.**
```ini
[db]
db01
db02

[db:vars]
username=admin
password=admin
ansible_ssh_private_key_file=/home/user/Downloads/key.pem
ansible_user=ubuntu
```

**Q20. What is variable priority order when the same variable is defined in multiple places?**
From lowest to highest priority (roughly): inventory group vars → inventory host vars → playbook group_vars → playbook host_vars → play vars → **`-e` extra-vars on the command line (highest priority)**.
Command-line `-e` vars always win over everything else.

**Q21. What is an "alias" in the context of inventory?**
A shortcut/nickname for a host used inside the inventory file, e.g. `web1 ansible_host=192.168.1.10` — `web1` is the alias.

**Q22. Scenario: You need Ansible to auto-discover and keep an up-to-date list of EC2 instances instead of maintaining a static inventory file. How would you achieve this?**
Use **Dynamic Inventory**. Instead of a static host file, you configure a dynamic inventory plugin (e.g., the AWS EC2 plugin, using `boto3` under the hood) that queries the cloud provider's API in real time. Steps: install `boto3`, configure an `aws_ec2.yml` inventory source with plugin, regions, and filters, then point `ansible.cfg`'s `inventory` setting at it. This gives automatic host discovery, always up-to-date inventory, scalability, and multi-cloud/hybrid support.

---

## 5. Ad-hoc Commands & Modules

**Q23. What is an ad-hoc command in Ansible?**
A one-off command you run directly from the CLI to do something quick without writing/saving a playbook — good for testing or simple one-time tasks.
```
ansible localhost -a "touch /tmp/test.txt"
ansible webservers -m service -a "name=nginx state=restarted"
```

**Q24. What is an Ansible module?**
A module is the basic unit of work in Ansible — a self-contained piece of code (usually Python) that performs one specific task (installing a package, managing a service, copying a file, etc.) on the target/localhost via SSH.

**Q25. What are the 3 types of modules?**
1. **Core** — maintained/supported directly by Ansible (Red Hat).
2. **Extra** — created/maintained by the community.
3. **Deprecated** — no longer supported; a newer module is preferred or it will be removed.

**Q26. Why are modules described as idempotent, and why does that matter?**
Modules check the current state before acting — if the desired state already exists, they report "ok" and make no change; if not, they make the change and report "changed". This means running the same playbook multiple times is safe and won't cause unintended side effects. It's the main reason Ansible is reliable for repeated automation.

**Q27. Name some commonly used modules and what they do.**
- `copy` / `file` — manage files/directories
- `service` — start/stop/restart services
- `user` — manage user accounts
- `package` / `yum` / `apt` — install/remove software packages
- `setup` — gathers all Ansible facts about a host
- `shell` / `command` — run shell/command-line instructions
- `template` — deploy Jinja2 templates with variables
- `git` — manage Git repos/checkouts
- `cron` — manage cron jobs
- `lineinfile` — ensure a specific line is present/absent in a file
- `fetch` — pull files from remote hosts to the control node
- `unarchive` — extract tar/zip archives
- `debug` — print variables/messages for debugging

**Q28. How do you view documentation for a module?**
```
ansible-doc -l                 # list all modules
ansible-doc <module_name>      # show docs for a specific module
ansible-doc -s <module_name>   # show a "snippet" (skeleton usage)
```

**Q29. Scenario: You want to install nginx on all web servers only if it isn't already installed, and you must not use a playbook — just a quick command. How?**
```
ansible webservers -m apt -a "name=nginx state=present" --become
```
The `apt` module is idempotent — it checks first, so if nginx is already installed nothing happens (`ok`), otherwise it installs it (`changed`).

---

## 6. Playbooks

**Q30. What is a Playbook?**
A YAML file containing an ordered list of **tasks** (which use modules with arguments) to be executed against a set of hosts pulled from the inventory. Multiple plays can exist in one playbook.

**Q31. What are the basic rules of YAML syntax relevant to Ansible?**
- Case-sensitive
- File begins with `---` and (optionally) ends with `...`
- Comments start with `#`
- List items begin with `-`
- Indentation defines structure (no tabs)

**Q32. Write a simple playbook that creates a directory and a file.**
```yaml
---
- name: Task on web server
  hosts: web01
  tasks:
    - name: Create a directory
      ansible.builtin.file:
        path: /etc/file
        state: directory
        mode: '0755'

    - name: Create a file
      ansible.builtin.file:
        path: /home/ubuntu/app/file.txt
        state: touch
```

**Q33. How do you run a playbook?**
```
ansible-playbook playbook.yml -i inventory
```

**Q34. Scenario: You just changed nginx's config file with a playbook task, but nginx won't reload automatically. How do you make Ansible reload nginx only if the config actually changed?**
Use a **handler** notified from the task that changes the config:
```yaml
tasks:
  - name: Update nginx config
    template:
      src: nginx.conf.j2
      dest: /etc/nginx/nginx.conf
    notify: Restart nginx

handlers:
  - name: Restart nginx
    service:
      name: nginx
      state: restarted
```
Handlers only run if the task that notifies them reports `changed`, and they run **once**, after all playbook tasks complete — even if notified multiple times.

---

## 7. Handlers

**Q35. What is a handler and when does it run?**
A handler is a special task that only executes when explicitly **notified** by another task using the `notify:` keyword, and only if that task's status is `changed` (not if it's `ok`). Handlers run after all the regular tasks in the play have completed — commonly used to reload/restart a service after a config change.

**Q36. If two different tasks both notify the same handler, how many times does it run?**
Only **once**, at the end of the play — even if notified multiple times.

**Q37. Does a handler run if the notifying task fails or reports "ok" (no change)?**
No. A handler only fires if the task reports `changed`. If the task is `ok` or fails, the handler is skipped.

---

## 8. Variables & Facts

**Q38. What are variables in Ansible and where can they be defined?**
Variables let you parameterize playbooks. They can be defined in: the inventory file (host_vars/group_vars), directly in a playbook (`vars:`), in a separate vars file, or passed on the command line with `-e`.

**Q39. What are Ansible Facts?**
Facts are pieces of information automatically **gathered from the remote/managed system** (OS family, IP addresses, hostname, memory, etc.) and are frequently used in conditionals and templates.

**Q40. How do you view/gather facts about a host?**
```
ansible <host> -m setup
```
The `setup` module collects and returns all facts. You can filter, e.g.:
```
ansible servers -m setup -a "filter=ansible_os_family"
```

**Q41. What does `gather_facts: no` do and why would you use it?**
It turns off automatic fact-gathering at the start of a play, which speeds up playbook execution — useful when you don't need any fact-based data (e.g., simple tasks not depending on OS type, etc.).

**Q42. Scenario: You want a task to install `httpd` only on RedHat-based systems and `apache2`-equivalent only on Debian-based systems. How do you do this using facts?**
```yaml
tasks:
  - name: Install httpd on RedHat
    yum:
      name: httpd
      state: present
    when: ansible_os_family == "RedHat"

  - name: Install apache2 on Debian
    apt:
      name: apache2
      state: present
    when: ansible_os_family == "Debian"
```
This uses the `ansible_os_family` fact gathered automatically by the `setup` module.

---

## 9. Conditionals

**Q43. How do you run a task conditionally in Ansible?**
Using the `when:` keyword. It evaluates an expression (which can reference facts, variables, or registered results) and only runs the task if the expression is true.
```yaml
- name: Install nginx on Debian
  apt:
    name: nginx
    state: present
  when: ansible_os_family == "Debian"
```

**Q44. Scenario: You have two tasks in sequence, and task 2 should only run if task 1's result changed the state. How would you implement this?**
Register the result of task 1, then use `when` on task 2:
```yaml
- name: Install nginx
  apt:
    name: nginx
    state: present
  register: nginx_result

- name: Notify only if install changed something
  debug:
    msg: "nginx was freshly installed"
  when: nginx_result.changed
```

**Q45. What's the difference between `ignore_errors: yes` and normal error handling?**
By default, if a task fails, Ansible stops execution on that host and subsequent tasks are skipped. Setting `ignore_errors: yes` on a task tells Ansible to continue to the next task even if that particular task fails.

---

## 10. Loops

**Q46. How do you loop over a list of items in a task?**
```yaml
- name: Create multiple files
  file:
    path: "/tmp/{{ item }}"
    state: touch
  loop:
    - file3.txt
    - file4.txt
```
`{{ item }}` is the default loop variable referencing each element.

**Q47. Scenario: You need to install nginx, git, and htop in a single task instead of writing three separate tasks. How?**
```yaml
- name: Install multiple packages
  apt:
    name: "{{ item }}"
    state: present
  loop:
    - nginx
    - git
    - htop
```

**Q48. Scenario: You need to create multiple users, each with a different name and group. How would you loop with multiple sub-items?**
```yaml
- name: Add several users
  user:
    name: "{{ item.name }}"
    group: "{{ item.group }}"
    state: present
  loop:
    - { name: 'user1', group: 'admins' }
    - { name: 'user2', group: 'staff' }
```

---

## 11. include vs import

**Q49. What is the difference between `include_tasks` and `import_tasks`?**

| | `import_tasks` | `include_tasks` |
|---|---|---|
| Processing | Static — pre-processed at **parse time** | Dynamic — processed at **runtime** |
| Loops (`when`/`loop`) | Applies to each included task individually (like they were written inline) | Applies to the include statement as a whole |
| Performance | Faster | Slightly slower |
| Tags | All tasks always visible | Tasks only appear once the include is reached |
| Use case | Static, unconditional inclusion | Conditional or variable-driven, dynamic inclusion |

**Q50. When would you prefer `include_tasks` over `import_tasks`?**
When you need to decide **at runtime** which task file to include (e.g., based on a variable or a fact), or if you want loops/conditionals to apply to the include as a single unit rather than being expanded to each inner task.

---

## 12. Templates (Jinja2)

**Q51. What is a Jinja2 template in Ansible used for?**
Templates (`.j2` files) let you dynamically generate configuration files by embedding variables/facts into a base file, which Ansible then renders and copies to the remote host — useful for config files that differ per host/environment (e.g., different `listen` ports or backend IPs).

**Q52. How do you deploy a template?**
```yaml
- name: Deploy nginx config from template
  template:
    src: nginx.conf.j2
    dest: /etc/nginx/nginx.conf
  notify: Restart nginx
```

**Q53. Scenario: You want the same nginx template deployed to multiple backend web servers, but each server's IP should be inserted dynamically into the config. How?**
Store the value as a variable (in inventory or `group_vars`), reference it inside the `.j2` file with `{{ variable_name }}`, and use the `template` module — Ansible substitutes the real value per host at render time.

---

## 13. Ansible Vault

**Q54. What is Ansible Vault and why is it needed?**
Ansible Vault is a feature that lets you **encrypt sensitive data** — passwords, keys, secrets — stored in files, rather than keeping them in plaintext. It uses **AES256** encryption.

**Q55. What are the key Vault commands?**
```
ansible-vault create secrets.yml       # create a new encrypted file
ansible-vault encrypt secrets2.yml     # encrypt an existing file
ansible-vault decrypt secrets2.yml     # decrypt a file
ansible-vault view secrets.yml         # view without decrypting on disk
ansible-vault edit secrets.yml         # edit an encrypted file in place
```

**Q56. How do you run a playbook that uses vault-encrypted variables?**
```
ansible-playbook play.yml --ask-vault-pass
```
or by pointing to a vault password file:
```
ansible-playbook play.yml --vault-password-file vault_pass.file
```

**Q57. What's the difference between interactive and non-interactive vault password entry?**
- **Interactive**: `--ask-vault-pass` prompts you to type the password each run.
- **Non-interactive**: `--vault-password-file` points to a file containing the password (that file itself should never be committed to version control).

**Q58. Scenario: A playbook needs a database password stored securely, and you don't want it in plaintext in your Git repo. How would you set this up?**
1. `ansible-vault create secrets.yml` and store `db_password: <value>` inside — it's encrypted on save.
2. Reference the variable inside the playbook via `vars_files: secrets.yml`.
3. Run with `ansible-playbook play.yml --ask-vault-pass` (or a vault password file excluded from Git).
This way the password itself never appears in plaintext in your version-controlled files.

---

## 14. Roles & Ansible Galaxy

**Q59. What is a Role in Ansible?**
A role is a structured, reusable way of organizing a playbook into standard, identifiable directories/categories (tasks, handlers, templates, files, vars, defaults, meta) so that automation logic is modular, shareable, and easier to maintain than one giant playbook.

**Q60. What is the standard directory structure of a role?**
```
roles/
  role_name/
    defaults/    # default variables (lowest priority)
    files/       # static files to copy
    handlers/    # handler tasks
    meta/        # role metadata/dependencies
    tasks/       # main list of tasks
    templates/   # Jinja2 templates
    tests/       # test playbook/inventory
    vars/        # role variables (higher priority than defaults)
```

**Q61. How do you create a role skeleton?**
```
ansible-galaxy role init role_name
```

**Q62. What is Ansible Galaxy?**
A platform for sharing and discovering pre-built Ansible roles — you can search, download, and reuse community/vendor roles instead of writing everything from scratch, or upload/share your own.

**Q63. Key Ansible Galaxy CLI commands?**
```
ansible-galaxy role init <role_name>          # create new role skeleton
ansible-galaxy install <author.role_name>     # install a role from Galaxy
ansible-galaxy list                           # list installed roles
ansible-galaxy remove <role_name>             # remove a role
ansible-galaxy search <keyword>               # search roles by tag/platform
ansible-galaxy info <role_name>               # detailed info about a role
```

**Q64. How do you use a role inside a playbook?**
```yaml
- hosts: all
  roles:
    - geerlingguy.mysql
    - geerlingguy.apache
```

**Q65. What are the benefits of using roles vs one large playbook?**
Reusability, cleaner and more manageable structure, easier collaboration across teams, encapsulation of platform-specific logic — it takes a bit more setup time upfront but pays off significantly in the long run for larger projects.

**Q66. Scenario: You need to stand up a LAMP server (MySQL, Apache, PHP) using existing community roles. How would you approach this?**
1. Install roles from Galaxy: `ansible-galaxy install geerlingguy.mysql geerlingguy.apache geerlingguy.php`
2. Create a playbook referencing them:
```yaml
- hosts: all
  roles:
    - geerlingguy.mysql
    - geerlingguy.apache
    - geerlingguy.php
```
3. Run it: `ansible-playbook -i inventory lamp.yml`

---

## 15. Strategies & Execution Control

**Q67. What are Ansible execution strategies, and what does each do?**
- **linear (default)** — for each task, all hosts complete that task before moving to the next task together (execution is synced per task).
- **free** — each host runs through the entire play independently at its own pace, without waiting for other hosts.
- **serial** — runs the playbook against a limited batch of hosts at a time (useful to reduce blast radius during rolling changes).
- **host_pinned** — keeps task execution pinned to a specific host so tasks run through completely on one host before moving to the next, minimizing host-switching overhead.

**Q68. Scenario: You're deploying an update across 10 web servers and want to avoid taking down all of them simultaneously. Which strategy/feature would you use?**
Use `serial` to roll out in batches, e.g.:
```yaml
- hosts: webservers
  serial: 2
  tasks:
    - ...
```
This updates 2 servers at a time, reducing downtime risk (a rolling deployment pattern).

**Q69. What are some playbook optimization techniques?**
1. Turn off fact gathering (`gather_facts: no`) when facts aren't needed.
2. Use parallelism/forks (`--forks` / `forks` in `ansible.cfg`) to run against more hosts concurrently.
3. Enable SSH optimizations — `ControlPersist` and pipelining in `ansible.cfg`:
```ini
[ssh_connection]
ssh_args = -o ControlMaster=auto -o ControlPersist=60s
pipelining = True
```
4. Use `async`/`poll` for long-running tasks so Ansible doesn't block waiting on them.

**Q70. What do `async` and `poll` do?**
`async` sets the maximum time (in seconds) a task is allowed to run in the background before Ansible considers it failed. `poll` sets how often (in seconds) Ansible checks the status of that async task. Together they let you kick off long-running jobs without blocking the whole playbook run.

---

## 16. ansible.cfg & Config Settings

**Q71. Where does `ansible.cfg` live and what does it configure?**
Typically at `/etc/ansible/ansible.cfg` or `~/ansible.cfg` — it controls default behavior: user privilege settings, SSH connection settings, fact caching, connection/fact cache timing, inventory path, log path, module search path, etc.

**Q72. What does the `forks` setting control?**
It's the default number of parallel processes Ansible uses when running tasks against multiple hosts. Higher forks = more concurrency, but too many can overwhelm the control node or the network.

**Q73. What's the difference between `sudo_user` and `become_user`... (i.e., privilege escalation settings)?**
Older `sudo`/`sudo_user` settings have been replaced by the `become`/`become_user` mechanism, which is generic across privilege escalation methods (`sudo`, `su`, etc.), configured in `ansible.cfg` under `[privilege_escalation]`.

---

## 17. Scenario-Based Wrap-Up Questions

**Q74. Scenario: A playbook run fails midway on host 5 of 20 due to a package conflict. What happens to the other hosts, and how do you re-run just against the failed hosts?**
Ansible continues running the play against the remaining hosts unless you're using `any_errors_fatal`/`max_fail_percentage`; only the failed host halts *its own* execution. Ansible automatically creates a `.retry` file (if enabled) listing failed hosts, and you can re-run with:
```
ansible-playbook play.yml --limit @play.retry
```

**Q75. Scenario: You need the same playbook to behave differently in Dev vs Prod (e.g., different DB hostnames), without maintaining two separate playbooks. How?**
Use separate inventory files/group_vars per environment (`inventory/dev`, `inventory/prod`), each defining environment-specific variables. The playbook itself stays the same — you just point to a different inventory at runtime:
```
ansible-playbook site.yml -i inventory/prod
```

**Q76. Scenario: You accidentally hardcoded a plaintext password in a task and pushed it to Git. How should this have been done, and how do you fix it going forward?**
It should have been stored using **Ansible Vault** and referenced as a variable. Going forward: move the secret into a vault-encrypted file, rotate/change the exposed credential (since it's now in Git history), and purge it from history if needed (e.g., `git filter-repo`/BFG).

**Q77. Scenario: Two different teams manage 200+ EC2 instances that are constantly scaling up/down. Maintaining a static inventory is becoming unmanageable. What's your solution and why?**
Use **Dynamic Inventory** with the AWS EC2 plugin (`boto3`-backed). It automatically discovers hosts, stays continuously up-to-date, scales naturally with the fleet, supports metadata-based grouping (by tag, region, instance type), and works across multiple clouds/hybrid setups — eliminating the need to hand-maintain a static host file.

**Q78. Scenario: You want new hires to be able to spin up a full role-based project structure (tasks, handlers, templates, etc.) without manually creating every folder. How?**
```
ansible-galaxy role init my_new_role
```
This scaffolds the entire standard role directory structure automatically, so the new role immediately follows best-practice conventions.

**Q79. Scenario: Your playbook works fine against 5 servers, but as it scales to 500 servers, execution time balloons. What levers can you pull?**
- Increase `forks` in `ansible.cfg` for more parallelism.
- Switch to the `free` strategy so faster hosts aren't blocked waiting on slower ones.
- Disable unnecessary fact gathering.
- Enable SSH pipelining and `ControlPersist`.
- Use `async`/`poll` for long-running tasks.
- Consider `serial` batching if you also need to control blast radius, at some cost to total speed.

---
