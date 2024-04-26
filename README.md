## Ansible Role: Bootstrap

[![CI](https://github.com/jokerwrld999/ansible-role-bootstrap/workflows/CI/badge.svg?event=push)](https://github.com/jokerwrld999/ansible-role-bootstrap/actions?query=workflow%3ACI)

This role provides basic pre-configuration tasks for your servers.

**Features:**

- **System Setup:**

  - Update System and Configure Repositories

- **User Configuration:**

  - Configure Root User
    - Tags: lxc
  - Configure Custom User

- **Install Software:**

  - Install Development Packages
    - Tags: lxc
  - Install Tweaks
  - Install Docker
    - Tags: docker
  - Install Utilities

- **Perform Tweaking:**
  - Configure Hostname
  - Configure Logging
  - Configure Microcode
  - Configure OpenSSH
  - Install Qemu Guest Agent

## Requirements:

None.

## Role Variables

Available variables are listed below, along with default values:

**Pre-Configuration:**

- `set_hostname`: Boolean, defaults to `false`. Set to `true` to configure a
  custom hostname.
- `custom_hostname`: String, a template for the hostname using
  `${{ custom_user }}`. Example: `{{ custom_user }}-server`

**SSH:**

- `ssh_port`: Integer, defaults to `22`. Set the desired SSH port.

**Users Config:**

- `setup_user`: Boolean, defaults to `false`. Set to `true` to configure a custom user.
- `custom_user`: String, defaults to the value found in environment variable
  `CUSTOM_USER`. Fallback is `"jokerwrld"`. Username for the custom user
  account.
- `temp_dir`: String, defaults to `"tmp"`. Temporary directory used for certain
  tasks.

**Users Password (Optional - Security Consideration):**

**Encrypt Password:**

```python
python3 -c 'import crypt,getpass;pw=getpass.getpass();print(crypt.crypt(pw) if (pw==getpass.getpass("Confirm: ")) else exit())'
```

- `root_passwd`: String, empty by default. Set your Root password.
- `custom_user_passwd`: String, empty by default. Set your Custom user's
  password.

Consider using Ansible Vault or other secure credential management solutions.

**Example Playbook:**

```yaml
- hosts: all
  roles:
    - jokerwrld999.bootstrap

  vars:
    # Set your desired values here
    set_hostname: true
    custom_hostname: "{{ custom_user }}-server"
    setup_user: true
    custom_user: myuser
    ssh_port: 2234
```

## Dependencies

None.

## License

MIT / BSD

## Author Information

This role was created in 2024 by [Joker Wrld](https://docs.jokerwrld.win/).
