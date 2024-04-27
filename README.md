## Ansible Role: Bootstrap

![CI](https://github.com/jokerwrld999/ansible-role-bootstrap/actions/workflows/ci.yaml/badge.svg)

## Description

This role provides basic pre-configuration tasks for your servers.

**Features**

- **System Setup**

  - Update System and Configure Repositories

- **User Configuration**

  - Configure Root User
    - Tags: lxc
  - Configure Custom User

- **Install Software**

  - Install Development Packages
    - Tags: lxc
  - Install Tweaks
  - Install Docker
    - Tags: docker
  - Install Utilities

- **Perform Tweaking**
  - Configure Hostname
  - Configure Logs Retention
  - Install Microcode
  - Configure OpenSSH
  - Install Qemu Guest Agent

## Usage

**Requirements**

This role requires some collections to be installed from ansible-galaxy. Here is an example `requirements.yml` file:

```
---
collections:
  - name: community.general
  - name: kewlfft.aur

roles:
  - name: jokerwrld999.bootstrap
```

**Install Requirements**

```
ansible-galaxy install -r requirements.yml
```

## Role Variables

Available variables are listed below, along with default values:

**Pre-Configuration**

- `set_hostname`: Boolean, defaults to `false`. Set to `true` to configure a
  custom hostname.

- `custom_hostname`: String, a template for the hostname using
  `${{ custom_user }}`.

  Example: `{{ custom_user }}-server`

**SSH**

- `ssh_port`: Integer, defaults to `22`. Set the desired SSH port.

**User's Config**

- `setup_user`: Boolean, defaults to `false`. Set to `true` to create a custom user with ZSH shell, p10k customizations and generated ssh keys of `ed25519` type.

- `custom_user`: String, defaults to the value found in environment variable
  `CUSTOM_USER`. Fallback is `"jokerwrld"`. Username for the custom user
  account.

  Example to create environment variable: `export CUSTOM_USER=username`.

- `temp_dir`: String, defaults to `"tmp"`. Temporary directory used for certain
  tasks.

**User's Password (Optional - Security Consideration)**

**Encrypt Password**

```python
python3 -c 'import crypt,getpass;pw=getpass.getpass();print(crypt.crypt(pw) if (pw==getpass.getpass("Confirm: ")) else exit())'
```

- `root_passwd`: String, empty by default. Set your Root password.

- `custom_user_passwd`: String, empty by default. Set your Custom user's
  password.

Consider using Ansible Vault or other secure credential management solutions.

**Variables File Example**

```yaml
---
# Pre-Configuration
# Custom Hostname
set_hostname: false
custom_hostname: "{{ custom_user }}-server"

# SSH
ssh_port: 22

# Users Config
setup_user: false
custom_user: "{{ lookup('env', 'CUSTOM_USER') | default('jokerwrld', true) }}" # >>> ubuntu | ec2-user | custom_user
temp_dir: "tmp"

# Encrypt Password
# python3 -c 'import crypt,getpass;pw=getpass.getpass();print(crypt.crypt(pw) if (pw==getpass.getpass("Confirm: ")) else exit())'
root_passwd: "$6$aBitRBNIk5O7.rRs$.r7jWebBbVsx6GL7/8EixidRnFxtvSPEcXMkCLa.zKXS8ETQjpay54Htc9Q4sLJfs1Cyvjj4VVGpe5yc1zXLe/" # root
custom_user_passwd: "$6$8ReSwQWsQjmGVfva$2/.pJ9aeIIXxAPhhpDuOhZ7161EKvAx2uFFdGpMC9tpMGEBO4m5MbGYR9nJloPrf68GSb7eSsN6Ef0FMKxkbQ1" # sudo
```

**Playbook Example**

```yaml
- name: Pre-configuring servers
  hosts: all
  become: true
  vars_files:
    - main.yml
  vars:
    set_hostname: true
    custom_hostname: "{{ custom_user }}-server"
    setup_user: true
    custom_user: myuser
    ssh_port: 2234
  tasks:
    - name: Pre-configuring servers
      block:
        - name: Include bootstrap role
          ansible.builtin.include_role:
            name: jokerwrld999.bootstrap
          tags: lxc, docker

      rescue:
        - name: Pre-Configuration | Rescue
          ansible.builtin.set_fact:
            task_failed: true
```

## Dependencies

None.

## License

MIT / BSD

## Author Information

This role was created in 2024 by [Joker Wrld](https://docs.jokerwrld.win/).
