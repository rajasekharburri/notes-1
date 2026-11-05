# CLASS NOTES: VIM EDITOR & LINUX FILE VIEWING

## 1. Vim Editor

### Introduction
- **Vim** stands for **Vi Improved**.
- Vim is a text editor used in Linux to create, edit, and modify files.
- It is an improved version of the `vi` editor.

### Syntax
```bash
vim <file-name>
```

### How Vim Works
1. If the file already exists, Vim opens the file.
2. If the file does not exist, Vim creates a new file buffer. The file is created on disk when you save it.

### Vim Modes
Vim mainly works with the following modes:

| Mode | Description |
|---|---|
| Normal Mode (Esc Mode) | Used for navigation and editing commands |
| Insert Mode | Used to insert or type text |
| Command-Line Mode | Used to execute commands like save, quit, and search |

**Note:** Vim starts in Normal Mode by default.

---

## 2. Command-Line Mode (Colon Mode)

To enter Command-Line Mode:
- Press `Esc`
- Type `:`
- Enter the required command

### Save and Exit Commands

| Command | Description |
|---|---|
| `:w` | Save the file |
| `:q` | Quit the editor |
| `:wq` | Save and quit |
| `:q!` | Quit without saving changes |
| `:w!` | Force save (subject to permissions) |

### Line Number Commands

| Command | Description |
|---|---|
| `:set nu` | Display line numbers |
| `:set nonu` | Hide line numbers |
| `:28` | Move cursor to line number 28 |

### Search Commands

| Command | Description |
|---|---|
| `:/root` | Search for the word `root` forward |
| `:?root` | Search for the word `root` backward |
| `:noh` | Remove search highlighting |

### Delete a Specific Line

```vim
:28d
```

- Deletes line number 28.

### Search and Replace

#### Replace First Occurrence in Line 28
```vim
:28s/root/ROOT
```
- Searches for `root` in line 28.
- Replaces only the first occurrence with `ROOT`.

#### Replace All Occurrences in Line 29
```vim
:29s/root/ROOT/g
```
- Replaces all occurrences of `root` with `ROOT` in line 29.
- `g` means global within that line.

#### Replace All Occurrences in the Entire File
```vim
:%s/root/ROOT/g
```
- `%` represents all lines in the file.
- `s` means substitute.
- `g` means replace all occurrences in each line.

---

## 3. Normal Mode (Esc Mode)

Press `Esc` to enter Normal Mode.

### Navigation Commands

| Command | Description |
|---|---|
| `gg` | Move cursor to the top of the file |
| `Shift + G` (`G`) | Move cursor to the bottom of the file |
| `28G` | Move cursor to line 28 |
| `h` | Move cursor left |
| `j` | Move cursor down |
| `k` | Move cursor up |
| `l` | Move cursor right |

### Editing Commands

| Command | Description |
|---|---|
| `dd` | Delete the current line |
| `yy` | Yank (copy) the current line |
| `p` | Paste below the current line |
| `P` | Paste above the current line |
| `u` | Undo the last change |
| `Ctrl + r` | Redo the undone change |

### Insert Mode Commands

| Command | Description |
|---|---|
| `i` | Insert text before the cursor |
| `a` | Insert text after the cursor |
| `o` | Open a new line below and enter Insert Mode |
| `O` | Open a new line above and enter Insert Mode |
| `Esc` | Return to Normal Mode |

---

## 4. Linux File Viewing Commands

### head Command

The `head` command displays the top lines of a file.

#### Syntax
```bash
head <file-name>
```

- By default, it displays the first 10 lines.

#### Display Specific Number of Lines
```bash
head -n 8 <file-name>
```
- Displays the first 8 lines.

### tail Command

The `tail` command displays the bottom lines of a file.

#### Syntax
```bash
tail <file-name>
```

- By default, it displays the last 10 lines.

#### Display Specific Number of Lines
```bash
tail -n 3 <file-name>
```
- Displays the last 3 lines.

---

## 5. Display a Specific Range of Lines

### Requirement
Display lines 55 to 65 from a file.

### Command
```bash
head -n 65 <file-name> | tail -n 11
```

### Explanation
- `head -n 65` → Displays the first 65 lines.
- `|` → Pipe operator; sends the output of one command to another.
- `tail -n 11` → Displays the last 11 lines from the output.
- Result → Displays lines 55 to 65.

### General Formula
To display lines from line A to line B:

```bash
head -n B <file-name> | tail -n (B-A+1)
```

Example: Display lines 20 to 30.

```bash
head -n 30 <file-name> | tail -n 11
```

---

## 6. Quick Revision

| Task | Command |
|---|---|
| Open a file | `vim file.txt` |
| Save | `:w` |
| Save and quit | `:wq` |
| Quit without saving | `:q!` |
| Display line numbers | `:set nu` |
| Go to top | `gg` |
| Go to bottom | `G` |
| Delete a line | `dd` |
| Copy a line | `yy` |
| Paste below | `p` |
| Undo | `u` |
| Redo | `Ctrl + r` |
| Display first 10 lines | `head file.txt` |
| Display last 10 lines | `tail file.txt` |
| Display lines 55–65 | `head -n 65 file.txt \| tail -n 11` |

## 7. Important Points to Remember

1. Vim opens in Normal Mode by default.
2. Press `i` to enter Insert Mode.
3. Press `Esc` to return to Normal Mode.
4. Use `:wq` to save and quit.
5. Use `:q!` to quit without saving.
6. `dd` deletes a line, whereas `yy` copies a line.
7. `p` pastes below, whereas `P` pastes above.
8. `head` displays lines from the beginning.
9. `tail` displays lines from the end.
10. The pipe (`|`) connects the output of one command to the input of another command.

# LINUX ADMINISTRATION – CLASS NOTES
## Topic: User Management, Groups, SSH & File Permissions

---

# 1. User Management

## Introduction

Linux Administration involves managing users, groups, permissions, and access to system resources.

### Basic Definitions

| Concept | Definition |
|---|---|
| User | A human or account that accesses the Linux system |
| Group | A collection of users |
| Role | A position or responsibility assigned to a user |
| Permissions | Access rights that determine what a user can do |

### User Management Operations

1. Create a user
2. Create a group
3. Add a user to a group
4. Remove a user from a group
5. Remove a user
6. Remove a group

---

# 2. User Creation

## Create a User

### Syntax
```bash
useradd <username>
```

### Example
```bash
useradd suresh
```

- Creates a user named `suresh`.
- By default, it may create a primary group with the same username, depending on the system configuration.
- The home directory is not necessarily created unless specified or configured.

### Create a User with a Home Directory
```bash
useradd -m suresh
```

- `-m` creates the home directory.

## Create a Group

### Syntax
```bash
groupadd <group-name>
```

### Example
```bash
groupadd devops
```

- Creates a group named `devops`.

---

# 3. User and Group Information

## id Command

### Syntax
```bash
id <username>
```

### Example
```bash
id suresh
```

### Output
```text
uid=1001(suresh) gid=1001(suresh) groups=1001(suresh)
```

### Explanation

| Term | Meaning |
|---|---|
| UID | User ID |
| GID | Primary Group ID |
| groups | Groups to which the user belongs |

Note: UID and GID numbers are assigned by Linux and may differ across systems.

---

# 4. Linux User and Group Files

## /etc/passwd

- Stores user account information.
- Contains username, UID, GID, home directory, and login shell.
- Password hashes are not stored here.

### Example
```text
suresh:x:1001:1001::/home/suresh:/bin/bash
```

### Field Explanation

| Field | Meaning |
|---|---|
| suresh | Username |
| x | Password information stored in `/etc/shadow` |
| 1001 | UID |
| 1001 | Primary GID |
| Empty field | User information/comment |
| /home/suresh | Home directory |
| /bin/bash | Login shell |

## /etc/group

- Stores group information.

### Example
```text
devops:x:1002:
```

### Field Explanation

| Field | Meaning |
|---|---|
| devops | Group name |
| x | Password placeholder |
| 1002 | GID |
| Empty field | Supplementary group members listed here |

## /etc/shadow

- Stores password hashes and password aging information.
- Accessible only to privileged users.
- Contains encrypted/hash-based password information rather than plain-text passwords.

---

# 5. Primary Group and Secondary Group

Every normal Linux user has:

- One primary group
- Zero or more secondary (supplementary) groups

## Primary Group

- The default group associated with the user.
- Used as the default group identity for newly created files.

## Secondary Group

- Additional groups assigned to the user.
- Provides additional access to files and resources.

---

# 6. Adding Users to Groups

## Change Primary Group

### Syntax
```bash
usermod -g <group-name> <username>
```

### Example
```bash
usermod -g devops suresh
```

- Changes the primary group of `suresh` to `devops`.

## Add a Secondary Group

### Syntax
```bash
usermod -aG <group-name> <username>
```

### Example
```bash
usermod -aG devops suresh
```

- Adds `suresh` to the secondary group `devops`.
- `-a` means append.
- `-G` specifies supplementary groups.

**Important:** Always use `-aG` when adding a supplementary group. Using `-G` alone can remove the user's existing supplementary group memberships.

---

# 7. Removing Users from Groups

## Remove User from a Supplementary Group

### Syntax
```bash
gpasswd -d <username> <group-name>
```

### Example
```bash
gpasswd -d suresh devops
```

- Removes `suresh` from the supplementary group `devops`.

Note: This does not remove the user from the group if `devops` is the user's primary group.

---

# 8. Password Management

## Set or Change User Password

### Syntax
```bash
passwd <username>
```

### Example
```bash
passwd suresh
```

- Sets or changes the password of `suresh`.
- A privileged administrator can set another user's password.

---

# 9. Removing Users and Groups

## Delete a User

### Syntax
```bash
userdel <username>
```

### Example
```bash
userdel suresh
```

- Deletes the user account.
- Does not normally remove the user's home directory.

### Delete User with Home Directory
```bash
userdel -r suresh
```

- Deletes the user and their home directory.

## Delete a Group

### Syntax
```bash
groupdel <group-name>
```

### Example
```bash
groupdel devops
```

- Deletes the group.
- A group cannot normally be deleted if it is the primary group of an existing user.

---

# 10. SSH Configuration

## SSH

SSH stands for Secure Shell.

- Used to connect to remote Linux systems securely.
- Supports remote login and remote command execution.

## SSH Configuration File

```bash
/etc/ssh/sshd_config
```

- Contains SSH server configuration settings.

## Disable Password Authentication

Edit the configuration file:

```bash
vim /etc/ssh/sshd_config
```

Set:

```text
PasswordAuthentication no
```

- Disables password-based SSH authentication.
- SSH key authentication should be configured and tested before disabling passwords.

## Check SSH Configuration Syntax

```bash
sshd -t
```

- Checks SSH configuration syntax.
- No output usually indicates successful validation.

## Restart SSH Service

```bash
systemctl restart sshd
```

- Restarts the SSH service to apply configuration changes.

Note: On some Linux distributions, the service name is `ssh` instead of `sshd`.

---

# 11. Linux File Permissions

Linux permissions determine who can read, write, and execute files or directories.

## Permission Types

| Permission | Symbol | Numeric Value |
|---|---|---:|
| Read | r | 4 |
| Write | w | 2 |
| Execute | x | 1 |

## Permission Categories

| Category | Symbol | Meaning |
|---|---|---|
| User/Owner | u | File owner |
| Group | g | Group associated with the file |
| Others | o | All other users |
| All | a | User, group, and others |

## Example: File Permissions

```text
-rw-r--r--
```

### Explanation

```text
-    rw-    r--    r--
     owner  group  others
```

| Section | Meaning |
|---|---|
| `-` | Regular file |
| `rw-` | Owner can read and write |
| `r--` | Group can read |
| `r--` | Others can read |

For directories, `x` means permission to enter/traverse the directory.

---

# 12. chmod Command

`chmod` stands for Change Mode.

Used to modify file and directory permissions.

## Symbolic Method

### Add Write Permission to Group

```bash
chmod g+w file.txt
```

- `g` → Group
- `+` → Add permission
- `w` → Write permission

### Remove Write Permission from Others

```bash
chmod o-w file.txt
```

### Add Execute Permission to Owner

```bash
chmod u+x script.sh
```

### Give Everyone Read Permission

```bash
chmod a+r file.txt
```

---

# 13. Numeric (Octal) Permissions

Permissions can also be assigned using numbers.

| Permission | Value |
|---|---:|
| Read | 4 |
| Write | 2 |
| Execute | 1 |
| Read + Write | 6 |
| Read + Execute | 5 |
| Read + Write + Execute | 7 |
| No Permission | 0 |

## Example: chmod 744

```bash
chmod 744 file.txt
```

### Calculation

| Category | Number | Permissions |
|---|---:|---|
| Owner | 7 | rwx |
| Group | 4 | r-- |
| Others | 4 | r-- |

Result:

```text
-rwxr--r--
```

## More Examples

| Command | Permission | Explanation |
|---|---|---|
| `chmod 777 file` | rwxrwxrwx | Everyone has full access |
| `chmod 755 file` | rwxr-xr-x | Owner full access, others read and execute |
| `chmod 644 file` | rw-r--r-- | Owner read/write, others read |
| `chmod 600 file` | rw------- | Only owner can read/write |
| `chmod 700 file` | rwx------ | Only owner has full access |

---

# 14. Practical Lab

## Scenario: Create DevOps Group and Suresh User

### Step 1: Create Group
```bash
groupadd devops
```

### Step 2: Create User
```bash
useradd -m suresh
```

### Step 3: Set Password
```bash
passwd suresh
```

### Step 4: Add Suresh to DevOps Group
```bash
usermod -aG devops suresh
```

### Step 5: Verify Group Membership
```bash
id suresh
```

### Step 6: Remove Suresh from DevOps Group
```bash
gpasswd -d suresh devops
```

### Step 7: Verify Again
```bash
id suresh
```

### Step 8: Remove Suresh User
```bash
userdel -r suresh
```

---

# 15. Quick Revision Table

| Task | Command |
|---|---|
| Create user | `useradd suresh` |
| Create user with home | `useradd -m suresh` |
| Create group | `groupadd devops` |
| Check user details | `id suresh` |
| Change primary group | `usermod -g devops suresh` |
| Add secondary group | `usermod -aG devops suresh` |
| Remove secondary group | `gpasswd -d suresh devops` |
| Set password | `passwd suresh` |
| Delete user | `userdel suresh` |
| Delete user and home | `userdel -r suresh` |
| Delete group | `groupdel devops` |
| SSH configuration | `vim /etc/ssh/sshd_config` |
| Validate SSH config | `sshd -t` |
| Restart SSH | `systemctl restart sshd` |
| Add group write permission | `chmod g+w file.txt` |
| Set permission 744 | `chmod 744 file.txt` |
| View permissions | `ls -l` |

---

# 16. Important Interview Points

1. What is the difference between UID and GID?
2. What is the difference between primary and secondary groups?
3. What is the difference between `usermod -g` and `usermod -aG`?
4. What is the purpose of `/etc/passwd`?
5. What is the purpose of `/etc/shadow`?
6. What is the purpose of `/etc/group`?
7. What is the difference between `userdel` and `userdel -r`?
8. What is the difference between `chmod 755` and `chmod 644`?
9. What is the purpose of `sshd -t`?
10. What is the difference between `chmod` and `chown`?

## Final Memory Trick

- `useradd` → Create user
- `groupadd` → Create group
- `usermod` → Modify user
- `gpasswd -d` → Remove user from supplementary group
- `userdel` → Delete user
- `groupdel` → Delete group
- `passwd` → Password management
- `chmod` → Change permissions
- `chown` → Change ownership

