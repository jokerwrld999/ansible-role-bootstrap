## Ansible Role: Bootstrap

This role provides basic pre-configuration tasks for your servers.

**Features:**

- **User Configuration:**
  - Configure Root User
  - Configure Custom User
    - Tags: development, docker
- **Install Software:**
  - Configure Repositories
    - Tags: development, docker, lxc
  - Configure Development Packages
    - Tags: development, docker, lxc
  - Configure Tweaks
  - Configure Docker
    - Tags: development, docker
  - Configure Utilities
- **Perform Remaining Tasks:**
  - Configure Hostname
    - Condition: When set_hostname is defined and set to true
  - Configure Logging
  - Configure Microcode
    - Tags: microcode
  - Configure OpenSSH
    - Tags: ssh
  - Install Qemu Guest Agent

**Requirements:**

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

- `custom_user`: String, defaults to the value found in environment variable
  `CUSTOM_USER`. Fallback is `"jokerwrld"`. Username for the custom user
  account.
- `temp_dir`: String, defaults to `"tmp"`. Temporary directory used for certain
  tasks.

**Alerts (Optional):**

- `telegram_chat_id`: String, empty by default. Set your Telegram chat ID if
  using Telegram alerts.
- `telegram_token`: String, empty by default. Set your Telegram bot token if
  using Telegram alerts.

Consider using Ansible Vault or other secure credential management solutions.

**Users Password (Optional - Security Consideration):**

# Encrypt Password

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
    custom_user: myuser
    ssh_port: 2234
    # Uncomment and set values for Telegram alerts (optional)
    # telegram_chat_id: "your_chat_id"
    # telegram_token: "your_bot_token"
```

## Dependencies

None.

## License

MIT

## Author Information

This role template was created based on the provided variables. Feel free to
customize it further based on your specific needs.
