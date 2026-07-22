# Tailscale Ansible

Ansible role for preparing a Debian-based host as a Tailscale node with basic network hardening and forwarding support.

## What it does

- Installs and starts Tailscale.
- Enables IPv4 and IPv6 IP forwarding permanently.
- Enables UDP GRO forwarding at boot through a systemd service.
- Installs and enables UFW with a deny-incoming policy.
- Allows inbound SSH on port `22`.
- Installs and enables Fail2ban.
- Hardens SSH by disabling password authentication and X11 forwarding.

## Requirements

- Ansible with SSH access and `sudo` privileges on the target host.
- The required collections:

  ```bash
  ansible-galaxy collection install ansible.posix community.general
  ```

## Inventory

The default inventory is [`inventory.ini`](inventory.ini):

```ini
[tailscale]
192.168.0.202
```

Replace the address with the host you want to configure. If the remote SSH user is different from your local user, add it to the host entry:

```ini
[tailscale]
192.168.0.202 ansible_user=admin
```

## Usage

Check the inventory and playbook syntax:

```bash
ansible-inventory --graph
ansible-playbook --syntax-check playbook.yml
```

Run the playbook:

```bash
ansible-playbook playbook.yml
```

If sudo requires a password, add `--ask-become-pass`:

```bash
ansible-playbook --ask-become-pass playbook.yml
```

## Important SSH note

The role disables SSH password authentication. Confirm that key-based SSH access works before applying it, or you may lose access to the target host.

## Project layout

```text
.
├── ansible.cfg
├── inventory.ini
├── playbook.yml
└── tailscale/
    ├── handlers/
    └── tasks/
        ├── networking/
        │   ├── firewall/
        │   ├── forwarding/
        │   └── ssh/
        └── tailscale/
```
