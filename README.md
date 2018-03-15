# 

## Requirements

Create `group_vars/all/accounts.yml` with the following contents (adapt to your
environment):

```yaml
---
ansible_password: packer
ansible_user: packer
vcenter_password: VMw@re1!
vcenter_username: administrator@vsphere.local
```

> NOTE: The above file is excluded from version control to ensure that account
> info is not leaked. You can also use `ansible-vault` if needed but you will
> need to remove `accounts.yml` from `.gitignore`.
