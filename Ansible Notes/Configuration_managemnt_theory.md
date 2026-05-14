# Configuration Management theory & Ansible

---

# 1. What is Configuration Management?

Configuration Management is the process of managing and maintaining server configurations automatically and consistently across infrastructure.

It helps DevOps teams:

* install packages
* apply updates/patches
* maintain same configurations
* reduce manual work
* avoid configuration drift

---

# 2. Why Configuration Management Was Needed

## Traditional System Administration Problems

Earlier, system administrators managed servers manually.

Example tasks:

* install Git
* update packages
* apply security patches
* configure applications

This became difficult because:

* servers increased massively
* cloud environments became dynamic
* microservices increased server count
* manual management caused inconsistencies

---

# 3. Real Problem → Solution Mapping

| Problem                  | Ansible Solution          |
| ------------------------ | ------------------------- |
| Different Linux commands | Common YAML abstraction   |
| Manual SSH into servers  | Centralized automation    |
| Human errors             | Automated execution       |
| Configuration drift      | Desired state enforcement |
| Scaling issues           | Push-based automation     |
| Maintaining scripts      | Reusable playbooks        |

---

# 4. Why Traditional Shell Scripting Became Difficult

## Different OS = Different Commands

Ubuntu:

```bash id="7m3nhd"
apt install git
```

CentOS:

```bash id="oh6o56"
yum install git
# or
dnf install git
```

Windows:

```powershell id="b8gc6y"
Install-WindowsFeature
```

Managing separate scripts for:

* Ubuntu
* CentOS
* Windows

became difficult at scale.

---

# 5. Cloud & Microservices Changed Everything

## Earlier Infrastructure

```text id="ffq9ii"
Few large servers
```

## Modern Infrastructure

```text id="hvh9ti"
Many small servers (microservices)
```

Cloud introduced:

* auto scaling
* dynamic infrastructure
* frequent server creation/deletion

This made automation mandatory.

---

# 6. Configuration Management Tools

Popular tools:

* Puppet
* Chef
* Salt
* Ansible

Most commonly used today:

# Ansible

---

# 7. Why Ansible Became Popular

---

## A. Push Model

### Definition

Ansible pushes configuration from control node to servers.

## Architecture

```text id="1vhj97"
Control Node ---> Managed Servers
```

### Why It Matters

* Faster deployment
* Centralized control
* Easier operations
* Better for dynamic cloud environments

---

## B. Agentless Architecture

### Definition

Ansible primarily uses:

* SSH (Linux)
* WinRM (Windows)

instead of continuously running agents.

### Why It Matters

* No agent installation
* Less maintenance
* Easier scaling
* Simpler setup
* Better cloud compatibility

### Real-World Benefit

When new cloud servers are created:

* just add inventory/access
* Ansible can manage immediately

No separate agent setup required.

---

# 8. Push vs Pull Model

---

## Push Model (Ansible)

Control node sends configuration.

```text id="6nt98e"
Control Node ---> Servers
```

### Advantage

Fast centralized execution.

---

## Pull Model (Puppet)

Servers request configuration from master.

```text id="gxx8vc"
Servers ---> Puppet Master
```

### Disadvantage

Requires:

* agents
* certificates
* additional maintenance

---

# 9. Puppet vs Ansible

| Feature            | Puppet     | Ansible |
| ------------------ | ---------- | ------- |
| Architecture       | Pull       | Push    |
| Agent Required     | Yes        | No      |
| Language           | Puppet DSL | YAML    |
| Setup Complexity   | Higher     | Lower   |
| Cloud Friendliness | Moderate   | High    |

---

# 10. Inventory File

Inventory file stores:

* server IPs
* DNS names

Example:

```ini id="y9z02l"
[web]
10.0.0.10
10.0.0.11

[db]
10.0.0.20
```

### Why It Matters

Allows centralized management of multiple servers.

---

# 11. Dynamic Inventory

## Static Inventory

Manually add server IPs.

## Dynamic Inventory

Ansible automatically discovers cloud servers.

### Used In

* AWS
* Azure
* GCP

### Why Important

Cloud servers scale dynamically.

---

# 12. YAML in Ansible

Example:

```yaml id="c5xx1e"
- hosts: web
  tasks:
    - name: Install git
      apt:
        name: git
        state: present
```

---

## Why YAML Became Popular

* Human readable
* Easy learning curve
* Easier code review
* Easier collaboration
* Already used in Kubernetes & CI/CD

---

# 13. Declarative vs Procedural

---

## Procedural (Shell Script Thinking)

Tell system:

> HOW to do something

Example:

```bash id="z7p8mj"
apt update
apt install git
```

---

## Declarative (Ansible Thinking)

Tell system:

> WHAT state should exist

Example:

```yaml id="9juz6p"
state: present
```

### Why Declarative Is Better

* Less scripting logic
* Easier maintenance
* Better scalability
* Reduced operational complexity

---

# 14. Desired State

Desired state means:
System should always remain in expected configuration.

Examples:

* Git must be installed
* Nginx must be running

Ansible enforces this state automatically.

---

# 15. Idempotency

## Definition

Running same playbook multiple times should NOT break system.

Example:
If Git is already installed:

* Ansible skips unnecessary reinstall

---

## Why It Matters

* Stability
* Predictable automation
* Safe repeated execution

---

# 16. Configuration Drift

## Definition

Servers that were originally identical slowly become different over time due to manual changes.

---

## Why It Is Dangerous

* inconsistent environments
* deployment failures
* difficult debugging
* security risks

---

## Example

Out of 100 servers:

* some updated manually
* some missed patches
* some changed configs

Now production behaves inconsistently.

---

# 17. Windows & Linux Support

## Linux

Uses:

```text id="ccn7qq"
SSH
```

## Windows

Uses:

```text id="m7k4zh"
WinRM
```

---

# 18. Ansible Galaxy

Used to:

* share reusable modules
* reuse community roles
* contribute custom modules

---

# 19. Advantages of Ansible

* Agentless
* YAML based
* Easy learning curve
* Push model
* Good cloud support
* Centralized automation
* Reusable playbooks
* Large community support

---

# 20. Limitations of Ansible

* Debugging can be difficult
* Windows support still improving
* Performance issues at massive scale

---

# 21. Real-World Scenario

## Scenario

Company traffic increases suddenly during sale.

Infrastructure scales:

```text id="sjlwmq"
50 servers ---> 500 servers
```

Without configuration management:

* inconsistent package versions
* manual setup delays
* deployment failures
* human errors
* unstable production

With Ansible:

* centralized automation
* same configuration everywhere
* faster scaling
* reduced operational risk

---

# 22. Important Interview Keywords

* Configuration Management
* Push Model
* Pull Model
* Agentless Architecture
* Desired State
* Idempotency
* Configuration Drift
* Declarative Automation
* Inventory File
* Dynamic Inventory
* YAML
* SSH
* WinRM

---

# 23. Important Interview Questions

## Basic

1. What is configuration management?
2. Why is Ansible popular?
3. Difference between Puppet and Ansible?
4. Push vs Pull model?
5. What is agentless architecture?

---

## Intermediate

6. What is configuration drift?
7. What is desired state?
8. What is idempotency?
9. Why YAML in Ansible?
10. Why shell scripting becomes difficult at scale?

---

## Scenario-Based

11. Why is Ansible suitable for cloud environments?
12. What operational problems happen without configuration management?
13. Why are manual configurations dangerous?

---

# 24. Fast Revision Summary

## Why Configuration Management?

To automate and standardize server management.

## Why Ansible?

* agentless
* YAML based
* scalable
* cloud friendly
* centralized automation

## Biggest Operational Benefit

Maintaining consistency across large dynamic infrastructure.

---

# End of Notes

