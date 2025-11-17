# Ansible Role: Ubuntu 24.04+ Hardening

This project contains an Ansible role designed to apply a security baseline (hardening) to an Ubuntu Server 24.04+ system (for example, an AWS EC2 instance).

The goal is to automate fundamental security configurations in a repeatable, scalable, and idempotent way. The role is based on widely accepted security best practices and benchmark recommendations.

## Main Features

This Ansible role automates the following hardening tasks:

- Firewall (UFW): installs and configures `ufw` with a default `deny` incoming policy.
- Secure SSH:
  - Disables `PermitRootLogin`.
  - Disables password authentication (only if an SSH key is detected).
  - Enables `LogLevel VERBOSE` for enhanced logging.
- Auditing and logs: installs and enables `auditd`.
- Kernel hardening (sysctl): applies secure sysctl settings to protect against common attack vectors (`log_martians`, `tcp_sync_cookies`, etc.).
- Password policies: configures `libpam-pwquality` and `login.defs` for stronger password requirements.
- File permissions: hardens permissions on sensitive files such as `/etc/shadow` and `/etc/ssh/sshd_config`.
- System cleanup: removes insecure or deprecated services such as `telnet`, `rsh`, and `ftp`.

## Repository Structure

```bash
ansible_hardening/
├── inventory.ini # Inventory of target hosts
├── playbook-hardening.yml # Playbook that runs the role
├── roles/
│ └── ansible-role-ubuntu-hardening/ # Role logic
└── key.pem # SSH key (add to .gitignore)
```


## Requirements

- Controller machine with Ansible installed (`pip3 install ansible`).
- Target node running Ubuntu Server 24.04+.
- SSH access using a private key.

## How to Use

### 1. Clone the repository

```bash
git clone [https://github.com/Kriptyon/ansible-hardening.git]
cd ansible_hardening
```

### 2. Configure the inventory (inventory.ini)
Example for an AWS EC2 instance:

```bash
[servers]
xxxxxxxxxx.compute.amazonaws.com

[all:vars]
ansible_user = ubuntu
ansible_ssh_private_key_file = ./key.pem
ansible_ssh_common_args = '-o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null'
ansible_python_interpreter = /usr/bin/python3.12
```

### 3. Protect your SSH key
```bash
chmod 400 ./key.pem
```

### 4. Test the connection
```bash
ansible -i inventory.ini servers -m ping
```

### 5. Run the playbook in dry-run mode
```bash
ansible-playbook -i inventory.ini playbook-hardening.yml --check
```

### 6. Apply the hardening
```bash
ansible-playbook -i inventory.ini playbook-hardening.yml
```

## Verifying Changes

### 1. Idempotency test
Run the playbook again.
If everything is already in the desired state, the PLAY RECAP should show changed=0.

### 2. Manual verification
```bash
sudo ufw status
sudo grep PasswordAuthentication /etc/ssh/sshd_config
systemctl status auditd
ls -l /etc/shadow
```



