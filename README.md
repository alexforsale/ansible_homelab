# Ansible Collection - alexforsale.homelab

This repo hosts the `alexforsale.homelab** Ansible Collection.

The collection include the roles I used in maintaining my homelab machines.

## Installation and Usage

``` shell
ansible-galaxy collection install alexforsale.homelab
```

You can also include in in a `requirements.yml` file and install it via `ansible-galaxy collection install -r requirements.yml` using the format:

``` yaml
collections:
  - name: alexforsale.homelab
```

## Included content
### Roles
| Name | Description
| --- | ---
| [kvm_provision](./roles/kvm_provision/README.md) | Provision Libvirt virtual machines
