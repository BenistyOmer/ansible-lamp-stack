# Automated Web Server Setup

> Ansible playbook that deploys a production-ready LAMP stack on rhel servers

## Overview

This project automates the deployment of a complete LAMP (Linux, Apache, MySQL, PHP) stack using Ansible. It transforms a fresh rhel 22.04 server into a production-ready web server with security hardening and best practices built-in.

**Goal:** Reduce manual server configuration from 2+ hours to under 5 minutes of automated deployment.

## Quick Start

```bash
# Clone this repository
git clone <repository-url>
cd automated-web-server-ansible-playbook

# Configure your inventory
cp inventory/hosts.example inventory/hosts
nano inventory/hosts

# Run the playbook
ansible-playbook -i inventory/hosts playbook.yml

# For encrypted secrets
ansible-playbook -i inventory/hosts --ask-vault-pass playbook.yml
```

## Features

- Fully automated LAMP stack installation
- Idempotent playbooks (safe to run multiple times)
- Security hardening (UFW firewall, MySQL secure installation)
- Role-based architecture for modularity
- Ansible Vault for secrets management
- Multi-environment support ready

## Tech Stack

- **Automation:** Ansible 2.15+
- **OS:** rhel 22.04 LTS
- **Web Server:** Apache 2.4
- **Database:** MySQL 8.0
- **Language:** PHP 8.1 with PHP-FPM

## Project Structure

```
automated-web-server-ansible-playbook/
├── playbook.yml              # Main playbook entry point
├── ansible.cfg               # Ansible configuration
├── inventory/
│   └── hosts                 # Server inventory
├── roles/
│   ├── common/               # Base system setup
│   ├── apache/               # Web server configuration
│   ├── mysql/                # Database setup
│   └── php/                  # PHP runtime
└── group_vars/
    └── all/
        ├── vars.yml          # Variables
        └── vault.yml         # Encrypted secrets
```

## Documentation

**[Learning Journey](JOURNEY.md)** - Follow my complete development process, challenges faced, and solutions discovered. See how this project evolved from concept to working automation.

## Prerequisites

- Ansible 2.15 or higher installed on control machine
- rhel 22.04 LTS target server(s)
- SSH access to target servers
- Sudo privileges on target servers

## Testing

```bash
# Syntax check
ansible-playbook --syntax-check playbook.yml

# Dry run (preview changes)
ansible-playbook --check -i inventory/hosts playbook.yml

# Test idempotency (run twice, second should show no changes)
ansible-playbook -i inventory/hosts playbook.yml
ansible-playbook -i inventory/hosts playbook.yml
```

## Verification

After deployment, verify the installation:

```bash
# Check Apache
curl http://your-server-ip

# Check PHP
ssh user@server "php -v"

# Check MySQL
ssh user@server "mysql -u root -p -e 'SELECT VERSION();'"

# Check firewall
ssh user@server "sudo ufw status"
```

## Security Considerations

- All passwords stored in Ansible Vault (encrypted)
- UFW firewall enabled with only necessary ports (22, 80, 443)
- MySQL root remote access disabled
- Apache security headers configured
- Server signatures disabled

## Common Commands

```bash
# Create vault for secrets
ansible-vault create group_vars/all/vault.yml

# Edit existing vault
ansible-vault edit group_vars/all/vault.yml

# Run with verbose output for debugging
ansible-playbook -vvv -i inventory/hosts playbook.yml

# List all tasks
ansible-playbook --list-tasks playbook.yml
```

## License

MIT License - See LICENSE file for details

## Acknowledgments

- [DigitalOcean Community Tutorials](https://www.digitalocean.com/community)
- [Official Ansible Documentation](https://docs.ansible.com/)
- [Ansible Examples Repository](https://github.com/ansible/ansible-examples)

---

**[Read the Learning Journey →](JOURNEY.md)**
