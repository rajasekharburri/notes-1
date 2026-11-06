# Linux & DevOps Class Notes -- AMI, Linux, RHEL, Database, and Application Tier

## 1. Process and Network Management -- Quick Revision

### Process commands

  -----------------------------------------------------------------------
  Command                             Purpose
  ----------------------------------- -----------------------------------
  `ps -ef`                            Lists processes across the system
                                      in full-format output.

  `top`                               Displays a live, interactive view
                                      of processes and CPU/memory usage.

  `kill PID`                          Sends `SIGTERM` by default,
                                      requesting graceful process
                                      termination.

  `kill -9 PID`                       Sends `SIGKILL`, forcing
                                      termination without allowing
                                      cleanup.
  -----------------------------------------------------------------------

Examples:

``` bash
ps -ef
top
kill 1234
kill -9 1234
```

**Important:** Prefer `kill PID` first. Use `kill -9 PID` only if the
process does not stop and you understand the impact.

### Check listening ports

``` bash
sudo netstat -lntp
```

This displays listening TCP ports, numeric addresses/ports, and (where
permissions allow) the process and PID using each socket.

Modern alternative:

``` bash
sudo ss -lntp
```

Options: - `-l`: Listening sockets - `-n`: Numeric addresses and port
numbers - `-t`: TCP sockets - `-p`: Process using the socket

> A listening port does not automatically mean it is reachable
> externally. Firewalls, cloud security groups, network ACLs, routes,
> and application health also matter.

------------------------------------------------------------------------

## 2. CRUD Operations

CRUD represents the four basic operations used to manage data.

  Operation   Meaning                  Example
  ----------- ------------------------ ----------------------------------
  Create      Add new data             Create a customer account
  Read        Retrieve existing data   View customer details
  Update      Modify existing data     Change a customer's phone number
  Delete      Remove data              Delete a customer record

------------------------------------------------------------------------

## 3. Application Request Flow

A common web application request flow is:

``` text
User / Browser
      |
      v
Load Balancer
      |
      v
Web Server
      |
      v
Application Server
      |
      v
Database Server
```

The user sends a request to the load balancer. The request is routed to
a web server, then to the application tier, which reads or writes data
in the database when required.

The response generally travels back through the application and web
tiers to the user.

**Remember:** The request direction is:

**User → Load Balancer → Web → Application → Database**

The database is not normally the first component contacted by the user.

------------------------------------------------------------------------

## 4. What Is an Image and What Is an AMI?

### What is an image?

In computing, an image is a prepared template used to create a machine
or system with a known configuration.

For a virtual machine, an image commonly contains: - An operating
system - Required system files and configuration - Installed software
and dependencies - Sometimes application code and other preconfigured
settings

An image is not necessarily a complete copy of everything around a
computer; it is a template or snapshot of a system's software and
configuration.

### What is an AMI?

**AMI (Amazon Machine Image)** is an AWS template used to launch EC2
instances.

An AMI can include: - Operating system - Preinstalled packages and
software - Configuration - Application components, if prepared in the
image

Example:

``` text
AMI
 |
 +-- Operating System
 |
 +-- Required Software
 |
 +-- Configuration
 |
 +-- Optional Application Components
```

If an organization creates an AMI named `devops-practice`, it can use
that AMI to launch multiple EC2 instances with a similar baseline
configuration.

### Why do we use AMIs?

-   Launch EC2 instances consistently.
-   Create reusable server templates.
-   Scale applications by launching more instances.
-   Restore or recreate servers using a known image.
-   Maintain standard operating system and software configurations.

**Interview answer:** An AMI is a template that contains the information
required to launch an EC2 instance, including its operating system and
configured software.

------------------------------------------------------------------------

## 5. Linux: Operating System or Kernel?

### Linux

Linux is technically a **kernel**, not a complete operating system by
itself.

The Linux kernel: - Manages CPU scheduling and processes. - Manages
memory. - Communicates with hardware through device drivers. - Provides
core services that applications use. - Manages system resources.

### What is a kernel?

A kernel is the core component of an operating system. It acts as the
interface between software and the computer's hardware.

``` text
User Applications
       |
System Utilities / Libraries
       |
Linux Kernel
       |
Hardware (CPU, RAM, Disk, Network)
```

### What is an operating system?

An operating system combines the kernel with system utilities,
libraries, services, and other components needed to operate and manage a
computer.

A Linux distribution commonly includes: - Linux kernel - System
utilities - Package manager - Libraries - Service manager - Shell and
other user tools

Examples of Linux distributions: - Red Hat Enterprise Linux (RHEL) -
CentOS Stream - Fedora - Ubuntu - Debian - SUSE Linux Enterprise -
openSUSE - AlmaLinux - Rocky Linux - Amazon Linux

### Who created Linux?

Linus Torvalds created the Linux kernel and released its first version
in 1991. The kernel is primarily written in C, with some
architecture-specific code written in assembly and other languages.

Linus Torvalds also created Git, the distributed version control system.

### Hardware and operating system examples

**Mac computer**

``` text
Apple Hardware + macOS
```

**Dell computer**

``` text
Dell Hardware + Operating System
```

A Dell computer can run different supported operating systems, such as
Windows or Linux. The hardware and operating system are separate
components.

Historically, Unix systems were widely used on servers. Linux later
became widely adopted for server workloads.

------------------------------------------------------------------------

## 6. Open Source vs. Enterprise Linux

### Open-source software

-   Source code is available under an open-source license.
-   Users and organizations can inspect, modify, and redistribute it
    according to the license.
-   Community projects often rely on community support.
-   Support and response times vary by project and service provider.

### Enterprise Linux

-   Designed and maintained for business and production environments.
-   Usually includes vendor support, tested updates, security
    advisories, and defined maintenance lifecycles.
-   Organizations pay for subscriptions or support services, depending
    on the vendor and product.

A useful simplified comparison:

  -----------------------------------------------------------------------
  Open-source/community distribution  Enterprise distribution
  ----------------------------------- -----------------------------------
  Community-driven support may be     Vendor-backed support may be
  available                           available

  No-cost options are common          Subscription or support fees may
                                      apply

  Fix and support timelines vary      Support commitments depend on the
                                      contract
  -----------------------------------------------------------------------

**Important:** Open source does not always mean free of cost, and paid
enterprise software is not simply identical to community software plus
support. Packaging, testing, certifications, lifecycle, and support can
differ.

### Red Hat Enterprise Linux

RHEL is an enterprise Linux distribution from Red Hat. It includes the
Linux kernel, system utilities, libraries, package management, and
enterprise lifecycle/support offerings.

### Understand the Linux distribution relationships

These distributions are related, but they are not all identical:

-   **Fedora:** Community-driven distribution that often introduces
    technologies used in the Red Hat ecosystem.
-   **RHEL:** Red Hat's enterprise Linux distribution.
-   **CentOS Stream:** A continuously delivered distribution that tracks
    ahead of the next RHEL minor release.
-   **AlmaLinux and Rocky Linux:** Enterprise Linux-compatible
    distributions.
-   **Amazon Linux:** AWS's own Linux distribution, optimized for AWS
    environments. It is related to the broader RPM/Fedora ecosystem, but
    it should not be described as exactly the same as RHEL, CentOS, or
    Fedora.
-   **Ubuntu:** A Debian-based Linux distribution, not part of the Red
    Hat family.
-   **SUSE Linux Enterprise:** A separate enterprise Linux distribution
    family.

### Red Hat family and RPM-based commands

Many Red Hat-family distributions use RPM packages and tools such as
`dnf` or `yum`.

Examples:

``` bash
cat /etc/os-release
cat /etc/redhat-release
```

Use these commands to identify the operating system and its version
before following installation instructions.

------------------------------------------------------------------------

## 7. Connecting to an AWS EC2 Linux Instance

For many Amazon Linux EC2 instances, the default SSH username is:

``` text
ec2-user
```

For Ubuntu EC2 instances, the default username is commonly:

``` text
ubuntu
```

The username depends on the AMI.

### SSH connection

``` bash
ssh -i <private-key.pem> ec2-user@<PUBLIC-IP>
```

Example with placeholders:

``` bash
ssh -i devops-practice.pem ec2-user@<EC2-PUBLIC-IP>
```

The private key must be protected. On Linux/macOS, you may need to
restrict its permissions:

``` bash
chmod 400 devops-practice.pem
```

Before connecting, check: - The EC2 instance is running. - You are using
the correct public IP or reachable private IP. - The correct username is
used for the AMI. - The security group allows inbound SSH (TCP port 22)
from your trusted IP. - The private key matches the key pair configured
for the instance.

**Security note:** Never store private keys or plaintext passwords in
class notes, Git repositories, scripts, or shared documents. If a real
password was shared in notes or chat, rotate it.

------------------------------------------------------------------------

## 8. Database Fundamentals

### What is a database?

A database is an organized collection of data that can be stored,
retrieved, updated, and managed.

### What is a database server?

A database server is a machine or compute instance that runs database
software and provides database services to clients or applications.

The word **server** can refer to: - Physical server: A physical
computer. - Virtual server: A virtual machine. - Cloud server: A cloud
compute instance, such as an EC2 instance.

A database server needs database software installed and configured.

### What is an RDBMS?

**RDBMS** stands for **Relational Database Management System**.

It stores data in tables made up of rows and columns. Relationships can
be established between tables using keys.

Excel provides a simple analogy: - Excel workbook → Similar to a
database (only as an analogy) - Worksheet → Similar to a table - Row →
Record - Column → Attribute/field

An RDBMS provides database-specific features such as constraints,
indexes, transactions, and SQL queries; it is much more than a
spreadsheet.

### Example of relational tables

**customers**

    customer_id name    city
  ------------- ------- -----------
            101 Ravi    Hyderabad
            102 Priya   Chennai

**orders**

    order_id   customer_id   amount
  ---------- ------------- --------
         501           101     1500
         502           102     2200

Here, `customer_id` in the `orders` table can reference `customer_id` in
the `customers` table. This relationship connects orders to customers.

### Examples of RDBMS products

-   MySQL
-   Microsoft SQL Server (MSSQL)
-   Oracle Database
-   PostgreSQL

### MySQL

MySQL is a relational database management system.

The default MySQL TCP port is:

``` text
3306
```

The administrative account is commonly named:

``` text
root
```

The root account's password and authentication method depend on how
MySQL was installed and configured. Set secure credentials during setup.

------------------------------------------------------------------------

## 9. Database Connection Details

To connect an application or client to a database, you generally need:

1.  Hostname or IP address
2.  Port number
3.  Protocol
4.  Database name (where applicable)
5.  Username
6.  Password or another authentication method

Example connection details:

``` text
Host:       <DATABASE-IP>
Port:       3306
Protocol:   TCP
Username:   <DB-USERNAME>
Password:   <DB-PASSWORD>
Database:   <DATABASE-NAME>
```

A website login is a different example: a browser may use HTTPS over TCP
port 443 to connect to a website. The website then handles user
authentication. Do not confuse a website login with a database
connection.

### MySQL login

Recommended interactive form:

``` bash
mysql -u root -p
```

The client prompts for the password.

You may also see:

``` bash
mysql -u root -p'<PASSWORD>'
```

There must be no space between `-p` and the password when using that
form. However, passing a password directly on the command line is not
recommended because it may be exposed through shell history or process
information. Prefer the interactive prompt.

### Secure MySQL setup

A common hardening utility is:

``` bash
sudo mysql_secure_installation
```

The available options and exact workflow vary by MySQL version and
distribution. Use the prompts to apply appropriate security settings.

------------------------------------------------------------------------

## 10. Database Server Setup -- Conceptual Flow

A typical database server setup follows this sequence:

``` text
Linux EC2 Instance
       |
       v
Install Database Server Software
       |
       v
Configure Database Service
       |
       v
Create Database / Schema
       |
       v
Create Tables
       |
       v
Insert and Manage Data
```

For example:

-   Linux server: Hosts the database software.
-   Database software: MySQL.
-   Database/schema: `expenses`.
-   Tables: `users`, `products`, `transactions`, etc.
-   Data: Actual records stored in the tables.

**Terminology note:** In MySQL, `CREATE DATABASE` creates a database.
The term *schema* is often used interchangeably with database in MySQL,
though terminology differs across database products.

------------------------------------------------------------------------

## 11. Application Tier -- Node.js Setup

The application tier contains the application code and business logic.
It communicates with the web tier and database tier.

Common application technologies include: - Java - Python - Node.js -
.NET - Go (Golang) - C++

### What is Node.js?

Node.js is a JavaScript runtime that allows JavaScript code to run
outside the browser, commonly on application servers.

### What is DNF?

`dnf` is a package manager used on modern Fedora and many Red Hat-family
Linux distributions.

It is the successor to `yum`. On some systems, `yum` remains available
as a compatibility command or alias.

Examples:

``` bash
dnf --version
sudo dnf install nodejs -y
```

### Node.js module streams

On distributions that support DNF module streams, you may see commands
such as:

``` bash
sudo dnf module list nodejs
sudo dnf module disable nodejs -y
sudo dnf module enable nodejs:20 -y
sudo dnf install nodejs -y
```

Explanation: - `dnf module list nodejs`: Lists available Node.js module
streams. - `dnf module disable nodejs -y`: Disables the default module
stream configuration. - `dnf module enable nodejs:20 -y`: Enables the
Node.js 20 stream, if that stream is available. -
`dnf install nodejs -y`: Installs Node.js from the configured
repositories.

**Important:** These module commands are distribution- and
repository-dependent. Not every Linux distribution, including every
Amazon Linux release, supports DNF module streams. First check the OS
version and available repositories. On systems without module streams,
use the package source recommended for that distribution.

Verify installation:

``` bash
node --version
npm --version
```

------------------------------------------------------------------------

## 12. Linux Service Accounts and Application Directories

### Create a service account

``` bash
sudo useradd expense
```

This creates a Linux user named `expense`, subject to the system's
default user-creation settings.

A service account is a user account used by an application or service
rather than a human user.

Examples: - `expense` - `nginx` - `mysql` - `jenkins`

Why use a service account? - Run an application without using the root
account. - Give the application only the permissions it needs. -
Separate application ownership and access. - Improve security and
operational control.

**Note:** A plain `useradd` command may create a regular login-capable
account depending on the distribution and options. For a non-login
service account, administrators often use options such as:

``` bash
sudo useradd --system --create-home --shell /sbin/nologin expense
```

The appropriate home-directory and shell settings depend on the
application.

### Create an application directory

``` bash
sudo mkdir -p /app
```

The `-p` option creates parent directories if needed and does not report
an error if the directory already exists.

Set ownership for the application account:

``` bash
sudo chown expense:expense /app
```

Check the directory:

``` bash
ls -ld /app
```

Avoid granting broad permissions such as `777`. Give the application
only the access it requires.

------------------------------------------------------------------------

## 13. Common Commands -- Quick Reference

  -----------------------------------------------------------------------
  Command                             Purpose
  ----------------------------------- -----------------------------------
  `ps -ef`                            List system processes

  `top`                               Monitor live process activity

  `kill PID`                          Request graceful process
                                      termination

  `kill -9 PID`                       Forcefully terminate a process

  `sudo ss -lntp`                     Display listening TCP ports and
                                      processes

  `cat /etc/os-release`               Identify Linux distribution and
                                      version

  `ssh -i key.pem user@host`          Connect to a remote Linux server

  `mysql -u root -p`                  Log in to MySQL interactively

  `dnf install nodejs -y`             Install Node.js where available in
                                      configured repositories

  `useradd expense`                   Create a user account

  `mkdir -p /app`                     Create an application directory

  `chown expense:expense /app`        Change directory ownership
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## 14. Interview Quick Revision

1.  **What is an AMI?** An AMI is an AWS template used to launch EC2
    instances with a defined operating system and software
    configuration.
2.  **What is Linux?** Linux is technically a kernel. A Linux
    distribution combines the kernel with utilities, libraries, and
    applications.
3.  **What is a kernel?** The kernel manages hardware resources and
    provides core services to software.
4.  **Who created Linux?** Linus Torvalds created the Linux kernel.
5.  **What is RHEL?** Red Hat Enterprise Linux is an enterprise Linux
    distribution provided by Red Hat.
6.  **What is the difference between community and enterprise Linux?**
    Enterprise offerings commonly provide vendor-backed support, tested
    updates, and defined maintenance lifecycles.
7.  **What is an RDBMS?** A relational database management system stores
    data in related tables consisting of rows and columns.
8.  **What is MySQL's default port?** TCP port 3306.
9.  **What is the MySQL root user?** It is a commonly used
    administrative account in MySQL.
10. **What is CRUD?** Create, Read, Update, and Delete.
11. **What is DNF?** DNF is a package manager used by Fedora and many
    modern RPM-based Linux distributions.
12. **What is a service account?** A user account used to run an
    application or service rather than for normal human login.
13. **Why should applications not run as root?** Running with limited
    privileges reduces the impact of application compromise or mistakes.
14. **What is the normal request flow in a 3-tier web application?**
    User → Load Balancer → Web Tier → Application Tier → Database Tier.

------------------------------------------------------------------------

## 15. Hands-on Practice

Run these on a test EC2 instance. Adapt commands to the Linux
distribution and avoid using production credentials.

``` bash
# Identify the Linux distribution
cat /etc/os-release

# View processes and resource usage
ps -ef
top

# Check listening TCP ports
sudo ss -lntp

# Check the installed package manager
dnf --version

# Check whether Node.js is installed
node --version
npm --version

# Create a service account (basic example)
sudo useradd expense

# Create an application directory
sudo mkdir -p /app

# Assign ownership
sudo chown expense:expense /app

# Verify directory ownership
ls -ld /app
```

**Before running setup commands:** Confirm the operating system, package
repositories, application requirements, and whether the command is
appropriate for that Linux release.
