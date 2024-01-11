RnodeC.berachain
=========
This role will deploy a berachain node.

***WIP***

Requirements
------------
Linux x86_64 host

Role Variables
--------------
`berachain_role` group_var defined in the inventory determines the type of node this will be.  This must be full, rpc, validator, archive, or sentry.  defaults to full.

For a full, rpc, or archive node, no further variables are required.  Here are some you might want to set, however:

```bash
berachain_moniker: # your validator's name
berachain_chain_id: # i.e. mainnet or  artio-80085
```
As for the node's keys (`node_key.json` and `priv_validator_key.json`).. this section is really only relevant if this is a validator.  And first of all, if it's a brand new validator you would probably just leave all these variables blank and allow them to be created for you.

> `node_key.json` is used for general p2p communications, by all node types (not just validators), and generally can be regenerated at any time.  `priv_validator_key.json` is much more important - this is the validator's signing key.  If you're a validator, you should know the rules - ***don't ever run two validators at the same time with the same key***.  And never expose this key anywhere.

If this happens to be a migration or restore of some existing validator, you may either provide the key mnemonic:
```bash
berachain_keys_mnemonic: # mnemonic that was derived from a 'berad keys add' command
```

Or alternatively, you may provide the path to local `node_key.json` and/or `priv_validator_key.json` files as ansible vars.  Otherwise, these will be created for you:
```bash
berachain_node_key_file: # path to local node_key.json
berachain_priv_validator_key_file: # path to local priv_validator_key.json
```

You may use `ansible-vault` to encrypt these files.  Here is an example of how to encrypt a key file with ansible vault (obviously still not a *highly* secure way of handling these sensitive keys...):
```bash
ansible-vault encrypt priv_validator_key.json
# create a password when prompted
```
...and then pass your vault password to the `ansible-playbook` command with `--ask-vault-pass` or `--vault-password-file`

Also, if this happens to be a migration of an existing validator to a new machine, you might want to import `priv_validator_state.json` as well, to help prevent any double signing:
```bash
berachain_priv_validator_state_file: # path to local priv_validator_state.json
```

See [`defaults/main.yaml`](defaults/main.yaml) for full list of available variables and their default values.

Example Inventory
----------------
This role depends upon the berachain_role group var that must be set in the inventory file to determine which type of node to deploy.

For example, to deploy a single rpc node:
```
[rpc_nodes]
rpc1.example.com

[rpc_nodes:vars]
berachain_role=rpc
```

Or a validator node:
```
[validator_nodes]
validator1.example.com

[validator_nodes:vars]
berachain_role=validator
```

Or a full sentry architecture:
```
[validator_nodes]
validator1.example.com

[sentry_nodes]
sentry1.example.com
sentry2.example.com
sentry3.example.com

[validator_nodes:vars]
berachain_role=validator
berachain_persistent_peers: 'sentry1.example.com,sentry2.example.com,sentry3.example.com'
berachain_pex_enabled: false

[sentry_nodes:vars]
berachain_role=sentry
berachain_persistent_peers: 'validator1.example.com'
berachain_private_peer_ids: 'validator1.example.com'
berachain_pex_enabled: true
```

Example Playbook
----------------
```bash
---
- name: Deploy berachain node
  hosts: all
  gather_facts: true

  vars:
    berachain_moniker: # your validator's name
    berachain_chain_id: # i.e. mainnet or  artio-80085

  roles:
  - role: RnodeC.berachain
```

Author Information
------------------
[RnodeC](https://rnodec.io)
