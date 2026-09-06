# Streams and Redirection

Linux programs communicate with the terminal using streams. A stream is just a flow of data.

## 1. Standard streams

Every command uses these three standard streams:

- `stdin` (standard input) = data coming into the program
- `stdout` (standard output) = normal output from the program
- `stderr` (standard error) = error messages and warnings

In Linux, these are represented by file descriptors:

- `0` = stdin
- `1` = stdout
- `2` = stderr

Example:

```bash
ls /etc
```

This command writes normal directory listing to `stdout`.

If a path is invalid:

```bash
ls /not-here
```

The shell prints an error to `stderr`.

## 2. Redirection operators

```text
/ 
├── home/
│   └── janindu/
│       └── notes/
├── etc/
├── usr/
├── var/
└── ...
```

### `>` : overwrite output to a file

```bash
echo "hello" > greeting.txt
cat greeting.txt
```

Output:

```text
hello
```

- `>` sends `stdout` to a file.
- If the file already exists, it is overwritten.

### `>>` : append output to a file

```bash
echo "first line" > demo.txt
echo "second line" >> demo.txt
cat demo.txt
```

Output:

```text
first line
second line
```

- `>>` keeps previous content.
- It adds new output at the end.

### `<` : send file contents to a command as input

```bash
cat < demo.txt
```

Output:

```text
first line
second line
```

- `<` redirects a file into the command as `stdin`.
- This is useful when a command expects input from a file.

### `2>` : redirect only errors

```bash
ls /wrong/path 2> error.log
cat error.log
```

Output:

```text
ls: cannot access '/wrong/path': No such file or directory
```

- `2>` sends only `stderr` to a file.
- `stdout` still appears in the terminal.

### `2>>` : append errors to a file

```bash
ls /wrong/path 2>> error.log
ls /wrong/path 2>> error.log
cat error.log
```

Output:

```text
ls: cannot access '/wrong/path': No such file or directory
ls: cannot access '/wrong/path': No such file or directory
```

- `2>>` appends error output to the existing log file.

### `&>` : redirect both stdout and stderr

```bash
ls /etc &> output.log
cat output.log
```

Example output:

```text
bin
boot
dev
etc
home
lib
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
```

- `&>` sends both normal output and errors to the same file.

### `1>` : redirect only stdout

```bash
ls /etc 1> list.txt
cat list.txt
```

Output:

```text
bin
boot
dev
etc
home
lib
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
```

- `1>` is the same as `>` but explicit.

### `2>&1` : send stderr to the same place as stdout

```bash
(ls /etc; ls /wrong/path) 2>&1 | tee combined.log
cat combined.log
```

Example output:

```text
bin
boot
dev
etc
home
lib
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
ls: cannot access '/wrong/path': No such file or directory
```

- `2>&1` merges errors into standard output.
- This is useful for logging everything together.

## 3. Pipes `|`

The pipe operator connects the output of one command to the input of another command.

```bash
ls /etc | head
```

Example output:

```text
bin
boot
dev
etc
home
```

- `ls /etc` writes to stdout
- `| head` reads that stdout as input
- `head` prints only the first lines

### Example with grep

```bash
ls /etc | grep "ssh"
```

Example output:

```text
ssh
```

This filters the output to only lines containing `ssh`.

### Example with wc

```bash
ls /etc | wc -l
```

Example output:

```text
24
```

- `wc -l` counts the number of lines.

## 4. Common command combinations

### Save command output to a file

```bash
date > now.txt
cat now.txt
```

Example output:

```text
Sun Jun  6 10:15:00 UTC 2026
```

### Append command output to an existing file

```bash
date >> now.txt
cat now.txt
```

### Redirect a command error to a log

```bash
mkdir /not/allowed 2> mkdir_error.log
cat mkdir_error.log
```

### Combine stdout and stderr into one file

```bash
find /etc -maxdepth 2 2>&1 > find.log
cat find.log
```

## 5. Quick reference

| Operator | Meaning | Example |
| --- | --- | --- |
| `>` | Write stdout to file, overwrite | `echo hi > file.txt` |
| `>>` | Append stdout to file | `echo hi >> file.txt` |
| `<` | Feed file to stdin | `cat < file.txt` |
| `2>` | Send stderr to file | `ls /bad 2> err.log` |
| `2>>` | Append stderr to file | `ls /bad 2>> err.log` |
| `&>` | Send stdout + stderr to file | `cmd &> log.txt` |
| `\|` | Pipe output to another command | `ls /etc \| grep ssh` |

## 6. Summary

Redirection and piping are core Linux skills. They let you:

- save output to files
- append logs
- redirect errors separately
- combine commands into useful pipelines

The key idea is simple:

- `>` writes output
- `<` reads input
- `|` connects commands
- `2>` handles errors

This makes Linux commands much more flexible and powerful.
