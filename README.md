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
    # department: ''
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

There is a special variable called 'autologic_department_pattern' which can be turned on (true) or off (false). If this flag is turned on, a variable can be used in group and host variables (group_vars/, host_vars/, etc) to determine if a given user - who belongs to a 'department:', as above - should be added to the hosts in a given group or a specific host directly.

Here's an example:

```yaml
---
# site.yml
- hosts: all
  sudo: true
  roles:
    - { role: autologic_users, autologic_department_pattern: true }
```

Above we are adding the autologic_users role to our site, but parameterising it and configuring it with 'autologic_department_patterm' turned on. Now we'll define our users, followed by group and host level declarations to refine what users can access what systems.

```yaml
---
# group_vars/all.yml
autologic_users:
  'Michael Crilly':
    username: 'mcrilly'
    sshkeys:
      - 'ssh-rsa AAAA...'
    department: 'systems'

  'John Smith':
    username: 'johns'
    sshkeys:
      - 'ssh-rsa AAAA...'
    department: 'developers'

  'Michael Jackson':
    username: 'mjackson'
    sshkeys:
      - 'ssh-rsa AAAA...'
    department: 'ui'
```

```yaml
---
# group_vars/webservers.yml
autologic_departments:
  - 'systems'
  - 'developers'
```

```yaml
---
# host_vars/web-03.yml
autologic_departments:
  - 'ui'
```

This yields the following results:

...

## Example Playbook

This example does not use the Department Pattern as seen above.

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
