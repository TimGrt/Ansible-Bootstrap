# Ansible Collection - timgrt.bootstrap

[![](https://img.shields.io/ansible/collection/2159)](https://galaxy.ansible.com/timgrt/bootstrap)

This collection contains roles to bootstrap the Ansible control node and managed nodes for further automation.  

When interacting with a new managed node, you need to accept the SSH host key manually, the *bootstrap_controller* role automates this. The first interaction with a new managed node must be done with SSH user/password authentication, the necessary `sshpass` package will be installed on the control node.  

I like to have a dedicated service user on all my managed nodes, the role *bootstrap_managed* creates a user `ansible` (adjustable by variable) with password-less sudo configured. The SSH public key of the user running the playbook will be added to the *authorized_keys* of the *ansible* user. 
Additionally, the role also **updates all packages** to the latest state **(!)**. 

See *Usage* section for additional information.

## Getting Started

This collection contains the following ressources:

| Ressources                     | Comment                                                                        | Privilege |
|:------------------------------ |:------------------------------------------------------------------------------ |:---------:|
| **roles/bootstrap_controller** | Prepares Ansible control node, sshpass installation and SSH host key handling. |    true   |
| **playbooks/controller.yml**   | Playbook, referencing the role *bootstrap_controller*.                         |    true   |
| **roles/bootstrap_managed**    | Prepares Ansible managed nodes, service user creation and package upgrades.    |    true   |
| **playbooks/managed.yml**      | Playbook, referencing the role *bootstrap_managed*.                            |    true   |

### Prerequisites

* [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/index.html) >= 2.9
* sudo permissions

### Installing

```sh
ansible-galaxy collection install timgrt.bootstrap
```

## Usage

After installing the collection, you can use the provided playbooks to prepare the control node and all managed nodes.

### Bootstrap control node

Use the following playbook:

```bash
ansible-playbook timgrt.bootstrap.controller
```

The `controller` playbook will run locally (assuming that this is your Ansible control node), if you lack sufficient sudo permissions, the playbook will fail with an error message stating to run with `--ask-become-pass` to be able to install the necessary *sshpass* package.
If you provide an inventory file, the role checks if all hosts in it are present in the *known_hosts* file. In this example an inventory file `hosts` is provided.

```bash
ansible-playbook timgrt.bootstrap.controller -i hosts
```

If hosts are not already in *known_hosts*, the role will gather the SSH host keys and add them, manually accepting the keys won't be necessary anymore.

### Bootstrap managed nodes

Run the playbook `timgrt.bootstrap.managed`. Provide an available and privileged user (e.g. `root`), as well as parameters `--ask-pass` to provide the SSH password and `--ask-become-pass` in case a sudo password is necessary.

```bash
$ ansible-playbook timgrt.bootstrap.managed -i hosts --ask-pass --ask-become-pass -e ansible_user=timgrt
SSH password: 
BECOME password[defaults to SSH password]: 

PLAY [Bootstrap playbook, run it with '-e ansible_user=<available-privileged-user> --ask-pass --ask-become-pass' initially] **************************

TASK [Gathering Facts] *******************************************************************************************************************************

[...]

```

As the role added the SSH public key of the current user to the created *ansible* user on the managed nodes, you may add this variable defintion to your inventory (or in e.g. `group_vars/all.yml`) afterwards:

```ini
[all:vars]
ansible_user = ansible
```

## License

MIT / BSD

## Author Information

This role was created in 2023 by Tim Grützmacher