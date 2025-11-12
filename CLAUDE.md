# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an Ansible-based automation project for deploying a complete LAMP (Linux, Apache, MySQL, PHP) stack on RHEL servers. The project aims to transform a fresh Linux server into a production-ready web server through automated configuration management.

**Current Status:** Early-stage portfolio project with documentation framework established, but core implementation (playbooks, roles, inventory) not yet created.

## Development Commands

### Ansible Playbook Operations

```bash
# Syntax validation
ansible-playbook --syntax-check playbook.yml

# Dry run (check mode - preview changes without applying)
ansible-playbook --check playbook.yml

# Run with inventory file
ansible-playbook -i inventory/hosts playbook.yml

# Verbose output for debugging
ansible-playbook -vvv playbook.yml

# List all tasks without execution
ansible-playbook --list-tasks playbook.yml

# Run with encrypted vault
ansible-playbook --ask-vault-pass playbook.yml
```

### Ansible Vault (Secrets Management)

```bash
# Create encrypted vault file
ansible-vault create group_vars/all/vault.yml

# Edit existing vault
ansible-vault edit group_vars/all/vault.yml

# View vault contents
ansible-vault view group_vars/all/vault.yml
```

### Testing Idempotency

Run the playbook twice consecutively - the second run should report 0 changes:
```bash
ansible-playbook -i inventory/hosts playbook.yml
ansible-playbook -i inventory/hosts playbook.yml  # Should show no changes
```

## Project Architecture

### Intended Directory Structure

When implementing this project, follow this structure:

```
automated-web-server-ansible-playbook/
├── playbook.yml              # Main playbook entry point
├── ansible.cfg               # Ansible configuration
├── inventory/
│   └── hosts                 # Server inventory
├── roles/
│   ├── common/               # Base system setup
│   │   ├── tasks/
│   │   ├── handlers/
│   │   ├── templates/
│   │   └── vars/
│   ├── apache/               # Web server role
│   ├── mysql/                # Database role
│   └── php/                  # PHP runtime role
├── group_vars/
│   └── all/
│       ├── vars.yml          # Non-sensitive variables
│       └── vault.yml         # Encrypted secrets (Ansible Vault)
└── docs/
    ├── screenshots/
    └── troubleshooting.md
```

### Role Responsibilities

**common/** - Base system configuration applicable to all servers:
- System package updates
- Essential tools installation (curl, wget, git, vim)
- Timezone and hostname configuration
- Basic security (fail2ban, UFW firewall)
- User management

**apache/** - Web server setup:
- Apache2 installation
- Virtual host configuration
- SSL/TLS configuration
- Required module enablement (rewrite, ssl, headers)
- Firewall rules for HTTP/HTTPS

**mysql/** - Database server:
- MySQL Server 8.0 installation
- Secure installation (remove test databases, anonymous users)
- Database and user creation
- Security hardening (root password, bind-address)

**php/** - PHP runtime:
- PHP-FPM installation
- Required modules (mysql, curl, gd, xml, mbstring)
- php.ini configuration
- Apache integration

## Key Design Principles

### Idempotency
Every task must be idempotent - safe to run multiple times without changing state unnecessarily. Use appropriate Ansible modules (apt, template, service) rather than shell commands when possible.

### Handlers
Use handlers for service restarts to avoid redundant restarts. Handlers only execute once at the end of the playbook run when notified.

### Variables over Hardcoding
All configurable values should be in variables files, not hardcoded in tasks. Use Jinja2 templates for configuration files.

### Security
- Store all passwords and sensitive data in Ansible Vault encrypted files
- Never commit unencrypted secrets
- Implement firewall rules (UFW)
- Follow MySQL secure installation practices
- Configure Apache security headers

## Documentation Standards

This project follows a portfolio-oriented documentation approach based on PROJECT_DOCUMENTATION_GUIDE.md principles:

### Key Documentation Files (To Be Created)

**README.md** - Quick start and overview (hub for all documentation)

**JOURNEY.md** - Day-by-day development log showing:
- Thought process and learning progression
- Challenges faced and solutions
- Commands used during debugging
- Screenshots of working solutions
- Metrics (time saved by automation)

**ARCHITECTURE.md** - Technical design decisions:
- Why Ansible over alternatives (shell scripts, Puppet, Chef)
- Role structure rationale
- Security considerations
- Design patterns used
- Testing strategy

**docs/troubleshooting.md** - Common issues and debugging approaches

### Documentation Writing Guidelines

When creating or updating documentation:
- Document errors and solutions (proves real learning vs copy-paste)
- Include the "why" behind technical decisions
- Show iterative development (v1 simple → v2 improved → v3 polished)
- Add screenshots for visual proof
- Include metrics when possible ("reduced setup time from 2 hours to 5 minutes")
- Link to resources used during development
- Be honest about limitations and future improvements

## Target Environment

- **Operating System:** RHEL 22.04 LTS (primary), with future support for CentOS/Debian
- **Web Server:** Apache 2.4
- **Database:** MySQL 8.0
- **PHP:** PHP 8.1 with PHP-FPM
- **Ansible:** Version 2.15+

## Security Requirements

1. Use Ansible Vault for all sensitive data (passwords, API keys)
2. Ensure `.gitignore` includes vault files: `*vault*.yml`
3. Implement UFW firewall with only necessary ports (22, 80, 443)
4. Apply MySQL security hardening (no anonymous users, no remote root)
5. Configure Apache security headers (X-Frame-Options, X-Content-Type-Options, X-XSS-Protection)
6. Disable Apache server signatures (ServerTokens Prod)
7. Rate limiting on SSH to prevent brute force attacks

## Testing Requirements

Before considering any playbook complete:

1. **Syntax Check:** Must pass `ansible-playbook --syntax-check`
2. **Idempotency Test:** Second run must report 0 changes
3. **Functional Verification:**
   - Apache responds: `curl http://server_ip`
   - MySQL accessible: `mysql -u appuser -p`
   - PHP working: `php -v`
4. **Security Validation:**
   - Firewall active: `sudo ufw status`
   - Only required ports open
   - MySQL root remote login disabled

## Common Pitfalls to Avoid

- Using `command` or `shell` modules when dedicated Ansible modules exist
- Hardcoding values instead of using variables
- Not handling idempotency properly (tasks that always report "changed")
- Committing unencrypted secrets to version control
- Not testing firewall rules (Apache installed but not accessible)
- Missing PHP module dependencies for target application (WordPress, etc.)
- Not using handlers for service restarts (causes redundant restarts)

## Future Enhancement Roadmap

As documented in PROJECT_DOCUMENTATION_GUIDE.md, planned improvements include:

1. Multi-environment support (dev/staging/production inventories)
2. Let's Encrypt SSL automation
3. Monitoring integration (Prometheus/Grafana)
4. Blue-green deployment capability
5. CI/CD pipeline for playbook testing
6. High availability setup (HAProxy, MySQL replication)
7. Multi-distribution support (CentOS, Debian)

## Reference Documentation

The repository contains comprehensive guides that should inform development:

- **PROJECT_DOCUMENTATION_GUIDE.md** - How to document the learning journey for portfolio impact
- **GITHUB_DOCUMENTATION_INTEGRATION.md** - How to structure and link documentation on GitHub
- **PROJECT_DESCRIPTION.md** - Project goals and reference implementations
