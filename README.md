# Ansible Role: sysctl

![Test and release.](https://github.com/chzerv/ansible-role-sysctl/workflows/Test%20and%20release./badge.svg?branch=master)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Ansible Role](https://img.shields.io/ansible/role/50256?color=dodgerblue)](https://galaxy.ansible.com/chzerv/sysctl)

> **NOTE**: Testing for this role is mostly done using Vagrant VMs locally. The CI is using [molecule](https://molecule.readthedocs.io/en/latest/)
> but the role will not apply any sysctl configuration if the target is a container, since it will most likely fail (even on privileged containers).

This role configures `sysctl` on a Linux system.

## Requirements

None

## Role Variables

```yaml
sysctl_set: true
```

> If set to true, the token value will be verified before it's set.

```yaml
sysctl_reload: true
```

> If set to true, sysctl will be reloaded (using sysctl -p) after the sysctl_file is updated.

```yaml
sysctl_file: "/etc/sysctl.d/99-sysctl.conf"
```

> The absolute path to the file in which the configuration will be saved.

```yaml
sysctl_entries: []
# sysctl_entries:
#   - name: net.ipv4.ip_forward
#     value: 1
#     state: present
#     sysctl_set: "{{ sysctl_set }}"
#     reload: "{{ sysctl_reload }}"
#     sysctl_file: "{{ sysctl_file }}"
```

> The token and the value to apply to this token. `name` and `value` are **required**, while the rest can be either configured globally (as shown above), or per entry. `state` is set to `present` by default, but can be changed to `absent` if you want to unset the token.
>
> **Note** that multiple entries can be specified at once, like so:
>
> ```yaml
> sysctl_entries:
>   - name: net.ipv4.ip_forward
>     value: 1
>   - name: kernel.kptr_restrict
>     value: 1
>     state: absent
>     sysctl_set: false
>     reload: true
> ```

## Dependencies

None

## Example Playbook

```yaml
- hosts: server
  vars:
    sysctl_entries:
      - name: net.ipv4.ip_forward
        value: 1
        state: present

      - name: kernel.kexec_load_disabled
        value: 1
        reload: true
        sysctl_set: true
        state: absent

  roles:
    - { role: chzerv.sysctl }
```

## License

MIT / BSD

## Author Information

Xristos Zervakis
