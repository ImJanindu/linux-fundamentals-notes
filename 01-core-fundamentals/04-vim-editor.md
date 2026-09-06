# Vim Editor Basics

Vim is a powerful terminal-based text editor used in Linux and Unix systems. It is lightweight, fast, and works well in remote servers and headless environments.

Unlike editors that open in a graphical interface, Vim works mainly through keyboard commands. It has different modes, so you must understand when you are in normal mode, insert mode, and command-line mode.

## 1. Install Vim

Vim is usually not installed on every Linux system by default. You can install it using the package manager for your distribution.

### Ubuntu/Debian

```bash
sudo apt update
sudo apt install vim
```

### RHEL/CentOS/Fedora

```bash
sudo dnf install vim
```

or on older RHEL-based systems:

```bash
sudo yum install vim
```

### Arch Linux

```bash
sudo pacman -S vim
```

### openSUSE

```bash
sudo zypper install vim
```

After installation, you can open a file with:

```bash
vim filename.txt
```

## 2. Opening and exiting Vim

### Open a file

```bash
vim filename.txt
```

If the file does not exist, Vim will create it when you save.

### Start editing

Press the `i` key to enter Insert mode.

```text
Esc -> switch back to Normal mode
```

### Exit without saving

```vim
:q!
```

- `:` enters command mode
- `q` quits
- `!` forces quit without saving

### Exit after saving

```vim
:wq
```

or

```vim
:wq!
```

- `w` writes/saves the file
- `q` quits
- `!` forces the operation even if there are warnings

> Note: The `!` mark is optional in many cases, but it is useful when you want to force an action.

## 3. Important Vim modes

### Normal mode
This is the default mode when Vim opens.

Used for:
- navigation
- deleting text
- copying and pasting
- searching
- saving and quitting

### Insert mode
Press `i` to insert text.

You can also use:
- `a` = insert after the current cursor position
- `I` = insert at the beginning of the line
- `A` = insert at the end of the line
- `o` = insert a new line below
- `O` = insert a new line above

### Command mode
Press `:` to enter command mode, then type commands like:
- `:w`
- `:q`
- `:wq`
- `:set nu`
- `:set nonu`

> Important: Before running commands, writing files, switching modes, or doing any other operation, always press `Esc` first to return to Normal mode.

This is a key Vim habit: if you are in Insert mode, the editor will treat your keystrokes as text, not commands.

## 3. Navigation inside Vim

### Move the cursor

```text
h       move left
j       move down
k       move up
l       move right
```

### Move by words

```text
w       next word
b       previous word
e       end of word
```

### Move by line

```text
0       beginning of line
$       end of line
G       last line of file
gg      first line of file
```

### Move by screen

```text
Ctrl + f    move forward one page
Ctrl + b    move backward one page
```

## 4. Search and find

### Search forward

```vim
/<query>
```

Example:

```vim
/hello
```

Then press:
- `n` = next match
- `N` = previous match

### Search and replace

```vim
:%s/<find>/<replace>/g
```

Example:

```vim
:%s/old/new/g
```

This means:
- replace all occurrences of `old` with `new`
- `g` means globally across the file

Example with a whole word:

```vim
:%s/hello/world/g
```

## 5. Line numbers

### Show line numbers

```vim
:set nu
```

### Hide line numbers

```vim
:set nonu
```

This is useful when you want to quickly reference a line number or debug errors.

## 6. Undo and redo

### Undo

Press `Esc` and then:

```text
u
```

This undoes the last action.

### Redo

```text
Ctrl + r
```

or

```vim
:redo
```

## 7. Selecting text

### Enter visual mode

```text
v
```

This selects text character by character.

### Select whole lines

```text
V
```

This selects lines.

### Select a block

```text
Ctrl + v
```

This selects a rectangular block.

### Exit visual mode

```text
Esc
```

## 8. Cut, copy, and paste

### Copy (yank)

```text
yy
```

This copies the current line.

To copy multiple lines:

```text
y2y
```

This copies 2 lines.

### Cut (delete)

```text
dd
```

This cuts the current line.

Example:

```text
d3d
```

This cuts 3 lines.

### Paste

```text
p
```

This pastes after the cursor position.

```text
P
```

This pastes before the cursor position.

## 9. Deleting and changing text

### Delete a character

```text
x
```

### Delete a word

```text
dw
```

### Delete from cursor to end of line

```text
d$
```

### Replace a character

```text
r
```

Then type the new character.

### Replace a whole word

```text
cw
```

This changes the word under the cursor.

## 10. Save and quit examples

```vim
:w
```

Save the file without quitting.

```vim
:q
```

Quit only if no changes were made.

```vim
:q!
```

Quit without saving.

```vim
:wq
```

Save and quit.

```vim
:wq!
```

Force save and quit.

## 11. Useful practice examples

### Example 1: Create and edit a file

```bash
vim notes.txt
```

Then:

```text
i
hello world
Esc
:wq
```

### Example 2: Search for text

```vim
/linux
n
```

This searches for `linux` and moves to the next match.

### Example 3: Add line numbers

```vim
:set nu
```

### Example 4: Remove numbers

```vim
:set nonu
```

### Example 5: Replace text globally

```vim
:%s/old/new/g
```

### Example 6: Select and copy a block

```text
v
```

Select text with the cursor, then:

```text
y
```

Copy it, then:

```text
p
```

Paste it elsewhere.

## 12. Quick reference summary

| Action | Command |
| --- | --- |
| Open file | `vim filename.txt` |
| Insert text | `i` |
| Go back to normal mode | `Esc` |
| Save | `:w` |
| Quit | `:q` |
| Save + quit | `:wq` |
| Force quit without saving | `:q!` |
| Force save + quit | `:wq!` |
| Search | `/<query>` |
| Next result | `n` |
| Show line numbers | `:set nu` |
| Hide line numbers | `:set nonu` |
| Undo | `u` |
| Redo | `Ctrl + r` |
| Copy line | `yy` |
| Cut line | `dd` |
| Paste | `p` |
| Replace all | `:%s/find/replace/g` |

## 13. Final note

Vim is keyboard-driven. The most important habit is to remember the mode you are in. In particular, always press `Esc` before running commands or switching actions. This keeps the editor predictable and helps avoid accidental edits.

With regular practice, Vim becomes a very efficient tool for editing files directly from the terminal.
