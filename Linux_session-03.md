# Linux & AWS EC2 – Class Notes (Session 02)

Beginner-friendly notes with concepts, commands, examples, corrections, and practice questions.

## 1. AWS EC2 & SSH Connection

### Generate an SSH key pair

```bash
ssh-keygen -f daws-86s
```

This creates:

| File | Purpose |
|---|---|
| `daws-86s` | Private key — keep it secure; never share it |
| `daws-86s.pub` | Public key — can be installed on the EC2 instance |

### Connect to an EC2 instance

```bash
ssh -i <private-key> ec2-user@<IP>
```

Example:

```bash
ssh -i daws-86s ec2-user@13.234.100.10
```

### EC2 security group rules

| Direction | Protocol | Port | Source/Destination |
|---|---|---:|---|
| Inbound | SSH | 22 | `0.0.0.0/0` |
| Outbound | All traffic | All | `0.0.0.0/0` |

> **Security:** `0.0.0.0/0` allows access from anywhere. For production, restrict SSH inbound access to your own IP or trusted network.

---

## 2. Git Bash & Linux Paths

Git Bash provides a Linux-like command-line environment on Windows and supports many Unix/Linux commands.

### Windows path vs. Git Bash path

- Windows: `C:\devops\daws-86s`
- Git Bash: `/c/devops/daws-86s`

```bash
cd /c/devops/daws-86s
```

### Absolute path vs. relative path

| Absolute path | Relative path |
|---|---|
| Starts from `/` (or a drive root in Windows/Git Bash) | Starts from the current directory |
| Specifies the complete path | Specifies a path relative to the current location |
| Example: `/c/devops/daws-86s` | Example: `daws-86s` |

```bash
cd /c/devops/daws-86s   # Absolute path
cd daws-86s             # Relative path; directory must exist here
```

---

## 3. Linux command structure

General syntax:

```bash
command <inputs> <options>
```

Example:

```bash
ls -l
```

- `ls` — command
- `-l` — option

Inputs and options may be required or optional depending on the command.

Example:

```bash
cp <source> <destination>
```

---

## 4. Linux users & root

### Prompt symbols

| Symbol | Meaning |
|---|---|
| `$` | Normal-user prompt |
| `#` | Root-user prompt |

### Important directories

| Directory | Meaning |
|---|---|
| `/` | Root of the filesystem |
| `/root` | Root user's home directory |
| `/home/ec2-user` | `ec2-user` home directory |
| `/home/<username>` | Typical home directory for a normal user |

### Switch to root

```bash
sudo su -
```

- Switches to root.
- Starts a login shell.
- Changes to root's home directory (`/root`).

```bash
sudo su
```

- Switches to root.
- Usually remains in the current working directory.
- Does not perform the same login-shell initialization as `sudo su -`.

### `pwd` — Print working directory

```bash
pwd
```

Displays the current directory, for example:

```text
/home/ec2-user
```

---

## 5. File operations (CRUD)

CRUD means:

- **C** — Create
- **R** — Read
- **U** — Update
- **D** — Delete

### Create a file

```bash
touch file.txt
```

Creates an empty file if it does not exist. If it already exists, updates its timestamps.

### Read a file

```bash
cat file.txt
```

Displays the file's contents.

### Create/write a file interactively

```bash
cat > file.txt
```

Type your content, press **Enter**, then **Ctrl+D** to finish:

```text
This is Linux class
Learning DevOps
```

### Redirection operators

| Operator | Meaning |
|---|---|
| `>` | Overwrites the destination file |
| `>>` | Appends to the destination file |

```bash
echo "Hello Linux" > file.txt
echo "Hello DevOps" >> file.txt
```

---

## 6. Directory commands

### `mkdir` — Make directory

```bash
mkdir devops
```

### `rmdir` — Remove an empty directory

```bash
rmdir devops
```

### `rm` — Remove files/directories

```bash
rm file.txt
rm -r devops
```

- `rm file.txt` — deletes a file.
- `rm -r devops` — recursively deletes a directory and its contents.

> **Caution:** `rm -rf directory` forcefully deletes a directory and its contents without prompting. Use carefully.

### `cd` — Change directory

```bash
cd /home/ec2-user
cd ..    # Parent directory (one level up)
cd .     # Current directory
```

Linux is case-sensitive: `cd` and `Cd` are different.

---

## 7. `ls` — List files and directories

```bash
ls
```

### Useful options

| Command | Meaning |
|---|---|
| `ls -l` | Long listing format |
| `ls -r` | Reverse sort order |
| `ls -t` | Sort by modification time, newest first |
| `ls -lr` | Long listing, reverse sort order |
| `ls -lt` | Long listing, newest modified first |
| `ls -ltr` | Long listing, oldest modified first |

**Note:** `ls -l` does not itself mean alphabetical order. `ls` sorts alphabetically by default (subject to locale and options).

---

## 8. `cp` and `mv`

### `cp` — Copy

```bash
cp <source> <destination>
cp file.txt backup.txt
cp file.txt /tmp/
```

### `mv` — Move or rename

```bash
mv <source> <destination>
mv file.txt /tmp/       # Move
mv old.txt new.txt      # Rename
```

---

## 9. `grep` — Search text in files

### Syntax

```bash
grep <word-to-search> <file-name>
```

Example:

```bash
grep Linux file.txt
```

### Useful options

| Option | Meaning |
|---|---|
| `-n` | Show line numbers |
| `-c` | Count matching lines |
| `-i` | Ignore case |
| `-v` | Show non-matching lines |

```bash
grep -n Linux file.txt
grep -c Linux file.txt
grep -i linux file.txt
grep -v Linux file.txt
```

> `grep -c` counts matching **lines**, not every occurrence of the word.

---

## 10. `wget` and `curl`

### `wget` — Download a file

```bash
wget <URL>
wget https://example.com/file.txt
```

Downloads the file into the current directory.

### `curl` — Transfer data

```bash
curl <URL>
curl https://example.com
```

By default, displays the response body in the terminal.

Download using `curl`:

```bash
curl -O https://example.com/file.txt
```

`-O` saves using the remote filename.

---

## 11. `cut` — Extract fields

### Syntax

```bash
cut -d "<delimiter>" -f <field-number>
```

- `-d` — delimiter
- `-f` — field number

Example URL:

```text
https://raw.githubusercontent.com/daws-86s/notes/refs/heads/main/session-02.txt
```

Commands:

```bash
echo "https://raw.githubusercontent.com/daws-86s/notes/refs/heads/main/session-02.txt" | cut -d "/" -f1
```

Output:

```text
https:
```

### URL fields separated by `/`

| Field | Value |
|---:|---|
| 1 | `https:` |
| 2 | Empty field |
| 3 | `raw.githubusercontent.com` |
| 4 | `daws-86s` |
| 5 | `notes` |
| 6 | `refs` |
| 7 | `heads` |
| 8 | `main` |
| 9 | `session-02.txt` |

The second field is empty because two slashes occur after `https:`.

---

## 12. `awk` — Extract and process fields

`awk` is a powerful text-processing tool used to extract, filter, and manipulate data.

### Syntax

```bash
awk -F "<delimiter>" '{print $N}' file
```

- `-F` — field separator
- `$1` — first field
- `$2` — second field
- `$3` — third field
- `$NF` — last field

Example:

```bash
echo "https://raw.githubusercontent.com/daws-86s/notes/refs/heads/main/session-02.txt" | awk -F "/" '{print $3}'
```

Output:

```text
raw.githubusercontent.com
```

Extract the first field:

```bash
echo "https://raw.githubusercontent.com/daws-86s/notes/refs/heads/main/session-02.txt" | awk -F "/" '{print $1}'
```

Output:

```text
https:
```

Extract the last field:

```bash
echo "https://raw.githubusercontent.com/daws-86s/notes/refs/heads/main/session-02.txt" | awk -F "/" '{print $NF}'
```

Output:

```text
session-02.txt
```

> **Correction:** `awk -F "/" '{print $1F}'` is not the correct syntax for printing field 1. Use `'{print $1}'`. For field 3, use `'{print $3}'`.

### `cut` vs. `awk`

| `cut` | `awk` |
|---|---|
| Simple field/character extraction | Advanced text processing |
| Easy for basic column extraction | Can extract, filter, and transform data |
| Uses delimiters and field numbers | Uses field variables such as `$1`, `$2`, `$NF` |

---

## 13. Linux symbols

| Symbol | Meaning |
|---|---|
| `/` | Root directory |
| `.` | Current directory |
| `..` | Parent directory |
| `~` | Current user's home directory |
| `$` | Normal-user prompt |
| `#` | Root-user prompt |
| `>` | Overwrite redirection |
| `>>` | Append redirection |
| `\|` | Pipe operator |

### Pipe (`|`)

A pipe sends one command's output to another command as input.

```bash
cat file.txt | grep Linux
```

Flow:

```text
cat file.txt → grep Linux → Matching lines
```

Another example:

```bash
echo "Hello Linux" | cut -d " " -f1
```

Output:

```text
Hello
```

---

## 14. Quick revision — command cheat sheet

| Command | Purpose |
|---|---|
| `pwd` | Print working directory |
| `ls` | List files |
| `ls -l` | Long listing |
| `cd` | Change directory |
| `cd ..` | Move one level up |
| `mkdir` | Create directory |
| `rmdir` | Remove empty directory |
| `touch` | Create file |
| `cat` | Read file |
| `cat > file` | Write file |
| `cp` | Copy |
| `mv` | Move/rename |
| `rm` | Delete |
| `rm -r` | Delete recursively |
| `grep` | Search text |
| `cut` | Extract fields |
| `awk` | Process text |
| `wget` | Download file |
| `curl` | Transfer/display data |
| `uname -a` | Display system information |
| `sudo su -` | Switch to root login shell |

---

## 15. Practice exercises

Try these in Git Bash or an EC2 Linux terminal:

1. Print your current working directory.
2. Create a directory named `linux-practice`.
3. Enter the directory.
4. Create three files: `file1.txt`, `file2.txt`, and `file3.txt`.
5. Write content into `file1.txt`.
6. Display the file content.
7. Copy `file1.txt` to `backup.txt`.
8. Rename `file2.txt` to `devops.txt`.
9. Search for `Linux` inside `file1.txt`.
10. Display files in reverse alphabetical order.
11. Download a file using `wget`.
12. Extract the third field from a URL using `awk`.
13. Switch to root.
14. Return to your normal user.
15. Delete the practice directory and its contents.

---

## 16. Interview questions

- What is the difference between absolute and relative paths?
- What is the difference between `cp` and `mv`?
- What is the difference between `>` and `>>`?
- What is the difference between `rmdir` and `rm -r`?
- What is the difference between `sudo su` and `sudo su -`?
- What is the difference between `grep -c` and `grep -n`?
- What is the difference between `cut` and `awk`?
- What is the difference between `wget` and `curl`?
- What is the difference between `/` and `/root`?
- Why is SSH port 22 used?

---

**Linux & AWS EC2 — Session 02 complete**
