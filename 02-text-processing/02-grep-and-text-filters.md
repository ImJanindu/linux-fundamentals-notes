# Grep, Awk, and Sed

Text processing is one of the most useful parts of Linux. Instead of opening large files in a GUI editor, you can quickly search, filter, transform, and summarize content directly from the terminal.

## Concept & Internals

Linux tools are designed to work with streams: text enters through standard input (`stdin`), is processed by a command, and then the result is printed to standard output (`stdout`).

This makes commands like `grep`, `awk`, and `sed` easy to combine with pipes (`|`) and redirection (`>`, `>>`).

The key idea is simple:

- `grep` = search/filter lines
- `awk` = process columns/fields
- `sed` = modify or print text based on patterns

## Core Command Reference

| Command | Purpose | Example |
| --- | --- | --- |
| `grep` | Search for matching text in a file or stream | `grep "error" /var/log/syslog` |
| `grep -i` | Case-insensitive search | `grep -i "error" file.txt` |
| `grep -n` | Show lines with numbers | `grep -n "root" /etc/passwd` |
| `grep -v` | Show lines that do not match | `grep -v "#" config.txt` |
| `grep -c` | Count matching lines | `grep -c "error" /var/log/messages` |
| `grep -E` | Extended regex search | `grep -E "(error\|fail)" logfile` |
| `grep -r` | Recursive search in folders | `grep -r "TODO" .` |
| `awk` | Filter and print columns | `df -h \| awk '{print $1, $2}'` |
| `sed` | Replace or print text | `sed 's/old/new/' file.txt` |
| `sed -n` | Print specific lines | `sed -n '74p' file.txt` |
| `sed -i` | Edit file in place | `sed -i 's/old/new/g' file.txt` |

## 1. Grep: search for text

### Search for a specific word

```bash
grep "root" /etc/passwd
```

Example output:

```text
root:x:0:0:root:/root:/bin/bash
```

- This prints only lines containing the word `root`.

### Case-insensitive search

```bash
grep -i "error" /var/log/syslog
```

- `-i` ignores uppercase/lowercase differences.
- Useful when the text may appear as `Error`, `ERROR`, or `error`.

### Search with line numbers

```bash
grep -n "error" /var/log/syslog
```

Example output:

```text
114:Jun  6 10:20:01 server kernel: error 42
220:Jun  6 10:25:12 server sshd[245]: error: failed login
```

- `-n` shows the line number before each matching line.

### Print all lines with line numbers

```bash
grep -n "" /var/log/syslog
```

- This prints every line, but each one is numbered.

### Search for lines that do not match

```bash
grep -v "#" config.txt
```

Example output:

```text
server_name=myserver
port=8080
```

- `-v` shows only lines that do not contain the given pattern.
- Often used to remove comments from config files.

### Count matching lines

```bash
grep -c "error" /var/log/syslog
```

Example output:

```text
12
```

- This returns the number of lines containing `error`.

### Multiple arguments and flags

```bash
grep -i -n "error" /var/log/syslog
```

- You can combine options together.
- This is a common and useful pattern.

### Search for patterns at the start of a line

```bash
grep "^J" names.txt
```

Example output:

```text
John
Jane
Jacob
```

- `^J` means: lines that start with `J`.

## 2. Redirect grep output to a file

### Overwrite a target file

```bash
grep "error" /var/log/syslog > error_report.txt
cat error_report.txt
```

Example output:

```text
Jun  6 10:20:01 server kernel: error 42
Jun  6 10:25:12 server sshd[245]: error: failed login
```

- `>` replaces the file content with the new output.
- This is destructive: previous content is overwritten.

### Append to a file

```bash
grep "warning" /var/log/syslog >> error_report.txt
cat error_report.txt
```

- `>>` appends new results to the end of the file.

> Warning: `>` replaces everything; `>>` adds to the end.
>
> This rule is not only for `grep`; it works for any command output such as:
> `df -h > disk.txt`
> `ls > files.txt`

## 3. Pipe examples

Pipes let you pass output from one command into another command.

### Example: filter mounted partitions

```bash
df -h | grep "/dev"
```

Example output:

```text
/dev/sda1      20G   12G  7.5G  62% /
/dev/sdb1      100G   30G  70G  30% /data
```

- `df -h` lists disk space usage.
- `grep "/dev"` keeps only lines containing `/dev`.

### Example: search processes

```bash
ps -ef | grep ssh
```

- This shows only running processes related to SSH.

## 4. AWK: filter output by columns

`awk` is excellent for working with structured text, especially output from commands like `df`, `ps`, and `ls`.

### Print specific columns

```bash
df -h | awk '{print $1, $2}'
```

Example output:

```text
Filesystem Size
/dev/sda1 20G
/dev/sdb1 100G
```

- `$1` = first column
- `$2` = second column

### Example with a custom text file

```bash
cat users.txt
```

```text
john admin 1001
alice user 1002
bob admin 1003
```

```bash
awk '{print $1, $3}' users.txt
```

Output:

```text
john 1001
alice 1002
bob 1003
```

- `awk` is very useful for column-based filtering.

## 5. SED: search and replace text

`sed` is used for text transformation. It can print, replace, or delete patterns.

### Replace the first match in a line

```bash
sed 's/Engineering/Doctor/' sample.txt
```

Example input:

```text
Engineering team is busy.
Engineering team is growing.
```

Example output:

```text
Doctor team is busy.
Engineering team is growing.
```

- `s/old/new/` replaces the first matched occurrence per line.

### Replace all matches in a line

```bash
sed 's/Engineering/Doctor/g' sample.txt
```

Example output:

```text
Doctor team is busy.
Doctor team is growing.
```

- `g` means global replacement within the line.

### Print only specific lines

```bash
sed -n '74p' file.txt
```

- `-n` prevents printing all lines.
- `74p` prints only line number 74.

### Print the first 74 lines

```bash
sed -n '1,74p' file.txt
```

- `1,74p` prints lines from 1 through 74 only.
- This is useful when you want to preview the beginning of a file without showing the whole content.

### Remove a specific line from output

```bash
sed '3d' file.txt
```

- `3d` deletes the 3rd line from the output.
- This does not modify the file unless you use `-i`.

### In-place edit using `sed -i`

```bash
sed -i 's/old/new/g' file.txt
```

- This edits the file directly.
- Use carefully, especially on important files.

## 6. Useful combinations

### Search and count matches

```bash
grep -c "error" /var/log/syslog
```

### Search in all files inside a folder

```bash
grep -r "TODO" .
```

- `-r` searches recursively through subdirectories.

### Show only matching lines and preserve line numbers

```bash
grep -in "warning" config.log
```

- `-i` = ignore case
- `-n` = show line numbers

## 7. Quick examples summary

```bash
# search for a word
grep "root" /etc/passwd

# case-insensitive search
grep -i "error" logs.txt

# line numbers
grep -n "error" logs.txt

# lines that do NOT match
grep -v "#" config.txt

# count matches
grep -c "error" logs.txt

# pipe output
df -h | grep "/dev"

# columns with awk
df -h | awk '{print $1, $2}'

# replace text with sed
sed 's/old/new/g' file.txt
```

## 8. Vim note

Text-processing tools are extremely powerful, and Vim also includes similar editing and searching features built in. For keyboard-based editing and searching inside a file, see [Vim](../01-core-fundamentals/04-vim-editor.md).

## 9. Summary

`grep` helps you find text.
`awk` helps you work with columns.
`sed` helps you replace and transform text.

These commands are often used together with pipes and redirection to build efficient text-processing workflows in Linux.
