RnodeC.beranode
=========

This role will deploy a berachain node.  

**This is just a placeholder, will not work until we have a testnet**

Requirements
------------

Linux x86_64 host

Role Variables
--------------

No variables are required for this role.  See `defaults/main.yaml` for full list of available variables and their default values.  Here are a few that you might likely want to apply:

```
beranode_moniker: <your nodes name>
```
 

Example Playbook
----------------

```bash
---
- name: Deploy beranode
  hosts: localhost

  roles:
  - role: RnodeC.beranode
```



Author Information
------------------

RnodeC
rnodec.io