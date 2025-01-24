Users
=========

A role for securing Linux users.

Requirements
------------

- Ansible >= 2.18
- Python 3.x

Role Variables
--------------

Display options:

| Variable                        | Type | Default | Description                            |
| ------------------------------- | ---- | ------- | -------------------------------------- |
| `display.display_users`         | boot | `false` | Display all users found in the system? |
| `display.display_regular_users` | boot | `true`  | Display found regular users?           |
| `display.display_system_users`  | boot | `false` | Display found system users?            |

Users:

| Variable           | Type | Default             | Description                                                      |
| ------------------ | ---- | ------------------- | ---------------------------------------------------------------- |
| `regular_users`    | list | `[alice,jane,john]` | Regular users to configure                                       |
| `ignore_users`     | list | `[ansible]`         | Users who should not be touched                                  |
| `privileged_users` | list | `[jane]`            | Users who should be able to escalate privileges with `sudo`      |
| `guest_users`      | list | `[alice]`           | Users who can't change their passwords                           |
| `default_shell`    | str  | `/bin/bash`         | Default shell to assign to all users                             |
| `privilege_group`  | str  | `wheel`             | Members of this group can elevate privileges with `sudo`         |
| `lock_root`        | bool | `true`              | Lock root password after other administrators have been created? |

PAM:

| Variable                 | Type | Default                      | Description |
| ------------------------ | ---- | ---------------------------- | ----------- |
| `PAM_pwquality_package`  | str  | `libpam-pwquality`           |             |
| `PAM_pwquality_filepath` | str  | `/etc/pam.d/common-password` |             |


Passwords:

- Password quality

| Variable                         | Type | Default | Description                     |
| -------------------------------- | ---- | ------- | ------------------------------- |
| `password_quality`               | bool | `true`  | Enforce password quality rules? |
| `password_complexity.min_length` | int  | `10`    | Minimum password length         |


- Brute-force protection

| Variable                             | Type | Default                  | Description                                                                                                                   |
| ------------------------------------ | ---- | ------------------------ | ----------------------------------------------------------------------------------------------------------------------------- |
| `bruteforce_protection`              | bool | `true`                   | Configure lockout brute-force password protection?                                                                            |
| `password_lockout.deny`              | int  | `5`                      | The number of failed password attempts before password lock                                                                   |
| `password_lockout.unlock_time`       | int  | `900`                    | Password lock time (in seconds)                                                                                               |
| `password_lockout.fail_intervail`    | int  | `900`                    | The time during which failed logins can cause a lockout                                                                       |
| `password_lockout.lockout_directory` | str  | `/var/lib/faillock`      | The directory where to store password locks<br>`/var/lib/faillock` persists across reboots, while `/var/run/faillock` doesn't |
| `password_lockout.PAM_config`        | str  | `/etc/pam.d/common-auth` | PAM configuration file that enforces `faillock` rules                                                                         |

- Password expiration

| Variable                                          | Type | Default | Description                                                                                               |
| ------------------------------------------------- | ---- | ------- | --------------------------------------------------------------------------------------------------------- |
| `password_expiration`                             | bool | `true`  | Configure password expiration?                                                                            |
| `password_expiration_data.expire_min`             | int  | `5`     | Minimum number of days between password change.                                                           |
| `password_expiration_data.expire_max`             | int  | `180`   | Maximum number of days between password change (password expiration time).                                |
| `password_expiration_data.expire_warn`            | int  | `5`     | Days before password expiration when to warn users                                                        |
| `password_expiration_data.expire_account_disable` | int  | `5`     | Days after which the account with expired password is disabled (can be enabled by an administrator later) |


Example Playbook
----------------

    - hosts: all
      roles:
         - { role: users }

License
-------

BSD

Author Information
------------------

0xtr1gger
