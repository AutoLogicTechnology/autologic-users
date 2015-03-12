# Role Name

A simple role for managing users and groups with Ansible.

## Role Variables

We have a variable for defining groups:

```yaml
autologic_groups:
  'Human Readable':
    groupname: 'superusers'
    state: 'present' # default is 'present'
    # gid: ''
    # system: false
```

And a variable for defining users:

```yaml
autologic_users:
  'Michael Crilly':
    username: 'mcrilly'
    state: 'present' # default is 'present'
    sshkeys:
      - 'ssh-rsa AAAA...'
    # uid: ''
    # system: false
    # group: 'mcrilly'
    # groups:
    #  - 'superusers'
    # home: '/home/mcrilly'
```

The top level key for each user, such as 'Michael Crilly' in the above example, is used for the 'comment=' field on the user module.

At present, with regards to users, only the basic operations are supported from within the role. That is, things like 'expired=', ssh related values (except for the public key), etc, are omitted until a need presents its self for a more complex role.

## Example Playbook

```yaml
---
- hosts: all
  sudo: true
  vars:
    autologic_users:
      'Michael Crilly':
        username: 'mcrilly'
        sshkeys:
          - 'ssh-rsa AAAA...'
      'John Doe':
        username: 'johnd'
        state: 'absent'
  roles:
    - autologic_users
```

## License

MIT

## Author Information

- Michael Crilly
- Autologic Technology Ltd
- http://www.mcrilly.me/
