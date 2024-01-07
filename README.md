RnodeC.beranode
=========

This role will deploy a berachain node.

***WIP***

Requirements
------------

Linux x86_64 host

Role Variables
--------------

`beranode_role` group_var defined in the inventory determines the type of node this will be.  This must be full, rpc, validator, archive, or sentry.  defaults to full.

For a full, rpc, or archive node, no further variables are required.

If this is a validator, you also must set these vars (obviously not a highly secure way of handling sensitive keys):
```bash
beranode_moniker: # your validator's name
validator_address: #
validator_pub_key: # pub key from priv_validator_key.json
validator_key: # private key from priv_validator_key.json
node_private_key: # private key from node_key.json
```

See [`defaults/main.yaml`](defaults/main.yaml) for full list of available variables and their default values.


Example Inventory
----------------
This role depends upon the beranode_role group var that must be set in the inventory file to determine which type of node to deploy.  For example, to deploy a single rpc node:

```bash
[rpc_nodes]
rpc1.example.com

[rpc_nodes:vars]
beranode_role=rpc
```

Or a validator node:
```bash
[validator_nodes]
validator1.example.com

[validator_nodes:vars]
beranode_role=validator
```

Example Playbook
----------------

```bash
---
- name: Deploy beranode
  hosts: all

  roles:
  - role: RnodeC.beranode
```

Author Information
------------------

RnodeC
rnodec.io
