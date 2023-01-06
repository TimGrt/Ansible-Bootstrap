# bootstrap_controller

This role prepares the Ansible control node, it installs the `sshpass` package for bootstrapping managed nodes and adds SSH host keys to `known_hosts`.

## Requirements

None, other than sudo permissions for installing the `sshpass` package.

## Dependencies

None.

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