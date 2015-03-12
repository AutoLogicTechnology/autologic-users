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
    department: 'systems'
    # uid: ''
    # system: false
    # group: 'mcrilly'
    # groups:
    #  - 'superusers'
    # home: '/home/mcrilly'
```

The top level key for each user, such as 'Michael Crilly' in the above example, is used for the 'comment=' field on the user module.

At present, with regards to users, only the basic operations are supported from within the role. That is, things like 'expired=', ssh related values (except for the public key), etc, are omitted until a need presents its self for a more complex role.

The **job:** value can be used to define what groups and or hosts this user should belong to. See the [Autologic Example Users](https://github.com/AutoLogicTechnology/example-users) repository for a working example on how-to use this field to easily manage users. The idea is to remove the need to do a hash merging, which gets very messy, very quickly.

## Department Pattern

There is a special variable called 'autologic_department_pattern' which can be turned on (true) or off (false). If this flag is turned on, a second variable can be used in group and host variables (group_vars/, host_vars/, etc) to determine if a given user - who belongs to a 'department:', as above - should be added to the hosts in a given group or a specific host directly.

This is somewhat complex and as such, an entirely separate repository has been setup [over here](https://github.com/AutoLogicTechnology/example-users) for your review.

Overall, the idea is to avoid merged hashes (hash_behaviour: 'merge'). When using merged hashes, it becomes very tempting to follow design patterns and behaviours which get messy, confusing, and hard to debug very quickly. The "Department Pattern" attempts to resolve this.

## License

MIT

## Author Information

- Michael Crilly
- Autologic Technology Ltd
- http://www.mcrilly.me/
