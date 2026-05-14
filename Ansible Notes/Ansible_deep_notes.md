# Ansible Practical + Interview Questions Complete Notes

> Comprehensive revision notes covering:
>
> * Ansible practical setup
> * SSH authentication
> * Inventory
> * Ad-hoc commands
> * Playbooks
> * Modules
> * Roles
> * Handlers
> * Dynamic inventory
> * Ansible Tower basics
> * Real-world DevOps usage
> * Interview questions with answers

---

# 1. What is Configuration Management?

Configuration Management is the process of managing:

* Software installation
* Updates
* Security patches
* Server configuration
* Application setup
* Infrastructure consistency

across multiple servers automatically.

## Problem Without Configuration Management

Suppose company has:

* 100 Linux servers
* Ubuntu + CentOS mixed environments
* Need nginx installed everywhere

Doing manually:

* Time consuming
* Error-prone
* Difficult to maintain
* Scripts differ for each OS

## Solution

Use tools like:

* Ansible
* Puppet
* Chef
* SaltStack

Ansible became popular because:

* Agentless
* Simple YAML syntax
* Easy SSH-based setup
* Huge community support

---

# 2. Why Ansible?

## Advantages of Ansible

### 1. Agentless

No agent installation required on target servers.

Only requirement:

* SSH access (Linux)
* WinRM (Windows)

---

### 2. Simple YAML Syntax

Playbooks are written in YAML.

Easy to read and maintain.

---

### 3. Idempotent

Running same playbook multiple times:

* does not break system
* does not repeat unnecessary changes

Example:

If nginx already installed:

* Ansible skips reinstall.

---

### 4. Huge Module Ecosystem

Thousands of built-in modules available.

Examples:

* apt
* yum
* service
* copy
* file
* shell
* user

---

### 5. Strong Community Support

* Backed by Red Hat
* Python ecosystem support
* Large open-source contributions

---
## Configuration Drift

What is Configuration Drift?

Configuration drift happens when servers that were originally identical slowly become different over time due to:

manual changes
missed updates
inconsistent configurations
different package versions
Example

Initially:

100 identical servers

Later:

some servers manually updated
some missed patches
some configs modified manually

Now systems behave differently.

Why Configuration Drift Is Dangerous

Problems:

inconsistent production behavior
deployment failures
difficult debugging
security risks
unstable infrastructure
How Ansible Helps

Ansible enforces consistent configuration across servers using playbooks.

This helps maintain infrastructure consistency.

Desired State
What is Desired State?

Desired state means:

Defining how the system SHOULD look.

Example:

nginx should be installed
Docker service should be running
firewall should be disabled
Example
- name: Ensure nginx installed
  apt:
    name: nginx
    state: present

Ansible checks current system state and tries to match desired state.

Why Desired State Important

Benefits:

consistency
predictable automation
easier maintenance
safer repeated execution
Declarative vs Procedural
Procedural Approach

Procedural automation defines:

HOW to perform steps.

Example:

apt update
apt install nginx
systemctl start nginx

This is common in shell scripting.

Declarative Approach

Declarative automation defines:

WHAT final state should exist.

Example:

- name: Ensure nginx installed
  apt:
    name: nginx
    state: present

Ansible handles underlying logic automatically.

Why Declarative Automation Better

Advantages: used by Ansible

simpler automation
less scripting logic
easier maintenance
better readability
improved scalability

---
# 3. Ansible Architecture

## Components

### Control Node

Machine where:

* Ansible installed
* Playbooks written
* Commands executed

### Managed Nodes / Target Servers

Machines configured using Ansible.

---

## Communication Flow

```text
Control Node ---> SSH ---> Target Servers
```

No agents required.

---

# 4. Installing Ansible

## Ubuntu Installation

```bash
sudo apt update
sudo apt install ansible
```

## Verify Installation

```bash
ansible --version
```

---

# 5. SSH Passwordless Authentication

## Why Required?

Ansible automates tasks.

If password required every time:

* automation fails
* manual intervention needed

So we configure SSH key-based authentication.

---

# 6. SSH Key Generation

## Generate SSH Keys

```bash
ssh-keygen
```

Files generated:

```bash
~/.ssh/id_rsa
~/.ssh/id_rsa.pub
```

---

## Key Types

### Private Key

```bash
id_rsa
```

* Never share
* Used for authentication

### Public Key

```bash
id_rsa.pub
```

* Shared with target servers

---

# 7. Configure Passwordless Authentication

## Step 1: Generate Key

```bash
ssh-keygen
```

---

## Step 2: Copy Public Key

```bash
cat ~/.ssh/id_rsa.pub
```

Copy output.

---

## Step 3: Add to Target Server

Paste inside:

```bash
~/.ssh/authorized_keys
```

---

## Step 4: Verify

```bash
ssh ubuntu@<target-ip>
```

If login works without password:

SUCCESS.

---

# 8. Inventory File

Inventory file stores:

* Target server IPs
* Hostnames
* Groups

---

## Example Inventory

```ini
[web]
172.31.10.1
172.31.10.2

[db]
172.31.10.3
```

---

## Default Inventory Path

```bash
/etc/ansible/hosts
```

---

# 9. Ad-Hoc Commands

## What are Ad-Hoc Commands?

Quick one-time commands executed directly.

Used for:

* Testing
* Quick tasks
* Small operations

---

# 10. Ad-Hoc Command Syntax

```bash
ansible -i inventory all -m shell -a "command"
```

---

## Command Breakdown

### ansible

Runs ad-hoc command.

### -i

Inventory file.

### all

Target hosts.

### -m

Module.

### -a

Arguments.

---

# 11. Common Ad-Hoc Examples

## Create File

```bash
ansible -i inventory all -m shell -a "touch test"
```

---

## Delete File

```bash
ansible -i inventory all -m shell -a "rm test"
```

---

## Check CPU Count

```bash
ansible -i inventory all -m shell -a "nproc"
```

---

## Check Disk Usage

```bash
ansible -i inventory all -m shell -a "df -h"
```

---

# 12. Modules in Ansible

Modules are reusable functionalities.

Examples:

* apt
* yum
* service
* copy
* shell
* command
* user

---

## Why Modules Preferred Over Shell?

### Shell

```yaml
shell: apt install nginx
```

### Better Approach

```yaml
apt:
  name: nginx
  state: present
```

---

## Benefits of Modules

* Idempotent
* Safer
* Cleaner
* Easier maintenance
* Better OS compatibility

---

# 13. Ad-Hoc vs Playbooks

| Ad-Hoc              | Playbook              |
| ------------------- | --------------------- |
| One/few quick tasks | Multi-step automation |
| Temporary usage     | Reusable              |
| CLI command         | YAML file             |
| Less maintainable   | Highly maintainable   |

---

# 14. What is a Playbook?

Playbook = YAML automation file.

Used for:

* Multiple tasks
* Reusable automation
* Structured configuration

---

# 15. YAML Basics

YAML is indentation-sensitive.

Wrong indentation = failure.

---

## YAML Rules

* Use spaces
* Avoid tabs
* Maintain indentation consistency

---

# 16. First Ansible Playbook

## Example

```yaml
---
- name: Install and start nginx
  hosts: all
  become: true

  tasks:

    - name: Install nginx
      apt:
        name: nginx
        state: present

    - name: Start nginx
      service:
        name: nginx
        state: started
```

---

# 17. Playbook Explanation

## ---

Indicates YAML file.

---

## hosts

Defines target machines.

---

## become: true

Executes tasks as root user.

Equivalent to sudo.

---

## tasks

Actual operations executed.

---

## apt module

Installs packages.

---

## service module

Starts/stops services.

---

# 18. Running Playbooks

## Syntax

```bash
ansible-playbook -i inventory playbook.yaml
```

---

## Example

```bash
ansible-playbook -i inventory first-playbook.yaml
```

---

# 19. Gather Facts

Before tasks start:

Ansible gathers:

* OS info
* CPU
* Memory
* Network details
* Python availability

This step:

```text
Gathering Facts
```

is automatic.

---

# 20. Verbose Mode

Used for debugging.

## Syntax

```bash
ansible-playbook -i inventory playbook.yaml -v
```

More verbosity:

```bash
-vv
-vvv
```

---

## Purpose

Shows:

* SSH connections
* Module execution
* Internal operations
* Debug information

---

# 21. Grouping Servers

## Inventory Grouping

```ini
[web]
172.31.10.1

[db]
172.31.10.2
```

---

## Run Only On Web Servers

```bash
ansible -i inventory web -m ping
```

---

# 22. Real-World DevOps Workflow

## Common Production Flow

### Terraform

Creates:

* EC2 instances
* Infrastructure

### Ansible

Configures:

* nginx
* docker
* kubernetes
* applications

---

# 23. Why Roles are Needed?

Small playbooks are easy.

But real production automation may contain:

* 50+ tasks
* variables
* templates
* secrets
* handlers

Single file becomes messy.

Solution:

Roles.

---

# 24. What are Roles?

Roles organize playbooks into structured directories.

Benefits:

* Better organization
* Reusability
* Cleaner structure
* Easy maintenance

---

# 25. Creating Roles

## Command

```bash
ansible-galaxy role init kubernetes
```

---

## Generated Structure

```text
kubernetes/
├── defaults/
├── files/
├── handlers/
├── meta/
├── tasks/
├── templates/
├── tests/
├── vars/
```

---

# 26. Role Folder Explanation

Roles are one of the most important concepts in real DevOps environments.

In small projects:

* simple playbooks are enough

But in real organizations:

* automation becomes huge
* playbooks become difficult to manage
* multiple teams work together
* variables differ per environment
* production/staging/dev all behave differently

That is why roles are heavily used.

---

# Real-Life Scenario

Suppose company wants to automate:

* Kubernetes cluster setup
* Docker installation
* nginx reverse proxy setup
* Monitoring agent installation
* Security hardening

If all tasks written in one playbook:

```text
1000+ lines
```

Problems:

* difficult debugging
* poor readability
* impossible maintenance
* hard collaboration
* difficult reuse

Solution:

Create separate roles.

Example:

```text
roles/
├── docker/
├── kubernetes-master/
├── kubernetes-worker/
├── nginx/
├── monitoring/
└── security-hardening/
```

Now every responsibility is separated.

This is how real DevOps teams structure automation.

---

# Typical Production Role Structure

```text
roles/
└── nginx/
    ├── defaults/
    ├── files/
    ├── handlers/
    ├── meta/
    ├── tasks/
    ├── templates/
    ├── tests/
    └── vars/
```

---

# tasks/

Most important folder.

Stores actual automation tasks.

Example:

```yaml
# tasks/main.yaml

- name: Install nginx
  apt:
    name: nginx
    state: present

- name: Start nginx
  service:
    name: nginx
    state: started
```

---

# Real DevOps Usage of tasks/

DevOps engineers commonly automate:

* nginx installation
* Docker installation
* Kubernetes setup
* User creation
* Security patching
* Package updates
* Monitoring setup
* Application deployment
* Service restart

---

# handlers/

Handlers execute only when notified.

Used mainly for:

* restarting services
* reloading configuration
* conditional operations

---

# Why Handlers Important?

Without handlers:

Every playbook run may restart services unnecessarily.

That can:

* create downtime
* interrupt traffic
* affect production systems

Handlers avoid unnecessary restarts.

---

# Real Example

Suppose DevOps engineer updates nginx configuration.

Only if configuration changes:

* nginx should restart.

Example:

```yaml
- name: Copy nginx config
  copy:
    src: nginx.conf
    dest: /etc/nginx/nginx.conf
  notify: Restart nginx
```

Handler:

```yaml
handlers:
  - name: Restart nginx
    service:
      name: nginx
      state: restarted
```

---

# templates/

Stores Jinja2 template files.

Templates allow dynamic configuration generation.

---

# Why Templates Needed?

Different environments need different values.

Example:

Production:

```text
server_name=prod.company.com
```

Staging:

```text
server_name=staging.company.com
```

Instead of maintaining multiple config files:

Use one template.

---

# Example Template

```jinja2
server {
    listen 80;
    server_name {{ domain_name }};
}
```

During execution:

Ansible replaces variables dynamically.

---

# Real DevOps Usage of templates/

Used heavily for:

* nginx configs
* application configs
* Kubernetes configs
* environment files
* systemd services
* database configs

---

# files/

Stores static files.

Examples:

* SSL certificates
* shell scripts
* HTML files
* static configs
* binaries

---

# Example

Suppose DevOps team wants default website page.

Store:

```text
index.html
```

inside:

```text
files/
```

Then copy to target server.

Example:

```yaml
- name: Copy index file
  copy:
    src: index.html
    dest: /var/www/html/index.html
```

---

# Real DevOps Usage of files/

Commonly used for:

* SSL certificate distribution
* custom scripts
* monitoring agent configs
* static application files
* startup scripts

---

# vars/

Stores variables with higher precedence.

Example:

```yaml
nginx_port: 80
```

---

# Real Usage of vars/

DevOps engineers store:

* ports
* package names
* environment values
* URLs
* service names

inside vars.

---

# defaults/

Stores default variable values.

Lowest precedence.

Example:

```yaml
nginx_port: 80
```

Can later be overridden.

---

# Real Importance of defaults/

Suppose role shared across teams.

Default values help:

* standardization
* easier reuse
* safer execution

---

# meta/

Stores metadata information.

Example:

* author
* supported platforms
* dependencies
* license information

---

# Real Usage of meta/

Suppose organization shares roles internally.

meta/ helps teams understand:

* compatibility
* dependencies
* ownership

---

# tests/

Used for testing role behavior.

Helps validate:

* role correctness
* configuration behavior
* task execution

---

# Real DevOps Importance of tests/

Production automation without testing:

VERY dangerous.

One bad playbook can:

* break production
* stop applications
* cause downtime

Testing reduces risk.

---

# Example: Real Organization Workflow Using Roles

Suppose company launches 50 new servers.

DevOps engineers may:

## Step 1

Terraform creates servers.

## Step 2

Ansible roles configure:

### base-server role

* create users
* configure SSH
* install utilities

### docker role

* install Docker
* configure daemon

### monitoring role

* install node exporter
* configure monitoring

### nginx role

* configure reverse proxy
* deploy configs

### security role

* disable root login
* configure firewall
* apply hardening

This is very close to real-world DevOps work.

---

# Common DevOps Uses of Ad-Hoc Commands

Ad-hoc commands are heavily used for quick operational tasks.

---

# Real DevOps Ad-Hoc Examples

## 1. Check Disk Usage Across Servers

```bash
ansible -i inventory all -m shell -a "df -h"
```

Used during:

* storage alerts
* troubleshooting
* monitoring checks

---

## 2. Restart Service Everywhere

```bash
ansible -i inventory web -m service -a "name=nginx state=restarted"
```

Used during:

* deployments
* config updates
* emergency fixes

---

## 3. Check Server Uptime

```bash
ansible -i inventory all -m shell -a "uptime"
```

---

## 4. Install Package Quickly

```bash
ansible -i inventory all -m apt -a "name=curl state=present"
```

---

## 5. Create Emergency User

```bash
ansible -i inventory all -m user -a "name=devopsadmin state=present"
```

---

# When DevOps Engineers Prefer Ad-Hoc Commands

Used for:

* emergency operations
* quick fixes
* temporary actions
* testing
* troubleshooting

Not ideal for:

* long-term reusable automation

---

# Common DevOps Uses of Playbooks

Playbooks are used for repeatable structured automation.

---

# Real DevOps Playbook Examples

## 1. Web Server Setup

Tasks:

* install nginx
* configure firewall
* copy configs
* start service

---

## 2. Docker Installation

Tasks:

* install Docker
* enable service
* configure daemon
* add users to docker group

---

## 3. Kubernetes Node Setup

Tasks:

* disable swap
* install kubeadm
* configure container runtime
* join cluster

---

## 4. Monitoring Setup

Tasks:

* install monitoring agents
* configure exporters
* start services

---

## 5. Security Hardening

Tasks:

* disable root login
* configure SSH rules
* install fail2ban
* patch systems

---

# Why Playbooks Important in Organizations?

Because organizations need:

* repeatability
* standardization
* documentation
* automation consistency

Playbooks become infrastructure documentation itself.

---

# Real DevOps Difference Summary

## Ad-Hoc Commands

Used for:

* quick operations
* temporary tasks
* troubleshooting

Example:

```bash
ansible all -m shell -a "df -h"
```

---

## Playbooks

Used for:

* repeatable automation
* deployments
* provisioning
* long-term automation

Example:

```yaml
Install nginx + configure service
```

---

## Roles

Used for:

* large-scale automation
* reusable structures
* enterprise-grade organization

Example:

```text
Separate reusable nginx role
```

---

# 27. Handlers

Handlers run only when notified.

---

## Example

```yaml
- name: Copy nginx config
  copy:
    src: nginx.conf
    dest: /etc/nginx/nginx.conf
  notify: Restart nginx
```

---

## Handler

```yaml
handlers:
  - name: Restart nginx
    service:
      name: nginx
      state: restarted
```

---

# 28. Dynamic Inventory

## Problem

Servers created dynamically in cloud.

Manual inventory maintenance difficult.

---

## Solution

Dynamic inventory automatically discovers:

* EC2 instances
* cloud resources

---

# 29. Ansible Tower

Enterprise version of Ansible.

Provides:

* GUI
* RBAC
* Scheduling
* Logging
* Team collaboration

---

# 30. RBAC in Ansible Tower

RBAC = Role-Based Access Control.

Used to:

* restrict permissions
* separate developer/admin access
* manage team roles

---

# 31. Windows Support

## Linux Connection

```text
SSH
```

## Windows Connection

```text
WinRM
```

---

# 32. Parallel Execution in Ansible

Ansible executes:

* same task across multiple hosts in parallel
* tasks sequentially

Example:

Task1 → all servers
Task2 → all servers

---

# 33. Variable Precedence

Common variable locations:

* defaults
* vars
* group_vars
* extra-vars

Higher precedence overrides lower.

---

# 34. Secrets Management

Secrets should not be hardcoded.

Common solution:

* HashiCorp Vault
* Ansible Vault

---

# 35. Common Beginner Mistakes

## 1. Wrong YAML Indentation

Very common.

---

## 2. Using Shell Everywhere

Prefer modules.

---

## 3. Forgetting become: true

Package installation fails.

---

## 4. Wrong Inventory Groups

Tasks run on wrong servers.

---

## 5. No Passwordless SSH

Ansible connection fails.

---

# 36. Important Real-World Usage

## Common Uses

* Security patching
* nginx installation
* Docker installation
* Kubernetes cluster setup
* Database configuration
* Server hardening
* Application deployment

---

# 37. Most Asked Ansible Interview Questions with Answers

---

# Q1. What is Configuration Management?

## Answer

Configuration Management is the process of automating software installation, updates, patching, and server configuration across multiple systems consistently.

Tools:

* Ansible
* Puppet
* Chef

---

# Q2. Why is Ansible better than other tools?

## Answer

Advantages:

* Agentless
* SSH-based
* Easy YAML syntax
* Fast setup
* Strong community support
* Huge module ecosystem

---

# Q3. Why is Ansible called agentless?

## Answer

Ansible does not require agents on target servers.

It communicates using:

* SSH (Linux)
* WinRM (Windows)

---

# Q4. Difference Between Ad-Hoc Commands and Playbooks?

## Answer

### Ad-Hoc Commands

* Quick one-time tasks
* Direct CLI usage

### Playbooks

* Multi-step automation
* YAML files
* Reusable

---

# Q5. What is Inventory File?

## Answer

Inventory file stores:

* target server details
* groups
* hostnames/IPs

Used by Ansible to identify where tasks should run.

---

# Q6. What are Modules?

## Answer

Modules are reusable functionalities provided by Ansible.

Examples:

* apt
* yum
* service
* copy

---

# Q7. Why prefer modules over shell commands?

## Answer

Modules are:

* Idempotent
* Safer
* Easier to maintain
* More reliable

---

# Q8. What is Idempotency?

## Answer

Running same playbook multiple times should produce same result without unwanted repeated changes.

---

# Q9. What is become: true?

## Answer

Used to execute tasks with elevated privileges.

Equivalent to sudo.

---

# Q10. What are Roles?

## Answer

Roles organize complex playbooks into reusable structured directories.

Improves:

* readability
* maintainability
* reusability

---

# Q11. What is Ansible Galaxy?

## Answer

Ansible Galaxy is:

* community platform
* role sharing system
* role initialization utility

Command:

```bash
ansible-galaxy role init nginx
```

---

# Q12. What are Handlers?

## Answer

Handlers are tasks executed only when notified.

Used mostly for:

* restarting services
* conditional operations

---

# Q13. What is Dynamic Inventory?

## Answer

Dynamic inventory automatically fetches cloud resources instead of manually maintaining inventory files.

Useful in:

* AWS
* Azure
* GCP

---

# Q14. What is Ansible Tower?

## Answer

Enterprise version of Ansible providing:

* GUI
* RBAC
* Scheduling
* Logging
* Team management

---

# Q15. What is RBAC?

## Answer

Role-Based Access Control.

Used to control user permissions.

---

# Q16. Which protocol does Ansible use?

## Answer

Linux:

```text
SSH
```

Windows:

```text
WinRM
```

---

# Q17. Explain Parallel Execution in Ansible

## Answer

Ansible executes same task on multiple hosts in parallel.

Tasks themselves run sequentially.

---

# Q18. How does Ansible help organizations?

## Answer

Ansible reduces:

* manual effort
* human errors
* deployment time

Used for:

* patching
* server setup
* deployments
* infrastructure configuration

---

# Q19. How do you handle secrets in Ansible?

## Answer

Using:

* Ansible Vault
* HashiCorp Vault

Avoid storing passwords in plain text.

---

# Q20. Can Ansible be used for Infrastructure as Code?

## Answer

Yes, but primarily Ansible is a Configuration Management tool.

Terraform is preferred for infrastructure provisioning.

Common workflow:

* Terraform creates infrastructure
* Ansible configures infrastructure

---

# Q21. Explain a real-world Ansible use case

## Answer

Example:

Automating Kubernetes cluster setup:

* install packages
* configure nodes
* setup master/worker nodes
* reduce manual setup time

---

# Q22. What can be improved in Ansible?

## Answer

Possible improvements:

* Better Windows support
* Better IDE plugins
* More advanced debugging support

---

# 38. Practical Practice Tasks

## Beginner Tasks

### Task 1

Install Ansible on Ubuntu.

---

### Task 2

Setup SSH passwordless authentication.

---

### Task 3

Create inventory file.

---

### Task 4

Run ad-hoc commands:

* create file
* delete file
* check disk
* check CPU

---

### Task 5

Write playbook:

* install nginx
* start nginx

---

### Task 6

Group servers:

* web
* db

Run commands only on web group.

---

### Task 7

Create role using:

```bash
ansible-galaxy role init nginx
```

Explore folders.

---

# 39. Revision Tips

Focus strongly on:

* Inventory
* SSH setup
* Ad-hoc commands
* Playbooks
* YAML indentation
* Modules
* Roles
* Handlers

These are most asked in interviews.

---

# 40. Final Quick Revision

## Core Flow

```text
Install Ansible
→ Setup SSH Keys
→ Create Inventory
→ Run Ad-Hoc Commands
→ Write Playbooks
→ Organize Using Roles
```

---

# 41. Important Commands Cheat Sheet

## Install Ansible

```bash
sudo apt install ansible
```

---

## Verify

```bash
ansible --version
```

---

## Generate SSH Key

```bash
ssh-keygen
```

---

## Run Ad-Hoc Command

```bash
ansible -i inventory all -m shell -a "df -h"
```

---

## Run Playbook

```bash
ansible-playbook -i inventory playbook.yaml
```

---

## Verbose Mode

```bash
ansible-playbook -i inventory playbook.yaml -vvv
```

---

## Create Role

```bash
ansible-galaxy role init nginx
```
## Verify Connectivity

```bash
ansible all -m ping
```

## Install Package

```bash
ansible all -m apt -a "name=nginx state=present"
```

## Restart Service

```bash
ansible web -m service -a "name=nginx state=restarted"
```

## Check Disk Usage

```bash
ansible all -m shell -a "df -h"
```

## Run Playbook

```bash
ansible-playbook -i inventory playbook.yaml
```

## Dry Run / Check Mode

```bash
ansible-playbook playbook.yaml --check
```
Used to preview changes before execution.

## Verbose Debugging

```bash
ansible-playbook playbook.yaml -vvv
```
## List Inventory

```bash
ansible-inventory --list
```

## View Module Documentation

```bash
ansible-
```
---

# 42. Interview Preparation Advice

Do not memorize blindly.

Understand:

* WHY SSH required
* WHY modules preferred
* WHY roles needed
* WHY Terraform + Ansible used together

Interviewers easily detect memorized answers.

Practical understanding matters more.

## ansible roadmap idealised

- adhoc command usecases and practice then;                             |  see github examples yml for playbooks and roles creation..
- creation of playbooks then;                                           |  usecases of each for devops roles..
- creation and understanding of roles then;                             |  interview queations situational..
- then complete ansible specific playlist....
 
