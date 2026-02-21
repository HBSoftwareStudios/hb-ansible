# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project description

Ansible project for server administration and service provisioning. The `baseline_config` role handles OS-level hardening, user management, firewall, logging, and container runtime setup.

## Commands

```bash
# Syntax check a playbook
ansible-playbook playbooks/<name>.yml --syntax-check

# Dry-run a playbook
ansible-playbook playbooks/<name>.yml --check --diff

# Run a playbook
ansible-playbook playbooks/<name>.yml

# Run specific tags only
ansible-playbook playbooks/site.yml --tags ssh,firewall

# Edit a vault file
ansible-vault edit <file>

# Lint check
ansible-lint
```

## Project structure

Standard Ansible directory layout:

- `inventories/` — Inventory files (hosts, groups)
- `playbooks/` — Playbooks
- `roles/` — Roles with standard structure (tasks, handlers, templates, defaults, vars, files)
- `group_vars/` — Group variables (vault-encrypted where needed)
- `host_vars/` — Host variables (vault-encrypted where needed)

## Conventions

- **Language (code)**: All code, task names, comments, and template comments must be in English.
- **Language (communication)**: Communicate with the user in German.
- **Secrets**: Must only go into Ansible Vault-encrypted files. Never put plaintext secrets in roles, playbooks, or templates. Insert placeholders — encryption is done manually.
- **New services**: Before creating a new role, analyze at least two existing comparable roles and adopt their structure and patterns.
- **Structure**: Strictly follow the existing directory and file structure. Do not introduce patterns that deviate from existing roles.
- **Tags**: Every task file imported in `tasks/main.yml` must have a corresponding tag. Tags allow selective deployment via `--tags`.
- **Idempotency**: All tasks must be idempotent. `changed` status should only occur on real changes. Handlers should use `changed_when: false` for command modules.
- **Warnings**: Ansible warnings must be investigated and resolved immediately. The playbook should run without any warnings.
- **Linting**: `ansible-lint` must pass cleanly (0 errors, 0 warnings).
- **Commits**: Never create commits autonomously. Commits are done exclusively via the `/ship` skill.
