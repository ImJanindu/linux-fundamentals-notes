# Linux File Permissions

Linux file permissions control who can read, write, and execute files or directories. Every file and directory has an owner, a group, and permission bits for three categories:

- user / owner
- group
- others

## Concept & Internals

A file or directory has:

- a user owner
- a group owner
- 3 permission sets

Each set contains:

- `r` = read
- `w` = write
- `x` = execute

Example:

```bash
ls -ld oslab
or 
ll -d oslab
```

Example output:

```text
drwxrwxr-x 3 janindu janindu 4096 Jun 17 02:01 oslab/
```

This means:

- `d` = directory
- `rwx` for user = owner can read, write, and execute
- `rwx` for group = group members can read, write, and execute
- `r-x` for others = others can read and execute, but not write

### Permission value summary

- `r` = 4
- `w` = 2
- `x` = 1

So:

- `rwx` = 7
- `rw-` = 6
- `r-x` = 5
- `---` = 0

## Core Command Reference

| Command | Purpose | Example |
| --- | --- | --- |
| `ls -l` | Show detailed permissions | `ls -l file.txt` |
| `ls -ld` | Show directory permissions | `ls -ld /path/to/dir` |
| `chmod <value> <file/dir>` | Change permissions | `chmod 755 script.sh` |
| `chmod u+x file` | Add execute to owner | `chmod u+x script.sh` |
| `chmod g+w file` | Add write to group | `chmod g+w file.txt` |
| `chmod o-r file` | Remove read for others | `chmod o-r file.txt` |
| `chown user:group <file/dir>` | Change owner and group | `sudo chown janindu:devteam file.txt` |
| `chgrp <group> <file/dir>` | Change group ownership | `sudo chgrp devteam file.txt` |
| `getfacl <file/dir>` | View ACL permissions | `getfacl grapes/` |
| `setfacl -m u:<user>:<perms> <file/dir>` | Grant access to a specific user | `setfacl -m u:malshan:rw grapes` |
| `setfacl -x u:<user> <file/dir>` | Remove a specific user ACL | `setfacl -x u:malshan grapes` |

## 1. Viewing file and directory permissions

We can get the details of a file by using:

```bash
ls -l <filename>
```

or a directory using:

```bash
ls -ld <dir>
```

Example:

```bash
janindu@rhel:~$ ls -ld oslab

drwxrwxr-x 3 janindu janindu 4096 Jun 17 02:01 oslab/
```

The permission string has 3 parts:

```text
drwxrwxr-x
```

- `d` = directory
- `rwx` = owner permissions
- `rwx` = group permissions
- `r-x` = other permissions

## 2. Permission values

The permission bits are based on numeric values:

- `r` = 4
- `w` = 2
- `x` = 1

Examples:

```text
-rwxrwxr-x
```

means:

- user: `rwx` = 4+2+1 = 7
- group: `rwx` = 4+2+1 = 7
- others: `r-x` = 4+0+1 = 5

Therefore the mode value is:

```text
775
```

## 3. Changing permissions with chmod

When updating permissions for a specific file or directory, use:

```bash
sudo chmod <value> <file/dir>
```

Example:

```bash
sudo chmod 755 script.sh
ls -l script.sh
```

Example output:

```text
-rwxr-xr-x 1 janindu janindu 4096 Jun 21 12:00 script.sh
```

We can also use symbolic mode:

```bash
chmod +x <file/dir>
chmod +r <file/dir>
chmod +w <file/dir>
```

These add permissions for:

- user
- group
- others

### Add execute permission for the owner only

```bash
chmod u+x script.sh
```

### Add write permission for the group

```bash
chmod g+w file.txt
```

### Add write permission for others

```bash
chmod o+w file.txt
```

### Remove read for others

```bash
chmod o-r file.txt
```

## 4. Symbolic mode and permission categories

`chmod` can work with three categories:

- `u` = user/owner
- `g` = group
- `o` = others

Example:

```bash
chmod u+rwx file.txt
chmod g+rw file.txt
chmod o-r file.txt
```

This allows targeted permission changes without affecting the rest.

## 5. Bonus: recursive permissions

We can use:

```bash
sudo chmod <value> -R <dir>
```

This applies permissions to all files and nested folders inside a directory.

Example:

```bash
sudo chmod 755 -R /var/www/html
```

> Warning: This is powerful. Use it carefully. Running `chmod -R /` would affect the whole Linux system and is not safe.

## 6. Changing ownership and group

### Change owner and group at once

```bash
sudo chown janindu:devteam file.txt
```

Example output:

```text
-rwxrwx--- 1 janindu devteam 4096 Aug 20 13:11 file.txt
```

### Change only the group

```bash
sudo chgrp devteam file.txt
```

### Change group ownership of a directory

```bash
sudo chown :devteam <dir>
```

## 7. Why group membership matters

Since users are grouped and assigned to groups, all users in that group will get the same file permissions if the group owns the file.

See [User and Group Creation](../03-identity-access/01-user-group-creation.md) for more information.

## 8. Special scenario: modify permissions for a specific user

When we need to give permissions for a specific user only and cannot change ownerships or groups, we can use access control lists (ACLs).

### Set ACL permissions for a user

```bash
setfacl -m u:<user>:<permissions> <dir/path>
```

Example:

```bash
janindu@rhel:~$ ls -ld grapes/
drwxrwxr-x 2 janindu janindu 4096 Aug 21 05:02 grapes/
janindu@rhel:~$ sudo setfacl -m u:malshan:rw grapes
janindu@rhel:~$ ls -ld grapes/
drwxrwxr-x+ 2 janindu janindu 4096 Aug 21 05:02 grapes/
janindu@rhel:~$ getfacl grapes
# file: grapes
# owner: janindu
# group: janindu
user::rwx
user:malshan:rwx
group::rwx
mask::rwx
other::r-x
```

- `user:malshan:rwx` means the specific user `malshan` gets read/write/execute access
- `mask::rwx` shows the maximum permission currently applied to group/ACL entries

### Remove ACL permission for a specific user

```bash
setfacl -x u:<user> <dir/file>
```

Example:

```bash
sudo setfacl -x u:malshan grapes
```

This removes the custom ACL entry for that user.

## 9. Delete permission example and directory write behavior

Example:

```bash
janindu@rhel:~$ ls -ld /data_dir

drwxr-xr-x 18 janindu janindu 4096 Jun 14 21:36 /data_dir/
```

Then:

```bash
janindu@rhel:~$ rm -rf test.txt
```

Even if a user does not have write permission on the file, they may still delete it if they have write permission on the containing directory.

This is important:

- file permissions control access to the file itself
- directory permissions control whether you can delete or rename files inside it

Example:

```bash
janindu@rhel:~$ ls -ld /data_dir

drwxr-xr-x 18 janindu janindu 4096 Jun 14 21:36 /data_dir/
-rw-r--r-- 1 janindu janindu 0 Jun 21 05:20 apple.txt
```

In this example, the user can delete `apple.txt` only if they have write permission on `/data_dir`.

## 10. Important notes

- `rwx` on a directory means permission to list the contents, create files, and enter the directory.
- `r-x` on a directory means you can view files but cannot create or delete inside it.
- `w` on a directory is required to create, rename, or delete files inside.
- ACLs are useful when you need to grant access to a specific user without changing ownership or group.

## 11. Summary

File permissions in Linux are based on:

- owner
- group
- others

The basic permission flags are:

- `r` = read
- `w` = write
- `x` = execute

And the numeric values are:

- `4` = read
- `2` = write
- `1` = execute

Use:

- `ls -l` to inspect
- `chmod` to change permissions
- `chown` / `chgrp` to change ownership
- `setfacl` for user-specific access control

This is the foundation of Linux security and access control.
