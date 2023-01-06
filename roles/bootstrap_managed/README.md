# bootstrap_managed

This role prepares the managed nodes for a given inventory. A user `ansible` will be created and password-less sudo configured.

## Requirements

None, other than sudo permissions for installing the `sshpass` package.

## Dependencies

None.

## Role variables

The role defines the following variables:

| Variable name   | Default value | Description                                                |
| --------------- | ------------- | ---------------------------------------------------------- |
| automation_user | *ansible*     | The name for the Ansible service user created by the role. |

## Example Playbook

```yaml
- hosts: localhost
  connection: local
  roles:
    - timgrt.bootstrap.bootstrap_controller
```

## Tags

The following tags are available:

* sshpass
* known_hosts

## License

MIT

## Author Information

This role was created in 2023 by Tim Grützmacher