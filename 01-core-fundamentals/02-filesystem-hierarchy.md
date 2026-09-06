# Linux Filesystem Hierarchy

The Linux filesystem is organized as a tree starting from the root directory `/`. Every file and folder in Linux lives somewhere under this top-level directory.

## 1. Root directory and shell prompts

- `/` = root directory (top of the filesystem)
- `~` = current user's home directory
- `/root` = home directory of the root (superuser) account
- `$` = normal user shell prompt
- `#` = root/sudo shell prompt

Examples:

```bash
[janindu@rhel ~]$ pwd
/home/janindu

[janindu@rhel ~]$
# user mode

[root@rhel ~]# pwd
/root

[root@rhel ~]#
# root/sudo mode
```

Switching from a normal user to root with sudo:

```bash
[janindu@rhel ~]$ sudo su -
[sudo] password for janindu: 
[root@rhel ~]# pwd
/root
```

- `sudo` = run a command with administrative privileges
- `su` = switch user
- `-` = start a login shell as the target user, loading their environment
- After this, the prompt changes to `#`, which means you are in root/sudo mode

Common prompt meanings:

- `~$` = home folder in user mode
- `/$` = root folder in user mode
- `~#` or `/#` = root/admin context
- `/root` = superuser home directory

## 2. Main filesystem tree

```text
/
├── bin -> /usr/bin
├── boot/
│   └── ... boot files, kernel, bootloader config
├── dev/
│   └── ... device files (hardware and virtual devices)
├── etc/
│   └── ... system-wide config files
├── home/
│   ├── janindu/
│   ├── alice/
│   └── ... user home directories
├── root/
│   └── ... root user's files (admin only)
├── usr/
│   ├── bin/
│   ├── sbin/
│   ├── lib/
│   ├── share/
│   └── local/
├── var/
│   ├── log/
│   ├── mail/
│   └── ... variable data
├── tmp/
│   └── ... temporary files
├── opt/
│   └── ... third-party software
├── media/
│   └── ... removable media like USB drives
├── mnt/
│   └── ... mounted filesystems (manual mounts)
├── srv/
│   └── ... service data
├── proc/
│   └── ... kernel and process information
├── sys/
│   └── ... kernel hardware info and sysfs
├── run/
│   └── ... runtime data for current session
├── sbin/
│   └── ... system admin binaries
├── lib/
│   └── ... shared libraries
├── lib64/
│   └── ... 64-bit shared libraries
└── ...
```

## 3. Important directories and what they contain

### `/` (root)
The root directory is the top-level folder of the filesystem. Everything else is under it.

### `/bin`
Contains essential binary executables needed for basic system operation.
Examples:
- `ls`
- `cp`
- `mv`
- `cat`

On most modern systems, `/bin` is a symbolic link to `/usr/bin`.

### `/sbin`
Contains system administration commands used mainly by root or the system administrator.
Examples:
- `reboot`
- `fdisk`
- `ifconfig` (older systems)

### `/boot`
Contains files needed to boot the operating system, including:
- kernel files
- bootloader configuration
- initial ramdisk images

### `/etc`
Contains system-wide configuration files.
Examples:
- `/etc/fstab` - disk mount information
- `/etc/passwd` - user account information
- `/etc/group` - group information
- `/etc/hostname` - system hostname

### `/home`
Contains personal home directories for regular users.
Example:

```bash
/home/janindu
/home/alice
```

Each user usually keeps documents, downloads, configs, and project files here.

### `/root`
This is the home directory for the root user. Only root/admin users normally have access.

### `/dev`
Contains device files that represent hardware and virtual resources.
Linux treats devices like files, so programs can read from or write to them.
Examples:
- hard disks
- terminals
- USB devices
- virtual devices

### `/usr`
Contains user-space programs and files, such as:
- command binaries
- libraries
- documentation
- shared system resources

Common subdirectories:
- `/usr/bin` - most user commands
- `/usr/sbin` - admin tools for advanced use
- `/usr/lib` - shared libraries
- `/usr/share` - common data and documentation

### `/var`
A variable-data directory that stores files that change often while the system runs.
Examples:
- `/var/log` - system logs
- `/var/mail` - mail storage
- `/var/cache` - cached data

### `/tmp`
Temporary directory for short-lived files used by applications and the system.

### `/opt`
Used for optional or third-party software packages that are installed outside the normal system directories.
Examples:
- IDEs
- commercial software
- self-contained applications

### `/media`
Used for automatically mounted removable media such as:
- USB flash drives
- external hard disks
- SD cards

### `/mnt`
Traditional mount point for manually mounted filesystems or external storage.

### `/proc`
A virtual filesystem that provides information about running processes and the kernel.
It does not live on disk; it is generated dynamically.

### `/sys`
Kernel virtual filesystem exposing hardware and system information.
It is mainly used for system and driver introspection.

### `/run`
Stores runtime information generated during the current system session.
Examples:
- process IDs
- sockets
- temporary runtime state

## 4. Absolute vs relative paths

### Absolute path
An absolute path begins from the root directory `/`.
Examples:

```bash
/home/janindu
/etc/fstab
/usr/bin/bash
```

### Relative path
A relative path is based on your current working directory.
Examples:

```bash
cd /home/janindu
ls Documents
```

This refers to `/home/janindu/Documents`.

Useful relative path markers:

- `.` = current directory
- `..` = parent directory

Examples:

```bash
pwd
/home/janindu

ls ..
# shows /home

ls .
# shows files in current directory
```

## 5. Notes about file system behavior

- Linux uses a single root tree: there is only one `/`.
- Linux treats hardware and devices as files in `/dev`.
- Most configuration files live under `/etc`.
- User files and projects are usually under `/home/<username>`.
- Root-level folders are important for system organization and security.

## 6. Quick command examples

```bash
pwd
# prints current directory

ls /
# lists root directory contents

ls /home
# shows user home folders

ls /etc
# shows system configuration files

ls -l /var/log
# shows logs with details
```

## 7. Shortcut: `ll` command

In many Linux systems, you may see:

```bash
ll
```

This is often an alias for:

```bash
ls -l
```

It shows a long listing with permissions, file sizes, and timestamps.

## 8. Summary

The Linux filesystem hierarchy is structured so that:

- `/` is the root of the system
- `/home` stores user files
- `/etc` stores system configuration
- `/usr` stores installed programs and libraries
- `/var` stores changing system data
- `/dev`, `/proc`, and `/sys` expose devices and running system information

Understanding this layout is essential for navigation, maintenance, and troubleshooting in Linux.
