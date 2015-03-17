# Autologic Users

Manage system groups and users using nothing but easy to read dictionaries. Using the more advanced ["Department Pattern"](#) you can also control access based on a user's role within your organisation. Should you hire a consultant or contractor who only needs limited access, the Department Pattern offers you a flexible way of adding individual users to individual systems.

## Version

2.1.1

## Role Variables

Here is a summary of the available variables, as defined in ```defaults/main.yml```:

```yaml
---
autologic_manage_groups: true
autologic_manage_users: true
autologic_manage_sshkeys: true

autologic_system_groups: []
autologic_system_users: []

autologic_department_pattern: false
autologic_department_access: []
autologic_user_access: []
```

Here is example groups list:

```yaml
---
autologic_system_groups:
  'Human Readable':
    groupname: 'superusers'
    state: 'present'
    # gid: ''
    # system: false

  'Development':
    groupname: 'development'
    state: 'present'
    gid: 5001
```

And an example variable for defining users:

```yaml
---
autologic_system_users:
  'Michael Crilly':
    username: 'mcrilly'
    state: 'present'
    sshkeys:
      - 'ssh-rsa AAAA...'
    department: 'systems'
    # groups:
    #  - 'superusers'
    # uid: ''
    # home: '/home/mcrilly'
    # group: 'mcrilly'
    # system: false
    # remove: false
    # force: false
```

If you decide to use the "Department Pattern", then you can make use of two additional variables. As an example:

```yaml
---
# group_vars/example.yml
autologic_department_access:
    - systems
```

Or for specific user access, primarily used for individual host access control:

```yaml
---
# host_vars/example.yml
autologic_user_access:
    - mcrilly
```

The top level key for each user, such as 'Michael Crilly' in the above example, is used for the 'comment=' field on the user module.

*At present, with regards to users, only the basic operations are supported from within the role. That is, things like 'expired=', ssh related values (except for the public key), etc, are omitted until a need presents its self for a more complex role.*

## Basic User Management (```autologic_department_pattern: false```)
If your needs are simple, then the default operation of this role is take all users from the ```autologic_system_user``` hash and add or remove them accordingly. This can be powerful enough for most people with a simple, flat user structure.

## Complex User Management (```autologic_department_pattern: true```)
The **department:** value can be used to define what groups and or hosts this user should belong to. The idea is to remove the need to do hash merging, which gets very messy, very quickly. To avoid using merges, we've created two lists you can use for access control: ```autologic_department_access``` and ```autologic_user_access```. You will also need to turn on this method using the ```autologic_department_pattern``` variable.

**Please note that even with the use of ```autologic_user_access```, a ```department:``` value still needs to be defined.**

See the [Autologic Example Users](https://github.com/AutoLogicTechnology/example-users) repository for a working example on how-to use this pattern to easily manage users.

## License

MIT

## Author Information

- Michael Crilly
- Autologic Technology Ltd
- http://www.mcrilly.me/
