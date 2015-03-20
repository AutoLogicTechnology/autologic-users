# Autologic Users

Manage UNIX system users and groups from a single role.

Using a single hash, this role will allow you to define what users exist on your systems. Using the more advanced "Department Pattern" allows you to introduce a simple list for each of your groups/hosts: ```autologic_department_access```.

## Version

3.1.0

## Role Variables

Here is a summary of the available variables, as defined in ```defaults/main.yml```:

```yaml
---
autologic_manage_users: true
autologic_manage_sshkeys: true

autologic_system_users: []

autologic_department_pattern: false
autologic_department_access: []
autologic_user_access: []

autologic_super_departments_nopasswd: true
autologic_super_departments: []
autologic_super_users_nopasswd: true
autologic_super_users: []
```

Here is an example variable for defining users:

```yaml
---
autologic_system_users:
  'Michael Crilly':
    username: 'mcrilly'
    state: 'present'
    departments:
      - 'systems'
    # comment: ''
    # uid: ''
    # home: '/home/mcrilly'
    # group: 'mcrilly'
    # system: false
    # remove: false
    # force: false
```

You'll note that there is no 'groups' hash or list for defining groups. This is because the ```departments:``` list is used to define what groups the user exists in. It's also used to decide what groups need to be created on what hosts. This means you don't need to worry about a group existing on a system because the role will ensure it exists before adding the user.

If you decide to use the "Department Pattern", then you can make use of two additional variables and open a large amount of flexibility to your estate. As an example:

```yaml
---
# group_vars/webservers.yml
autologic_department_access:
    - systems
    - developers
```

Or for specific user access, primarily used for individual host access control:

```yaml
---
# host_vars/web-01.yml
autologic_user_access:
    - mcrilly
    - johndoe
```

## Basic User Management (autologic_department_pattern: false)
If your needs are simple, then the default operation of this role is take all users from the ```autologic_system_user``` hash and add or remove them accordingly. This can be powerful enough for most people with a simple, flat user structure.

The ```departments:``` list will be used to create whatever groups are needed on each system for the user in question.

## Complex User Management (autologic_department_pattern: true)
The **departments:** value can be used to define what groups and or hosts this user should belong to. The idea is to remove the need to do hash merging, which gets very messy, very quickly. To avoid using merges, we've created two lists you can use for access control: ```autologic_department_access``` and ```autologic_user_access```. You will also need to turn on this method using the ```autologic_department_pattern``` variable.

If you have a user that doesn't belong to a particular department, such as an external company or contractor, then you can just make ```departments: []``` an empty list, like this.

See the [Autologic Example Users](https://github.com/AutoLogicTechnology/example-users) repository for a working example on how-to use this pattern to easily manage users.

## SSH Keys
Each user's SSH is managed from a dedicted file. This makes the ```autologic_system_users:``` hash much easier to manage and read.

When utilised, the role will use SSH keys found in ```files/sshkeys/{{autologic_system_users.username}}``` for managing your user's ```authorized_keys``` file. That is, in your Ansible root create a folder called ```files/sshkeys``` and place a new file in there with the same name as your username(s). This will be that user's dedicated SSH key file, enabling you to easily manage multiple keys and SSH options from a single point, without creating overly complicated variable files.

See the [Autologic Example Users](https://github.com/AutoLogicTechnology/example-users) repository for a working example on how-to manage user SSH keys.

## Sudo Access
If you want to use sudo to control priviledged access, then there are some additional variables you can use:

```yaml
autologic_super_departments: []
autologic_super_users: []
```

If you list departments under the 'super_departments' list, then they will be added to a file which permits them passwordless root access (via ```sudo -i```, not via the SSH login prompt.)

The file departments and users are added to is: ```/etc/sudoers.d/autologic-sudoers```.

If you list individual users under the 'super_users' group, then you will get the same effect on a per-user basis.

**Please note**, that by default, these departments and users get passwordless root access. If you do not want this to be the case, forcing people to supply a password to the sudo command, then turn off ```autologic_super_departments_nopasswd: true``` and or ```autologic_super_users_nopasswd: true```.

## Kanban

The project's Kanban board can be found [here on Trello](https://trello.com/b/VE5PMaUc). Considering reviewing this board before raising an issue or feature request as your needs might already be in the works.

## License

MIT

## Author Information

- Michael Crilly
- Autologic Technology Ltd
- http://www.mcrilly.me/
