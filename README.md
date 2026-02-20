# Workstation Setup

This repository contains my personal Ansible playbooks to set up a new Ubuntu/Debian workstation quickly and easily.

## Initial Setup (from scratch)

If you are setting up a completely new machine, you can run the playbook directly from GitHub using `ansible-pull`.

```bash
# Update system and restart if needed
sudo apt update && sudo apt upgrade -y
# sudo shutdown -r now

# Install dependencies
sudo apt install -y git python3-pip
pip3 install ansible

# Run the playbook directly from the repository
~/.local/bin/ansible-pull -U https://github.com/HeySlava/workstation local.yml -K
```

*(Note: For VirtualBox/VMware guests, you might also want to run `sudo apt install gcc make perl` first).*

## Running Locally

If you already have the repository cloned, you can run the playbook using `ansible-playbook`:

```bash
ansible-playbook local.yml -K
```
*(The `-K` or `--ask-become-pass` flag will prompt you for your `sudo` password, which is required for package installations).*

## Installing Specific Components (Using Tags)

The playbook is divided into roles and tasks using **tags**. This is incredibly useful if you only want to install or update a specific app (for example, just `neovim` or `docker`) without running the entire playbook.

To run only specific tasks, use the `--tags` parameter:

```bash
# Example: Install ONLY Neovim
ansible-playbook local.yml --tags neovim -K

# Example: Run multiple specific tags
ansible-playbook local.yml --tags "neovim,docker,node" -K
```

### Available Tags

Here is the list of available tags you can use:

- `antigravity` – Install Antigravity IDE/agent
- `apt` – Install basic default applications (curl, git, htop, etc.)
- `dbeaver` – Install DBeaver Community Edition
- `deadsnakes` – Install the deadsnakes PPA for Python versions
- `docker` – Install Docker and Docker Compose
- `dotfiles` – Clone personal dotfiles repository
- `i3` – Install i3 window manager and related tools
- `kubernetes` – Install kubectl and minikube
- `neovim` – Install the latest Neovim
- `node` – Install Node.js LTS via NodeSource
- `pip_packages` – Install Python packages (pynvim, etc.)
- `pre_commit` – Install and configure pre-commit hooks
- `snap_packages` – Install applications via Snap
- `ssh` – Generate SSH keys
- `stow` – Run GNU Stow to link dotfiles

### Skipping Tags

Conversely, if you want to run everything *except* a specific component, use `--skip-tags`:

```bash
# Example: Run everything but skip SSH key generation
ansible-playbook local.yml --skip-tags ssh -K
```

## Syntax Check
If you made changes to the YAML files and want to verify they are correct before running them:
```bash
ansible-playbook --syntax-check local.yml
```
