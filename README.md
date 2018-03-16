<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [ansible-consul-cluster](#ansible-consul-cluster)
  - [Assumptions](#assumptions)
  - [Requirements](#requirements)
  - [Usage](#usage)
    - [Spinning Up VMs](#spinning-up-vms)
      - [Updating Variables](#updating-variables)
      - [Spinning Up and Provisioning](#spinning-up-and-provisioning)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# ansible-consul-cluster

## Assumptions

The following list of assumptions are made but may be adjusted as neccessary.

1.  Ubuntu (preferred) VM template available in vCenter.
2.  DHCP is available to assign IP addresses to VMs when they spin up.

## Requirements

Create `group_vars/all/accounts.yml` with the following contents (adapt to your
environment):

```yaml
---
# Defines Linux SSH password
ansible_password: packer

# Defines Linux SSH user
ansible_user: packer

# Defines vCenter password
vcenter_password: VMw@re1!

# Defines vCenter username
vcenter_username: administrator@vsphere.local
```

> NOTE: The above file is excluded from version control to ensure that account
> info is not leaked. You can also use `ansible-vault` if needed but you will
> need to remove `accounts.yml` from `.gitignore`.

## Usage

### Spinning Up VMs

#### Updating Variables

You will first need to spin up 3 VMs for the Consul cluster.

Modify the following to meet your environment requirements:

1.  [group_vars/all/vms.yml](group_vars/all/vms.yml)
2.  [group_vars/all/dns.yml](group_vars/all/dns.yml)
3.  [group_vars/all/vcenter.yml](group_vars/all/vcenter.yml)

You also will need to adjust the following for Consul.

`group_vars/consul_cluster/consul.yml`:

```yaml
# Generated using 'uuidgen'
# make sure to generate a new token and replace this one
consul_acl_master_token: 7A993C85-1EA6-412D-85DB-DA14EFCD03AB

# Generate using 'consul keygen'
# make sure to generate a new key and replace this
# also update key if you changed the it in your cluster via 'consul keyring',
# otherwise the role may deploy an outdated key to additional nodes which then
# can't join the cluster
consul_encryption_key: OJVXXkKdwifyqc9BrDe1VQ==
```

#### Spinning Up and Provisioning

To spin up the VMs:

```bash
ansible-playbook provisioned_vms.yml
```

After the VMs have been provisioned, `hosts.inv` will be populated to use as
Ansible inventory.

To provision the VMs:

```bash
ansible-playbook -i hosts.inv playbook.yml
```
