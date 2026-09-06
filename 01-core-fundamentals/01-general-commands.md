# General Commands and System Navigation

## Concept & Internals
The Linux shell is your primary interface for interacting with the operating system. When you open a terminal, you are placed in a shell environment (like Bash). The shell interprets your text commands, translates them into system calls, and returns the output. Mastering basic navigation and file manipulation is your first step to controlling the system. 

## Core Command Reference

| Command | Syntax | Purpose | Typical use |
| --- | --- | --- | --- |
| `pwd` | `pwd` | Print the current working directory | Confirm where you are in the filesystem |
| `cd` | `cd <directory_path>` | Change directory | Move into another folder |
| `ls` | `ls [options] [path]` | List files and folders | View contents of the current directory |
| `ls -a` | `ls -a [path]` | List hidden files too | See dotfiles like `.bashrc` |
| `ls -al` | `ls -al [path]` | Long listing including hidden files | See permissions, size, and timestamps |
| `mkdir` | `mkdir <directory_name>` | Create a directory | Make a new folder |
| `mkdir -p` | `mkdir -p <path/to/new/folder>` | Create parent folders automatically | Build nested directory structures safely |
| `touch` | `touch <file_name>` | Create an empty file or update timestamps | Create a blank file quickly |
| `rmdir` | `rmdir <empty_directory>` | Remove an empty directory | Delete directories that contain no files |
| `rm -r` | `rm -r <directory>` | Remove directories recursively | Delete folders and their contents |
| `rm -rf` | `rm -rf <directory>` | Force recursive removal | Delete files/directories without prompts |
| `cp` | `cp <source> <destination>` | Copy files or directories | Duplicate a file/folder |
| `mv` | `mv <current_file_path> <new_path>` | Move or rename files/directories | Rename a file or move it to another folder |
| `history` | `history` | Show previously executed commands | Review or repeat commands you already ran |
| `passwd` | `passwd` | Change password | Update your user account password |

### Quick Examples with Terminal Output

#### 1) Navigating directories

```bash
[janindu@rhel ~]$ pwd
/home/janindu

[janindu@rhel ~]$ cd Downloads
[janindu@rhel Downloads]$ pwd
/home/janindu/Downloads
```

- `pwd` tells you your current location.
- `cd` changes to another directory.
- `~` means your home directory, usually `/home/your_user`.
- In these examples, `$` marks a regular user prompt. A `#` prompt commonly indicates a root (superuser) shell. However, prompt symbols can be customized, so do not rely on them as a security check.

#### 2) Listing files and creating folders/files

```bash
[janindu@rhel ~]$ ls
Desktop  Documents  Downloads  Music  Pictures

[janindu@rhel ~]$ ls -a
.  ..  .bashrc  Desktop  Documents  Downloads  .cache

[janindu@rhel ~]$ ls -al
total 12
drwxr-xr-x  5 janindu janindu 4096 Jun  6 10:15 .
drwxr-xr-x  3 root    root    4096 Jun  6 09:50 ..
-rw-r--r--  1 janindu janindu  220 Jun  6 09:30 .bashrc
drwxr-xr-x  2 janindu janindu 4096 Jun  6 10:15 Desktop

[janindu@rhel ~]$ mkdir project
[janindu@rhel ~]$ mkdir -p project/linux-notes/notes
[janindu@rhel ~]$ touch project/linux-notes/notes/intro.md
[janindu@rhel ~]$ ls -R project
project:
linux-notes

project/linux-notes:
notes

project/linux-notes/notes:
intro.md
```

- `ls` shows the visible files and folders.
- `ls -a` includes hidden entries starting with `.`
- `ls -al` gives a detailed long listing.
- `mkdir` creates a directory.
- `mkdir -p` creates nested directories automatically and does not fail if parent folders already exist.
- `touch` creates an empty file if it doesn't exist.

#### 3) Removing and copying files

```bash
[janindu@rhel ~]$ mkdir demo
[janindu@rhel ~]$ touch demo/example.txt
[janindu@rhel ~]$ cp demo/example.txt demo/example-copy.txt
[janindu@rhel ~]$ ls demo
example-copy.txt  example.txt

[janindu@rhel ~]$ mv demo/example-copy.txt demo/renamed.txt
[janindu@rhel ~]$ ls demo
example.txt  renamed.txt

[janindu@rhel ~]$ rm demo/renamed.txt
[janindu@rhel ~]$ rmdir demo
```

- `cp` copies a file.
- `mv` moves or renames a file.
- `rm` removes files.
- `rmdir` removes an empty directory.
- Use `rm -r` to remove directories and contents; `rm -rf` forces the action without confirmation.

> Warning: `rm -rf` is destructive. Use it carefully, especially on important directories.

#### 4) Changing a password

```bash
[janindu@rhel ~]$ passwd
Changing password for janindu.
Current password: 
New password: 
Retype new password: 
passwd: password updated successfully
```

This command lets you update your account password. A system may hide the password input for security.