# Learning Journey

**[← Back to README](README.md)**

---

This document chronicles my development process for building an Ansible-based LAMP stack automation. I'm documenting challenges, solutions, and learnings to demonstrate real understanding beyond just the final code.

## Table of Contents

- [Planning Phase](#planning-phase)
- [Development Log](#development-log)
- [Challenges Faced](#challenges-faced)
- [Key Learnings](#key-learnings)
- [Metrics & Results](#metrics--results)
- [Future Improvements](#future-improvements)

---

## Planning Phase

### Initial Research

**Goal:** Automate LAMP stack deployment to reduce manual configuration time and eliminate human error.

**Research Questions:**
- Why Ansible over shell scripts, Puppet, or Chef?
- What's the best way to structure an Ansible project?
- How to handle secrets securely?
- What are LAMP stack security best practices?

**Resources Used:**
- DigitalOcean Ansible tutorials
- Official Ansible documentation
- GitHub example repositories (see [PROJECT_DESCRIPTION.md](PROJECT_DESCRIPTION.md))
- Ansible best practices guide

**Key Decision: Why Ansible?**

Considered alternatives:
1. **Shell Scripts** ❌
   - Not idempotent by default
   - Poor error handling
   - Harder to maintain as complexity grows

2. **Puppet/Chef** ❌
   - Requires agent installation on managed nodes
   - Steeper learning curve
   - Overkill for this use case

3. **Ansible** ✅
   - Agentless (SSH-based)
   - Idempotent by default
   - YAML syntax (human-readable)
   - Large community and module ecosystem
   - Perfect for learning DevOps fundamentals

### Initial Architecture Design

Decided on role-based structure:
- **common/** - Base system setup (updates, security, tools)
- **apache/** - Web server installation and configuration
- **mysql/** - Database setup with security hardening
- **php/** - PHP runtime and required modules

**Why roles?** Modularity and reusability. Each role can be tested independently and reused in other projects.

---

## Development Log

### Day 1: Project Setup

**What I Did:**
- Created project repository structure
- Researched Ansible role conventions
- Set up initial documentation framework

**Learning:** Start with documentation structure early - it's harder to remember details later.

### Day 2-3: [To Be Updated During Development]

*This section will be filled in as I build the actual playbooks and roles.*

**Planned Tasks:**
1. Create basic playbook.yml
2. Set up inventory file
3. Implement common role (base system setup)
4. Test on fresh Almalinux VM

### Day 4-5: [Apache Role]

*To be documented...*

### Day 6-7: [MySQL Role]

*To be documented...*

### Day 8-9: [PHP Role]

*To be documented...*

### Day 10: [Integration & Testing]

*To be documented...*

---

## Challenges Faced

*This section will document specific problems encountered and how they were solved. Each challenge will include:*
- **Problem:** Description of the issue
- **Debugging Steps:** What I tried to diagnose it
- **Root Cause:** What was actually wrong
- **Solution:** How I fixed it
- **Prevention:** How to avoid this in the future

### Example Template (To Be Filled During Development):

#### Challenge 1: [Title]

**Problem:**
[Describe what went wrong]

**Debugging Process:**
```bash
# Commands used to investigate
command1
command2
```

**Root Cause:**
[What I discovered]

**Solution:**
[How I fixed it]

```yaml
# Code example
- name: Fixed task
  module:
    parameter: value
```

**Learning:**
[What this taught me]

---

## Key Learnings

### Technical Skills

*To be updated as I progress:*

**Ansible Concepts:**
- [ ] Playbook structure and syntax
- [ ] Role-based organization
- [ ] Variables and variable precedence
- [ ] Handlers for service management
- [ ] Templates with Jinja2
- [ ] Ansible Vault for secrets
- [ ] Idempotency principles

**Linux Administration:**
- [ ] Package management (apt)
- [ ] Service management (systemd)
- [ ] Firewall configuration (UFW)
- [ ] File permissions and ownership
- [ ] MySQL administration

**DevOps Practices:**
- [ ] Infrastructure as Code (IaC)
- [ ] Idempotent operations
- [ ] Configuration management
- [ ] Security hardening
- [ ] Testing and validation

### Tools & Commands Learned

```bash
# Ansible commands
ansible-playbook --syntax-check playbook.yml
ansible-playbook --check playbook.yml
ansible-playbook -vvv playbook.yml
ansible-vault create/edit/view vault.yml

# Linux commands
systemctl status service_name
ufw status
netstat -tulpn
journalctl -u service_name

# MySQL commands
mysql_secure_installation
mysql -u user -p
SHOW DATABASES;
```

### DevOps Principles

**Idempotency is Critical:**
Running the same playbook multiple times should produce the same result. This means tasks should check current state before making changes.

**Test Frequently:**
Don't wait until everything is written. Test each role independently and iterate.

**Security from the Start:**
Don't add security as an afterthought. Build it into the automation from day one.

**Document as You Go:**
Capture challenges and solutions immediately while they're fresh in memory.

---

## Metrics & Results

*To be filled after completion:*

### Time Savings
- **Before Automation:** ~X hours per server (manual configuration)
- **After Automation:** ~X minutes per server
- **Time Saved:** X hours per deployment

### Code Quality
- Idempotency: X% (verified with multiple runs)
- Lines of Code: X
- Roles: X
- Tasks: X

### Testing
- Number of test runs: X
- Bugs fixed: X
- Iterations: X

---

## Future Improvements

### Short Term
1. [ ] Add more comprehensive error handling
2. [ ] Create example inventory files
3. [ ] Add molecule tests for roles
4. [ ] Improve variable documentation

### Medium Term
1. [ ] Multi-environment support (dev/staging/prod)
2. [ ] Let's Encrypt SSL automation
3. [ ] Add support for Debian/CentOS
4. [ ] Create CI/CD pipeline for testing

### Long Term
1. [ ] Monitoring integration (Prometheus/Grafana)
2. [ ] High availability setup (HAProxy, MySQL replication)
3. [ ] Blue-green deployment capability
4. [ ] Container migration path (Docker/Kubernetes)

---

## Reflections

### What Went Well
*To be filled...*

### What I'd Do Differently
*To be filled...*

### Most Valuable Learning
*To be filled...*

### Advice for Others
*To be filled...*

---

**[← Back to README](README.md)**
