# User and Group Creation

Linux systems manage users and groups to separate access, permissions, and responsibilities. A user is an account that can log in and run commands. A group is a collection of users with shared permissions.

## Concept & Internals

Linux stores user information in `/etc/passwd` and group information in `/etc/group`.

- `/etc/passwd` stores basic user account data
- `/etc/group` stores group definitions and member assignments
- `/etc/shadow` stores encrypted passwords (normally restricted access)

When a user logs in, the system checks the account information and assigns the correct permissions.

## Core Command Reference

| Command | Purpose | Example |
| --- | --- | --- |
| `sudo adduser <username>` | Create a new user | `sudo adduser janindu` |
| `sudo deluser <username>` | Delete a user | `sudo deluser janindu` |
| `sudo addgroup <groupname>` | Create a new group | `sudo addgroup devteam` |
| `su - <username>` | Log in as another user | `su - janindu` |
| `sudo usermod -aG <group> <user>` | Add a user to a secondary group | `sudo usermod -aG sudo janindu` |
| `sudo usermod -g <group> <user>` | Change the user's primary group | `sudo usermod -g devteam janindu` |

## 1. Create a user

```bash
sudo adduser janindu
```

Example output:

```text
Adding user `janindu' ...
Adding new group `janindu' (1001) ...
Adding new user `janindu' (1001) with group `janindu' ...
Creating home directory `/home/janindu` ...
Copying files from `/etc/skel` ...
New password: 
Retype new password: 
passwd: password updated successfully
```

- This creates a new user account and home directory.
- It also creates a matching default group for that user.

## 2. Delete a user

```bash
sudo deluser janindu
```

Example output:

```text
Removing user `janindu' ...
Warning: group `janindu' has no more members.
Done.
```

- This removes the user account.
- You may also need to remove the home folder manually if desired.

## 3. Create a group

```bash
sudo addgroup devteam
```

Example output:

```text
Adding group `devteam' (1002) ...
Done.
```

- This creates a new group that users can be assigned to.

## 4. User and group information files

### `/etc/passwd`

This file contains user account information.

Example:

```bash
cat /etc/passwd | head
```

Example output:

```text
root:x:0:0:root:/root:/bin/bash
janindu:x:1001:1001:Janindu:/home/janindu:/bin/bash
```

- Each line contains user information in this format:
  `username:password:UID:GID:comment:home:shell`

### `/etc/group`

This file stores group definitions and members.

Example:

```bash
cat /etc/group | head
```

Example output:

```text
root:x:0:
admin:x:4:janindu
sudo:x:27:janindu
devteam:x:1002:
```

- Group lines have the format:
  `groupname:password:GID:members`

## 5. Log in as another user

```bash
su - janindu
```

Example output:

```text
Password:
[janindu@rhel ~]$
```

- `su` switches users.
- `-` loads a login shell environment for that user.
- This is useful when you need to test or work as a different account.

## 6. Add user to a group

### Add a user to a secondary group

```bash
sudo usermod -aG sudo janindu
```

This command usually produces no output on success.

- `-a` = append
- `-G` = secondary group list
- This adds the user to the sudo group without removing other groups.
- Without `-a`, the command replaces the user’s current supplementary group list instead of appending to it.

### Add multiple groups

```bash
sudo usermod -aG sudo,devteam janindu
```

- Multiple groups can be passed as a comma-separated list.

### Change the primary group

```bash
sudo usermod -g devteam janindu
```

- `-g` changes the primary group.
- This sets the user's main group to `devteam`.

### Without `-a`

```bash
sudo usermod -G devteam janindu
```

- Without `-a`, the user may lose their existing supplementary groups and only keep the new group set.

## 7. Edit group membership manually

You can also update group membership by editing `/etc/group`.

Example:

```text
devteam:x:1002:janindu,alice
```

This means both `janindu` and `alice` are members of `devteam`.

If you want to do it manually without `usermod`, edit the group line and append the username:

```bash
sudo nano /etc/group
```

Then change:

```text
devteam:x:1002:
```

to:

```text
devteam:x:1002:janindu
```

You can also add multiple users:

```text
devteam:x:1002:janindu,alice,bob
```

> This method is possible, but `usermod` is safer and easier to manage.

### Manual user-to-group mapping in `/etc/passwd`

The system also uses the `/etc/passwd` entry for the user's primary group. You can manually update the GID in the user's entry to change primary group if needed:

```text
janindu:x:1001:1002:Janindu:/home/janindu:/bin/bash
```

Here, `1002` is the primary group ID for the user. This means the user is primarily assigned to the group with GID `1002`.

> This is an advanced manual approach and should be used carefully. `usermod` is the safer method.

## 8. Give sudo privileges

If a user needs administrative rights, add them to the correct admin group.

### Ubuntu/Debian: `sudo` group

```bash
sudo usermod -aG sudo janindu
```

This adds the user to the `sudo` group, which is commonly used on Debian/Ubuntu systems.

### RHEL/CentOS/Fedora: `wheel` group

```bash
sudo usermod -aG wheel janindu
```

On RHEL-based systems, the `wheel` group is typically used to grant sudo access.

Now the user can run commands with `sudo`.

## 8.1. Switching from a normal user to sudo/admin access

After creating a new user and adding them to the correct admin group, the normal user must log out and log back in before the new group membership is active.

### Step-by-step example

```bash
sudo adduser devuser
sudo usermod -aG sudo devuser
```

Then log out and log back in as `devuser`.

After login, test sudo access:

```bash
whoami
sudo whoami
```

Example output:

```text
devuser
root
```

- `whoami` shows the current user (`devuser`)
- `sudo whoami` runs the command as root and prints `root`

### Use sudo as a normal user

```bash
sudo apt update
sudo ls /root
```

Example output:

```text
[sudo] password for devuser:
```

Then the command runs with root privileges.

### Open a root shell from a normal user

```bash
sudo -i
```

or

```bash
sudo su -
```

Example output:

```text
root@rhel:~#
```

- `sudo -i` = login as root with root environment
- `sudo su -` = switch to root shell using sudo

> The user is still acting as root, but the command was started through `sudo`.

### Switch from a normal user to another user

If you want to become another user after login:

```bash
su - janindu
```

Example output:

```text
Password:
[janindu@rhel ~]$
```

- `su -` = switch user and load their login shell
- This is different from `sudo`, which temporarily grants admin privileges.

### `/etc/sudoers` file

The `/etc/sudoers` file defines which users or groups can use `sudo`.

Always edit it with:

```bash
sudo visudo
```

This opens the sudoers file safely and checks syntax before saving.

Then add a line like this:

```text
janindu ALL=(ALL:ALL) ALL
```

This grants the user full sudo access.

You can also grant access by group:

```text
%wheel ALL=(ALL:ALL) ALL
```

- `%wheel` means “all members of the wheel group”
- This is common on RHEL-based Linux distributions

> Use `visudo` instead of editing `/etc/sudoers` directly. It validates the sudoers syntax and reduces the risk of breaking sudo access.

### Confirming sudo access

```bash
id janindu | grep sudo
```

Example output:

```text
uid=1001(janindu) gid=1001(janindu) groups=1001(janindu),27(sudo)
```

For RHEL-based systems:

```bash
id janindu | grep wheel
```

Example output:

```text
uid=1001(janindu) gid=1001(janindu) groups=1001(janindu),10(wheel)
```

Another useful pattern is:

```bash
cat /etc/group | grep sudo | head
```

Example output:

```text
sudo:x:27:janindu
```

Or on RHEL:

```bash
cat /etc/group | grep wheel | head
```

Example output:

```text
wheel:x:10:janindu
```

## 9. Example workflow

```bash
sudo adduser newuser
sudo addgroup devteam
sudo usermod -aG devteam newuser
sudo usermod -aG sudo newuser
```

This creates a user, adds them to a developer group, and grants sudo access.

## 10. Practical notes

- Primary group = main group assigned to the user
- Secondary group = extra permissions from additional groups
- `sudo` gives temporary administrative privileges
- Use `id <username>` to inspect the user's groups

Example:

```bash
id janindu
```

Example output:

```text
uid=1001(janindu) gid=1001(janindu) groups=1001(janindu),27(sudo),1002(devteam)
```

## 11. Summary

User and group management is essential for Linux access control.

- create users with `adduser`
- create groups with `addgroup`
- assign users with `usermod`
- grant admin access by adding a user to the `sudo` group
- inspect accounts via `/etc/passwd` and `/etc/group`

This gives you the foundation for managing identity and permissions in Linux systems.
