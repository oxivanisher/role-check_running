check_running
=============
[![Ansible Lint](https://github.com/oxivanisher/role-check_running/actions/workflows/ansible-lint.yml/badge.svg)](https://github.com/oxivanisher/role-check_running/actions/workflows/ansible-lint.yml)

This role checks the `systemd is-system-running` output. It will log the current state and `fail` if the state returned is not in the `check_running_accepted_states` list. If this list is empty, the role will automatically accept the `running` state and if the state is `initializing` or `starting` it will wait up to `check_running_startup_wait_retries` times 5 seconds to do the final state verification.

Please be aware that this role has `allow_duplicates` turned on so that it is allowed to run multiple times per play.

Requirements
------------

Currently none.

Role Variables
--------------

| Name                               | Comment                                 | Default value |
|------------------------------------|-----------------------------------------|---------------|
| check_running_accepted_states      | Which states are additionally accepted? | `[]`          |
| check_running_startup_wait_retries | How many 5 second retries until the role fails if the state is `initializing` or `starting`. | `60` |

**Note:** The state `running` will automatically be accepted, if `check_running_accepted_states` is empty.

The following descriptions is taken from `man systemctl`:
```plaintext
┌─────────────┬─────────────────────────────────┬───────────┐
│Name         │ Description                     │ Exit Code │
├─────────────┼─────────────────────────────────┼───────────┤
│initializing │ Early bootup, before            │ > 0       │
│             │ basic.target is reached or the  │           │
│             │ maintenance state entered.      │           │
├─────────────┼─────────────────────────────────┼───────────┤
│starting     │ Late bootup, before the job     │ > 0       │
│             │ queue becomes idle for the      │           │
│             │ first time, or one of the       │           │
│             │ rescue targets are reached.     │           │
├─────────────┼─────────────────────────────────┼───────────┤
│running      │ The system is fully             │ 0         │
│             │ operational.                    │           │
├─────────────┼─────────────────────────────────┼───────────┤
│degraded     │ The system is operational but   │ > 0       │
│             │ one or more units failed.       │           │
├─────────────┼─────────────────────────────────┼───────────┤
│maintenance  │ The rescue or emergency target  │ > 0       │
│             │ is active.                      │           │
├─────────────┼─────────────────────────────────┼───────────┤
│stopping     │ The manager is shutting down.   │ > 0       │
├─────────────┼─────────────────────────────────┼───────────┤
│offline      │ The manager is not running.     │ > 0       │
│             │ Specifically, this is the       │           │
│             │ operational state if an         │           │
│             │ incompatible program is running │           │
│             │ as system manager (PID 1).      │           │
├─────────────┼─────────────────────────────────┼───────────┤
│unknown      │ The operational state could not │ > 0       │
│             │ be determined, due to lack of   │           │
│             │ resources or another error      │           │
│             │ cause.                          │           │
└─────────────┴─────────────────────────────────┴───────────┘
```

Tags
----

None.

Dependencies
------------

Currently none.

Example Playbook
----------------

```yaml
- hosts: servers
  roles:
    - role: oxivanisher.linux_base.check_running                  # Check if the system is running as expected
```

License
-------

BSD

Author Information
------------------

This role is part of the [oxivanisher.linux_base](https://galaxy.ansible.com/ui/repo/published/oxivanisher/linux_base/) collection, and the source for that is located on [github](https://github.com/oxivanisher/collection-linux_base).
