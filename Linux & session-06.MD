# Linux Class Notes: Process Management, Network Management, Linux Hierarchy, and 3-Tier Architecture

## 1. Process Management

### What is a process?

A **process** is a program that is currently running. Whenever Linux
starts a program, the operating system creates a process and assigns it
a unique **Process ID (PID)**.

-   **PID (Process ID):** Unique identifier assigned to a running
    process.
-   **PPID (Parent Process ID):** PID of the process that started the
    current process.
-   **Parent process:** A process that starts another process.
-   **Child process:** A process started by another process.

### Simple real-world example

Think of a software team:

-   Team Manager
-   Team Lead
-   Senior Engineer
-   Junior Engineer

If the Team Lead starts a task and assigns it to a Senior Engineer, the
Team Lead is like the **parent process**, and the Senior Engineer is
like the **child process**.

Similarly, in Linux, when one process starts another process, the first
process is the parent and the newly started process is the child.

> Note: The analogy helps explain the relationship. Linux PIDs and PPIDs
> are operating-system identifiers, not employee IDs.

### Common process commands

  -----------------------------------------------------------------------
  Command                             Purpose
  ----------------------------------- -----------------------------------
  `ps`                                Displays processes associated with
                                      the current terminal/session.

  `ps -ef`                            Displays processes across the
                                      system in full-format listing.

  `ps -ef \| grep nginx`              Searches the process list for
                                      processes matching `nginx`.

  `top`                               Displays running processes and
                                      their resource usage interactively.

  `sleep 10`                          Runs a process that waits for 10
                                      seconds.

  `sleep 10 &`                        Starts the sleep process in the
                                      background.

  `kill PID`                          Sends the default `SIGTERM` signal
                                      to a process.

  `kill -9 PID`                       Sends `SIGKILL` to forcefully
                                      terminate a process.
  -----------------------------------------------------------------------

Example:

``` bash
ps -ef
```

Typical output:

``` text
UID    PID  PPID  C STIME TTY  TIME CMD
root     1     0  0 01:53  ?   00:00:00 /usr/lib/systemd/systemd
root     2     0  0 01:53  ?   00:00:00 [kthreadd]
```

Important columns:

-   `UID`: User who owns the process.
-   `PID`: Process ID.
-   `PPID`: Parent Process ID.
-   `C`: CPU utilization/scheduling information (field meaning can vary
    by `ps` format).
-   `STIME`: Process start time.
-   `TTY`: Terminal associated with the process, if any.
-   `TIME`: Accumulated CPU time.
-   `CMD`: Command that started the process.

### Foreground and background processes

**Foreground process** - Runs attached to the current terminal. - The
shell normally waits for it to finish before accepting another
command. - Example: `sleep 10`

**Background process** - Runs without keeping the terminal occupied. -
Add `&` at the end of a command to start it in the background. -
Example: `sleep 10 &`

The `&` symbol is called an **ampersand**.

Check sleep processes:

``` bash
ps -ef | grep sleep
```

> Correction: `sleep 10 &` is a background process, not a foreground
> process.

### Monitoring CPU and memory

Use:

``` bash
top
```

`top` helps identify processes consuming high CPU or memory.

Example observations:

  Situation                      Users/Load (illustrative)   CPU   RAM
  ---------------------------- --------------------------- ----- -----
  Normal load                                     10 users   10%   25%
  Increased load                                 100 users   20%   40%
  Possible application issue                      10 users   20%   40%

These numbers are only examples. User count alone does not determine CPU
or memory usage.

### What to do when an application consumes high resources?

1.  Check CPU and memory usage using `top` or `free -h`.
2.  Identify the resource-consuming process.
3.  Check application logs and system logs.
4.  Investigate whether the application has a memory leak, stuck
    process, or inefficient workload.
5.  Restart the application only when appropriate and after considering
    the impact.
6.  If required, restart the server as a last resort, following the
    operational procedure.

Useful commands:

``` bash
top
free -h
ps -ef
journalctl -u nginx
sudo systemctl status nginx
```

A well-behaved application should release resources it no longer needs.
However, high memory usage does not automatically mean there is a memory
leak; investigate before taking action.

------------------------------------------------------------------------

## 2. Linux Service Management

Linux services are commonly managed using `systemctl` on systems that
use systemd.

For the Nginx service:

``` bash
sudo systemctl status nginx
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx
sudo systemctl enable nginx
```

  Command     Purpose
  ----------- --------------------------------------------------------
  `status`    Checks service status.
  `start`     Starts the service.
  `stop`      Stops the service.
  `restart`   Restarts the service.
  `enable`    Configures the service to start automatically at boot.

Check whether Nginx is running:

``` bash
sudo systemctl status nginx
ps -ef | grep nginx
```

If Nginx is serving HTTP traffic, test it using:

``` text
http://<PUBLIC-IP>:80
```

Port `80` is the default HTTP port. The server must also allow inbound
traffic on that port through its firewall and, in AWS, the applicable
security group and network ACL rules.

------------------------------------------------------------------------

## 3. Network Management

Network management in Linux includes checking listening ports, network
connections, routes, and service connectivity.

In AWS, AWS manages the underlying physical infrastructure and network,
while we configure and troubleshoot items such as VPCs, subnets, route
tables, security groups, network ACLs, and the operating system network
settings.

### Check listening ports

Traditional command:

``` bash
sudo netstat -ltnp
```

Modern alternative (recommended on many Linux distributions):

``` bash
sudo ss -ltnp
```

Options:

-   `-l`: Listening sockets.
-   `-t`: TCP sockets.
-   `-n`: Show numeric addresses and port numbers.
-   `-p`: Show the process using the socket (may require `sudo`).

Example:

``` bash
sudo ss -ltnp
```

To check whether Nginx is listening on port 80:

``` bash
sudo ss -ltnp | grep ':80'
```

To test HTTP connectivity locally:

``` bash
curl -I http://localhost:80
```

To test a remote endpoint:

``` bash
curl -I http://<PUBLIC-IP>:80
```

**Important:** A process listening on a port does not guarantee that it
is reachable from the internet. Also check the host firewall, AWS
security group, network ACL, route table, and application health.

------------------------------------------------------------------------

## 4. Linux Filesystem Hierarchy

Linux organizes files and directories in a tree structure starting from
the root directory `/`.

  -----------------------------------------------------------------------
  Directory                           Purpose
  ----------------------------------- -----------------------------------
  `/`                                 Root of the filesystem hierarchy.

  `/home`                             Home directories of regular users.

  `/root`                             Home directory of the root user.

  `/etc`                              System and application
                                      configuration files.

  `/var`                              Variable data such as logs, cache,
                                      and spool files.

  `/var/log`                          System and application logs.

  `/tmp`                              Temporary files.

  `/usr`                              User-space programs, libraries, and
                                      shared resources.

  `/bin`                              Essential user command binaries
                                      (often linked to `/usr/bin`).

  `/sbin`                             System administration binaries
                                      (often linked to `/usr/sbin`).

  `/opt`                              Optional/add-on application
                                      software.

  `/boot`                             Files needed to boot the system.

  `/dev`                              Device files.

  `/proc`                             Virtual filesystem exposing process
                                      and kernel information.

  `/sys`                              Virtual filesystem exposing device
                                      and kernel information.

  `/mnt`                              Common temporary mount point.

  `/media`                            Common mount point for removable
                                      media.
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## 5. Soft Links, Hard Links, and Inodes

### Inode

An **inode** is a filesystem data structure that stores metadata about a
file, such as ownership, permissions, timestamps, size, and pointers to
the file's data blocks.

The filename is stored in a directory entry, which maps the name to an
inode number.

Check inode numbers:

``` bash
ls -i file.txt
```

### Hard link

A hard link is another directory entry that points to the same inode as
the original file.

``` bash
ln original.txt hardlink.txt
```

Key points: - Both names refer to the same underlying file data and
inode. - Changes made through either name affect the same file. -
Removing one name does not remove the data while another hard link
remains. - Hard links generally cannot cross filesystem boundaries. -
Creating hard links to directories is normally restricted.

### Soft link (symbolic link)

A soft link is a separate file that stores a path to another file or
directory.

``` bash
ln -s original.txt softlink.txt
```

Key points: - It has its own inode. - It points to a pathname. - It can
point across filesystem boundaries. - It can point to directories. - If
the target is moved or deleted, the symbolic link may become broken.

### Hard link vs. soft link

  -----------------------------------------------------------------------
  Feature                 Hard link               Soft link
  ----------------------- ----------------------- -----------------------
  Inode                   Same inode as target    Different inode

  Points to               File's inode            Target path

  Cross-filesystem        No                      Yes

  Directory link          Normally restricted     Supported

  Target deleted          Data remains if another Link may become broken
                          hard link exists        
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## 6. Forward Proxy vs. Reverse Proxy

### Forward proxy

A forward proxy acts on behalf of **clients** when they access external
services.

Example: - Employees access the internet through a company proxy. - The
proxy can enforce access policies, filtering, and logging. - External
websites generally see the proxy as the source of the request.

### Reverse proxy

A reverse proxy acts on behalf of **servers** and receives client
requests before forwarding them to backend servers.

Example: - Nginx receives incoming HTTP requests. - It forwards requests
to one or more application servers. - It can provide TLS termination,
routing, caching, and load balancing.

  -----------------------------------------------------------------------
  Forward proxy                       Reverse proxy
  ----------------------------------- -----------------------------------
  Represents clients                  Represents backend servers

  Controls or mediates outbound       Handles inbound client requests
  access                              

  Common in corporate internet access Common in web application
                                      architectures
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## 7. Application Architecture: 1-Tier, 2-Tier, and 3-Tier

### Desktop application vs. web application

**Desktop application** - Installed and executed on a user's computer. -
Uses local machine resources. - Updates and maintenance may need to be
performed on individual systems. - Local data may be at risk if the
device fails and data is not backed up. - Usually depends on the
operating system and device where it is installed.

**Web application** - Accessed through a browser over a network. -
Application logic and data are commonly hosted on servers. - Centralized
deployment makes updates easier to manage. - Can be accessed from
different devices, subject to authentication and network access. -
Security and availability depend on proper server-side controls and
infrastructure.

### 1-tier architecture

All major application components are hosted on a single system.

Example: - UI - Application logic - Database

A small roadside hotel is a useful analogy: one person takes orders,
cooks, and collects payment.

``` text
User Interface + Application + Database
              |
         Single System
```

### 2-tier architecture

The application is separated into two layers, commonly a
client/application layer and a database layer.

Example: - Client or application communicates directly with the
database. - A small hotel has one person managing orders and another
person cooking.

``` text
Client / Application
          |
       Database
```

### 3-tier architecture

The application is divided into three logical tiers:

1.  **Presentation/Web tier:** Receives user requests and serves the UI.
2.  **Application tier:** Executes business logic and processes
    requests.
3.  **Database tier:** Stores and retrieves application data.

Restaurant analogy:

-   **Captain:** Welcomes guests and directs them to a table.
-   **Waiter:** Takes orders and delivers prepared food.
-   **Chef:** Prepares the food.

Technical mapping:

-   Captain → Load balancer / request entry point
-   Waiter → Web or presentation tier (depending on the design)
-   Chef → Application/business logic tier
-   Database → Stores the raw data used by the application

A typical web architecture can look like this:

``` text
Users / Browsers
       |
       v
Load Balancer (ALB)
       |
       v
Web Tier (Nginx)
       |
       v
Application Tier
(Java / Python / .NET / Node.js / Go)
       |
       v
Database Tier
(MySQL / PostgreSQL / Oracle / MongoDB)
```

> Architecture varies by application. A load balancer is an
> infrastructure component and is not necessarily counted as a separate
> application tier. Nginx can also act as a reverse proxy, and in some
> designs it can serve static content directly.

### Why do we use 3-tier architecture?

-   **Separation of concerns:** Each tier has a clear responsibility.
-   **Loose coupling:** One tier can be changed with limited impact on
    other tiers when interfaces remain compatible.
-   **Scalability:** Web and application servers can be scaled
    independently.
-   **Maintainability:** Easier to troubleshoot and update individual
    layers.
-   **Security:** Database access can be restricted to the application
    tier.

This separation is also called **decoupling** or **loose coupling**.

------------------------------------------------------------------------

## 8. Web, Application, and Database Tiers

### Web / Frontend / Presentation tier

Responsibilities: - Receives HTTP/HTTPS requests. - Serves frontend
content such as HTML, CSS, and JavaScript. - Routes requests to
application servers. - May perform reverse proxying and load balancing.

Common technologies: - Nginx - Apache HTTP Server

### Application / Backend / Middleware tier

Responsibilities: - Implements business logic. - Processes user
requests. - Performs CRUD operations. - Communicates with databases and
other services.

Common technologies: - Java - .NET - Python - C++ - Go (Golang) -
Node.js

### Database tier

Responsibilities: - Stores and retrieves application data. - Manages
queries, transactions, and data consistency. - Provides persistence and
backup/recovery capabilities.

Examples:

**Relational databases (SQL)** - MySQL - Oracle Database - Microsoft SQL
Server - PostgreSQL

**NoSQL databases** - MongoDB - Apache Cassandra

**In-memory data store / cache** - Redis

**Messaging systems** - ActiveMQ - IBM MQ (formerly WebSphere MQ)

> Redis is commonly used as an in-memory data store or cache. ActiveMQ
> and IBM MQ are messaging systems; they are not relational or NoSQL
> databases.

### CRUD operations

CRUD represents the four basic operations performed on application data:

  Operation   Meaning                Example
  ----------- ---------------------- -------------------
  Create      Insert new data        Register a user
  Read        Retrieve data          View user details
  Update      Modify existing data   Update an address
  Delete      Remove data            Delete a record

------------------------------------------------------------------------

## 9. Interview Quick Revision

1.  **What is a process?** A process is a program in execution.
2.  **What is PID?** PID is the unique ID assigned to a running process.
3.  **What is PPID?** PPID is the ID of the parent process that started
    a child process.
4.  **How do you list all processes?** Use `ps -ef`.
5.  **How do you monitor process resource usage?** Use `top`.
6.  **How do you start a process in the background?** Add `&` at the
    end, for example `sleep 10 &`.
7.  **How do you terminate a process gracefully?** Use `kill PID`, which
    normally sends `SIGTERM`.
8.  **How do you forcefully terminate a process?** Use `kill -9 PID`,
    which sends `SIGKILL`.
9.  **How do you check Nginx status?** Use
    `sudo systemctl status nginx`.
10. **How do you check listening TCP ports?** Use `sudo ss -ltnp`.
11. **What is an inode?** It stores file metadata and identifies the
    file within a filesystem.
12. **What is a hard link?** Another filename that points to the same
    inode.
13. **What is a soft link?** A separate link that points to a pathname.
14. **What is a reverse proxy?** A server that receives client requests
    and forwards them to backend servers.
15. **What is 3-tier architecture?** An architecture that separates
    presentation, application logic, and data storage.
16. **Why use 3-tier architecture?** It improves separation,
    maintainability, security, and independent scalability.
17. **What is CRUD?** Create, Read, Update, and Delete.

------------------------------------------------------------------------

## 10. Hands-on Practice

Run these commands on a Linux test machine:

``` bash
# 1. View processes
ps
ps -ef

# 2. Search for Nginx
ps -ef | grep nginx

# 3. Start a background process
sleep 60 &

# 4. Find the sleep process
ps -ef | grep sleep

# 5. Monitor processes
top

# 6. Check Nginx service
sudo systemctl status nginx

# 7. Check listening ports
sudo ss -ltnp

# 8. Check HTTP response locally
curl -I http://localhost:80

# 9. Create a hard link and a symbolic link
touch original.txt
ln original.txt hardlink.txt
ln -s original.txt softlink.txt

# 10. Compare inode numbers
ls -li original.txt hardlink.txt softlink.txt
```

**Practice safely:** Use a test machine. Before killing a process or
restarting a service, verify its PID and understand what depends on it.
