# Path Resolution and Navigation

In Linux, every file and folder is found by its path. A path tells the system where something lives inside the filesystem tree. Understanding how Linux resolves a path is essential for navigating directories, running commands, and managing files.

## 1. Working directory and shell context

- `/` = root directory
- `~` = home directory of the current user
- `.` = current directory
- `..` = parent directory
- `$` = normal user prompt
- `#` = root/sudo prompt

Examples:

```bash
[janindu@rhel ~]$ pwd
/home/janindu

[janindu@rhel ~]$ cd Documents
[janindu@rhel Documents]$ pwd
/home/janindu/Documents

[janindu@rhel Documents]$ cd ..
[janindu@rhel ~]$ pwd
/home/janindu
```

- `pwd` prints the current location.
- `cd` changes the working directory.
- `..` moves one directory up.

## 2. Path types in Linux

```text
/
├── home/
│   └── janindu/
│       ├── Documents/
│       ├── Downloads/
│       └── projects/
├── etc/
├── usr/
├── var/
├── tmp/
└── ...
```

### Absolute path
An absolute path starts from `/` and gives the full location of a file or directory.

Examples:

```bash
/home/janindu
/home/janindu/Documents
/etc/fstab
/usr/bin/ls
```

- Always valid, no matter where you are currently located.
- Best for clarity and scripting.

### Relative path
A relative path is based on your current working directory.

Examples:

```bash
[janindu@rhel ~]$ pwd
/home/janindu

[janindu@rhel ~]$ ls Documents

[janindu@rhel ~]$ cd projects
[janindu@rhel projects]$ ls ../Downloads
```

- `Documents` means `/home/janindu/Documents` from the home directory.
- `../Downloads` means go up one level, then into `Downloads`.

## 3. Special path shortcuts

### `.` - current directory

```bash
[janindu@rhel ~]$ pwd
/home/janindu

[janindu@rhel ~]$ ls .
```

This shows the contents of the current directory.

### `..` - parent directory

```bash
[janindu@rhel ~]$ cd Documents
[janindu@rhel Documents]$ cd ..
[janindu@rhel ~]$ pwd
/home/janindu
```

### `../..` - move up two levels

```bash
[janindu@rhel ~]$ cd /home/janindu/projects/linux-notes
[janindu@rhel linux-notes]$ cd ../..
[janindu@rhel ~]$ pwd
/home/janindu
```

- `..` moves up one directory
- `../..` moves up two directories
- This is useful when you want to go back to a higher-level folder quickly

### `~` - home directory

```bash
[janindu@rhel ~]$ pwd
/home/janindu

[janindu@rhel ~]$ cd ~/Downloads
[janindu@rhel Downloads]$ pwd
/home/janindu/Downloads
```

- `~` is short for your home folder.
- It makes paths easier to read and write.

## 4. How Linux resolves paths

When you type a command, the shell interprets the path based on your current directory.

Example:

```bash
[janindu@rhel ~]$ pwd
/home/janindu

[janindu@rhel ~]$ ls projects
```

This means:

```bash
ls /home/janindu/projects
```

If you are in a different directory, the same relative path may point somewhere else.

```bash
[janindu@rhel ~]$ cd /etc
[janindu@rhel etc]$ ls ../home
```

This resolves to:

```bash
ls /home
```

So, the meaning of a relative path depends on your current location.

## 5. Common navigation commands

| Command | Syntax | Purpose | Example |
| --- | --- | --- | --- |
| `pwd` | `pwd` | Print current directory | `pwd` |
| `cd` | `cd <path>` | Change directory | `cd /home/janindu/Downloads` |
| `ls` | `ls [path]` | List files in a directory | `ls ~/Documents` |
| `realpath` | `realpath <path>` | Show the full absolute path | `realpath ./notes.txt` |

### Example with `realpath`

```bash
[janindu@rhel ~]$ cd Documents
[janindu@rhel Documents]$ realpath ./notes.txt
/home/janindu/Documents/notes.txt
```

This converts a relative path into an absolute path.

## 6. Absolute vs relative path summary

| Type | Starts with | Depends on current location? | Example |
| --- | --- | --- | --- |
| Absolute | `/` | No | `/home/janindu/Downloads` |
| Relative | `.` / `..` / file name / folder name | Yes | `../Downloads` |
| Home shortcut | `~` | No, relative to current user | `~/Documents` |

## 7. Quick examples

```bash
# absolute path
ls /home/janindu

# relative path from home
ls Documents

# move to parent directory
cd ..

# navigate up two parent directory levels using double dots
cd ../..

# go to home directory quickly
cd ~

# show current directory
pwd
```

## 8. Best practices

- Use absolute paths in scripts and documentation when clarity matters.
- Use relative paths when working quickly inside a project directory.
- Use `.` and `..` carefully when moving around folders.
- Use `pwd` often to confirm where you are.

## 9. Summary

Path resolution in Linux is based on the filesystem tree. Absolute paths start at `/`, while relative paths start from your current directory. Special symbols like `.` , `..`, and `~` help simplify navigation. Understanding these ideas makes terminal work more predictable and easier to manage.
