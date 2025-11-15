# Ansible Role: Ubuntu Server Hardening (24.04+)

This role applies professional hardening steps to Ubuntu 24.04+ systems.

**Features:**
- SSH hardening (root login, password auth, X11, log level)
- UFW firewall
- Password aging and complexity
- Remove insecure services
- Auditd installation & enablement
- Kernel sysctl hardening
- Optional IPv6 disable
- Core dump disable
- GRUB hardening

**Usage Example:**
```yaml
- hosts: ubuntu
  become: yes
  roles:
    - role: ansible-role-ubuntu-hardening
      ssh_port: 2222
      disable_ipv6: true
