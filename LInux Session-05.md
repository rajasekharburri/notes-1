# Linux Administration -- Class Notes

## 1. File Ownership and Permissions

### `chmod` -- Change file permissions

-   The **file owner** or the **root user** can change a file's
    permissions.
-   Example:

``` bash
chmod 755 file
chmod 700 directory
```

### `chown` -- Change file ownership

-   `chown` changes the owner and/or group of a file or directory.
-   Normally, only the **root user** (or a user with suitable
    privileges) can change a file's ownership.

``` bash
chown <user>:<group> file
chown <user>:<group> directory
```

### Recursive ownership change

Use `-R` to apply ownership changes recursively to all files and
subdirectories.

``` bash
chown -R <user>:<group> directory
```

------------------------------------------------------------------------

## 2. SSH Login Using Public-Key Authentication

SSH key-based authentication allows a user to log in to a server without
entering the account password each time.

### Important SSH files

For a user named `suresh`:

``` text
/home/suresh/.ssh/
└── authorized_keys
```

-   `authorized_keys` contains the **public key** of each client allowed
    to log in as that user.
-   The user keeps their **private key** secure on their own computer.
    It should never be shared.

### Generate an SSH key pair

An employee can generate a key pair on their local machine:

``` bash
ssh-keygen -f ramesh
```

This creates:

-   `ramesh` -- private key; keep it secret.
-   `ramesh.pub` -- public key; share this with the server
    administrator.

### Configure key-based access for a user

Example: granting SSH access to `sivakumar`.

**Step 1: Create the user**

``` bash
useradd sivakumar
```

**Step 2: Create the SSH directory**

``` bash
mkdir -p /home/sivakumar/.ssh
```

**Step 3: Set directory permissions**

``` bash
chmod 700 /home/sivakumar/.ssh
```

`700` means only the owner has read, write, and execute permissions.

**Step 4: Set directory ownership**

``` bash
chown sivakumar:sivakumar /home/sivakumar/.ssh
```

**Step 5: Create the `authorized_keys` file**

``` bash
touch /home/sivakumar/.ssh/authorized_keys
chmod 600 /home/sivakumar/.ssh/authorized_keys
chown sivakumar:sivakumar /home/sivakumar/.ssh/authorized_keys
```

`600` means only the owner can read and write the file.

**Step 6: Add the employee's public key**

The employee sends the contents of `sivakumar.pub` to the administrator.
The administrator adds the complete public-key line to:

``` text
/home/sivakumar/.ssh/authorized_keys
```

For example, the administrator can paste the key into the file using a
text editor. Do not place the private key in `authorized_keys`.

**Step 7: Connect to the server**

From the employee's local machine, using the private key:

``` bash
ssh -i sivakumar sivakumar@3.87.244.153
```

The private key file should be protected and readable only by its owner.
If needed:

``` bash
chmod 600 sivakumar
```

------------------------------------------------------------------------

## 3. Sudo Access

### Sudoers configuration file

``` text
/etc/sudoers
```

This file controls which users and groups can run commands with elevated
privileges.

Use `visudo` to edit the sudoers configuration safely. It checks the
syntax before saving.

``` bash
visudo
```

> Do not edit `/etc/sudoers` with a regular text editor unless you know
> how to validate it. A syntax error can break sudo access.

### Give a user sudo access through the `wheel` group

On many RHEL-based systems, the `wheel` group is used to grant sudo
privileges.

**Step 1: Add the user to the wheel group**

``` bash
usermod -aG wheel sivakumar
```

-   `-a` means append the group.
-   `-G` specifies supplementary groups.
-   Using `-aG` avoids removing the user from other supplementary
    groups.

**Step 2: Configure the wheel group**

Run:

``` bash
visudo
```

Find or add the appropriate rule:

``` text
%wheel ALL=(ALL) ALL
```

This allows members of `wheel` to run commands using sudo, normally with
password authentication.

To allow members of `wheel` to use sudo **without a password**, use:

``` text
%wheel ALL=(ALL) NOPASSWD: ALL
```

Passwordless sudo should be granted only when required, because it gives
users significant administrative power.

------------------------------------------------------------------------

## 4. Network Ports

A TCP or UDP port number is a 16-bit number.

-   Port range: **0--65,535**
-   Ports **0--1,023** are commonly known as well-known or system ports.
-   Common examples:
    -   HTTP -- `80`
    -   HTTPS -- `443`
    -   SSH -- `22`

A service listens on a port so that clients can connect to it.

------------------------------------------------------------------------

## 5. How a Browser Connects to a Website

Example:

``` text
https://facebook.com
```

-   `https` -- protocol used to communicate securely.
-   `facebook.com` -- domain name.
-   DNS resolves the domain name to an IP address.
-   HTTPS normally uses port `443`.
-   The browser connects to the server and requests the web application.
-   After successful authentication, the application can show the user's
    home feed.

A simplified flow:

``` text
Browser
   |
   |  https://facebook.com
   v
DNS lookup
   |
   v
Server IP address
   |
   |  HTTPS connection (port 443)
   v
Web application
   |
   v
Response displayed in browser
```

### Analogy: Sending a courier

Think of sending a courier from Delhi to Hyderabad:

-   City/address -- identifies where the courier should go.
-   Building/apartment -- identifies the destination within that area.
-   Flat number -- identifies the exact recipient.

Similarly, network communication uses addressing information (such as an
IP address) and a port number to reach the intended service on a
computer.

Example address from class:

``` text
Gandhi Apartments,
Ameerpet, Hyderabad – 500082
Flat No. 102
```

------------------------------------------------------------------------

## 6. Package Management

A **package manager** is a tool used to install, remove, update, and
query software packages on a Linux server.

### RHEL-based distributions

RHEL and related distributions use `dnf` (the successor to `yum`).

``` bash
dnf install <package-name>
dnf remove <package-name>
dnf update <package-name>
dnf info <package-name>
```

Examples:

``` bash
dnf install nginx
dnf remove nginx
dnf info nginx
```

### Ubuntu-based distributions

Ubuntu uses APT.

``` bash
apt-get install <package-name>
apt-get remove <package-name>
apt-get update
apt-get upgrade
```

### Repository configuration

On RHEL-based systems, repository configuration files are commonly
stored in:

``` text
/etc/yum.repos.d/
```

### List packages

``` bash
dnf list installed
```

Lists packages already installed on the server.

``` bash
dnf list available
```

Lists packages available from enabled repositories that are not
currently installed.

------------------------------------------------------------------------

## 7. Service Management with `systemctl`

A **service** is a background process managed by the operating system.

Example: **Nginx** is a web server that can serve web pages and
application content.

### Start Nginx

``` bash
systemctl start nginx
```

### Check service status

``` bash
systemctl status nginx
```

### Stop Nginx

``` bash
systemctl stop nginx
```

### Restart Nginx

``` bash
systemctl restart nginx
```

### Enable Nginx at boot

``` bash
systemctl enable nginx
```

This configures Nginx to start automatically when the system boots.

### Disable automatic startup

``` bash
systemctl disable nginx
```

This prevents Nginx from being started automatically at boot. It does
not necessarily stop a service that is already running.

### Test the web server

If the server is reachable and its firewall allows HTTP traffic, open:

``` text
http://3.87.244.153:80
```

HTTP normally uses port `80`. The `:80` can be omitted because it is the
default HTTP port:

``` text
http://3.87.244.153
```

------------------------------------------------------------------------

## 8. Quick Revision

  Topic                          Command / Important detail
  ------------------------------ --------------------------------
  Change permissions             `chmod`
  Change ownership               `chown`
  Recursive ownership            `chown -R`
  SSH public keys                `authorized_keys`
  SSH private key login          `ssh -i <private-key> user@IP`
  SSH directory permission       `700`
  `authorized_keys` permission   `600`
  Safely edit sudoers            `visudo`
  Add user to wheel              `usermod -aG wheel <user>`
  Port range                     `0–65,535`
  HTTP / HTTPS / SSH             `80` / `443` / `22`
  RHEL package manager           `dnf`
  Ubuntu package manager         `apt` / `apt-get`
  Repository directory (RHEL)    `/etc/yum.repos.d/`
  Start a service                `systemctl start <service>`
  Check service                  `systemctl status <service>`
  Enable at boot                 `systemctl enable <service>`
  Disable at boot                `systemctl disable <service>`
