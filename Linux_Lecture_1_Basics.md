# Linux Basics — Lecture 1

## Beginner-Friendly Linux OS Notes

> **Lecture Goal:** Understand what Linux is, where it came from, why it is important, where it is used, and why Linux is especially important for DevOps, Cloud, and IT careers.

---

# 1. What is an Operating System?

An **Operating System (OS)** is system software that acts as an interface between the **user/applications and computer hardware**.

### Simple flow

```text
User
  ↓
Application
  ↓
Operating System
  ↓
Hardware
```

### Examples of Operating Systems

- Windows
- macOS
- Linux
- Android
- iOS

### What does an OS do?

An operating system manages:

- CPU
- RAM / Memory
- Storage
- Files and directories
- Processes
- Devices
- Network resources
- Users and permissions
- Applications

### Simple example

When you open Chrome:

```text
You
 ↓
Chrome
 ↓
Linux / Windows / macOS
 ↓
CPU + RAM + Storage + Network
```

The application does not directly control every piece of hardware. The OS manages these resources.

---

# 2. What is Linux?

**Linux is an open-source operating system family built around the Linux kernel.**

More precisely:

> **Linux itself is technically the kernel. A complete Linux operating system is usually a Linux distribution (distro) containing the kernel, system libraries, utilities, package manager, and applications.**

Linux is known for being:

- Open source
- Secure
- Stable
- Fast
- Customizable
- Resource efficient
- Developer friendly
- Widely used on servers and cloud platforms

Linux is especially important for:

- Developers
- System Administrators
- DevOps Engineers
- Cloud Engineers
- SREs
- Cybersecurity Professionals
- Network Engineers

---

# 3. Linux vs UNIX — Important Background

Before understanding Linux history, we should understand **UNIX**.

UNIX is a powerful multi-user and multitasking operating system family that originated at **AT&T Bell Labs** around the end of the 1960s.

UNIX introduced many ideas that strongly influenced modern operating systems.

Linux was created later and was heavily influenced by UNIX concepts.

### Simple timeline

```text
1969
 ↓
UNIX begins at Bell Labs
 ↓
1970s–1980s
UNIX grows and spreads
 ↓
1991
 ↓
Linux kernel created by Linus Torvalds
 ↓
Linux + GNU tools + other software
 ↓
Modern Linux distributions
```

---

# 4. History of Linux

## 4.1 UNIX — The Foundation

UNIX was developed at **Bell Labs** by a team including:

- Ken Thompson
- Dennis Ritchie
- Other Bell Labs researchers

Dennis Ritchie also played a major role in developing the **C programming language**.

UNIX became important because it provided:

- Multi-user support
- Multitasking
- Portability
- Powerful command-line tools
- Hierarchical filesystem concepts

---

# 5. Linux is Born

In **1991**, a Finnish computer science student named **Linus Torvalds** started developing the Linux kernel.

His goal was to create a UNIX-like operating system kernel for his personal computer.

The project was released as open source and attracted contributors from around the world.

### Important point

Linux was **not created by one company**.

It became a global open-source project where developers could:

- Read the source code
- Modify it
- Improve it
- Contribute changes
- Distribute versions according to open-source licenses

---

# 6. What is Open Source?

**Open source** means the source code is available under a license that permits people to inspect, modify, and redistribute it subject to that license.

### Traditional proprietary software

```text
Company
   ↓
Source Code
   ↓
Users
```

Users generally cannot freely modify the source code.

### Open-source software

```text
Developers
    ↓
Source Code
    ↓
Community
 ↙  ↓  ↘
Use Modify Improve
```

### Why is this important?

Open source encourages:

- Collaboration
- Transparency
- Innovation
- Customization
- Community contribution
- Faster problem solving

---

# 7. Linux and GNU

You may hear the term **GNU/Linux**.

The Linux kernel is only one major component of a complete Linux-based operating system.

The **GNU Project** provided many important tools, libraries, and utilities used with Linux.

A simplified view is:

```text
Applications
     ↓
Shell / Utilities / Libraries
     ↓
Linux Kernel
     ↓
Hardware
```

So, when people say "Linux OS", they often mean a complete Linux distribution built around the Linux kernel.

---

# 8. Why is Linux Important?

This is one of the most important questions for students.

## 8.1 Linux Powers Servers

Linux is widely used for servers and data-center workloads.

Examples:

- Web servers
- Application servers
- Database servers
- File servers
- DNS servers
- Proxy servers
- Monitoring systems

Example:

```text
User
 ↓
Internet
 ↓
Web Server
 ↓
Linux
 ↓
Application
```

---

# 9. Linux in Cloud Computing

Modern cloud platforms run huge amounts of Linux-based infrastructure.

For example, cloud workloads can use Linux virtual machines such as:

- AWS EC2
- Azure Virtual Machines
- Google Cloud Compute Engine

### DevOps example

```text
Developer
    ↓
Git
    ↓
CI/CD
    ↓
Docker
    ↓
Linux Server
    ↓
Cloud
```

This is why Linux knowledge is extremely valuable for DevOps and Cloud careers.

---

# 10. Linux in DevOps

If you want to work in **DevOps**, Linux is not optional knowledge.

Many DevOps tools run on or interact heavily with Linux environments.

Examples:

- Git
- Jenkins
- Docker
- Kubernetes
- Terraform
- Ansible
- Nginx
- Apache
- Prometheus
- Grafana

A DevOps engineer commonly needs to:

- Connect to Linux servers
- Install packages
- Manage services
- Read logs
- Configure networking
- Manage users
- Set permissions
- Monitor CPU and memory
- Troubleshoot applications
- Write shell scripts

---

# 11. Where is Linux Used?

Linux is not only for servers.

## Major Linux Use Cases

### 1. Servers and Hosting

Used for:

- Web servers
- Application servers
- Database infrastructure
- Cloud infrastructure

### 2. Cloud Computing

Used extensively in:

- Virtual machines
- Containers
- Kubernetes clusters
- Cloud infrastructure

### 3. DevOps

Linux is a foundation for many:

- CI/CD systems
- Containers
- Infrastructure automation
- Monitoring systems

### 4. Cybersecurity

Examples:

- Kali Linux
- Parrot OS

These distributions are commonly used for security testing and cybersecurity education.

### 5. Embedded Systems

Linux is used in:

- Routers
- Smart devices
- Industrial systems
- IoT devices
- Automotive systems

### 6. Supercomputers

Linux is widely used in high-performance computing and supercomputing environments.

### 7. Desktop / Personal Computing

Linux can also be used as a desktop operating system.

Examples:

- Ubuntu
- Linux Mint
- Fedora
- Debian

---

# 12. Why Do Companies Use Linux?

Let's look at the practical reasons.

## 12.1 Cost

Many Linux distributions can be used without proprietary OS licensing fees.

This can be attractive for organizations operating large numbers of servers.

---

## 12.2 Security

Linux has a strong permissions model and mature security tooling.

Important concepts include:

- Users
- Groups
- File permissions
- Ownership
- sudo
- SELinux / AppArmor on applicable distributions

> Important: Linux is not "immune to viruses". Security still depends on correct configuration, updates, permissions, monitoring, and administration.

---

## 12.3 Stability

Linux is widely used for systems that need to run continuously.

For example:

```text
Web Server
   ↓
24 × 7
   ↓
Linux
```

Servers can run for long periods when properly maintained.

---

## 12.4 Performance

Linux can be very efficient with system resources.

This makes it suitable for:

- Servers
- Containers
- Cloud VMs
- Embedded devices
- Older hardware

---

## 12.5 Customization

Linux gives administrators significant control over:

- System configuration
- Services
- Networking
- Security
- Desktop environment
- Packages
- Automation

---

## 12.6 Automation

Linux provides a powerful command-line environment.

For example:

```bash
ls
cd
pwd
mkdir
cp
mv
rm
grep
find
```

Commands can also be combined into scripts.

Example:

```bash
#!/bin/bash

echo "Starting deployment..."
```

This becomes very useful in DevOps.

---

# 13. Why Learn Linux as a Student?

If you are planning a career in:

```text
Cloud
DevOps
Cybersecurity
System Administration
SRE
Networking
Backend Development
Data Engineering
```

Linux knowledge will help you understand how production systems actually work.

### Career connection

```text
Linux Basics
     ↓
Linux Commands
     ↓
File System
     ↓
Users & Permissions
     ↓
Processes & Services
     ↓
Networking
     ↓
Shell Scripting
     ↓
Docker
     ↓
Kubernetes
     ↓
Cloud & DevOps
```

---

# 14. Linux Distributions

A **Linux distribution (distro)** is a complete operating system built around the Linux kernel.

A distribution generally includes:

- Linux kernel
- System libraries
- System utilities
- Shell
- Package manager
- Applications
- Configuration tools

### Common distributions

| Distribution | Common Use |
|---|---|
| Ubuntu | Beginners, development, servers |
| Debian | Stability and servers |
| Fedora | Modern development technologies |
| RHEL | Enterprise environments |
| Rocky Linux | Enterprise/server environments |
| Kali Linux | Cybersecurity and penetration testing |
| Arch Linux | Advanced users and customization |
| Linux Mint | Beginner-friendly desktop |
| Lubuntu | Lightweight systems |

---

# 15. Why Are There So Many Linux Distros?

Students often ask:

> "Sir, Linux is one OS, then why are there Ubuntu, Kali, Fedora, Debian, etc.?"

Answer:

Linux is a kernel, and different communities/organizations build complete operating systems around it.

They can choose different:

- Packages
- Desktop environments
- Security defaults
- Release models
- Package managers
- Configuration tools
- Target users

### Example

```text
                Linux Kernel
                     |
       ┌─────────────┼─────────────┐
       ↓             ↓             ↓
    Ubuntu         Fedora        Arch
       |             |             |
   Beginners      Developers    Advanced
```

---

# 16. Ubuntu vs Kali Linux

### Ubuntu

Best suited for:

- Beginners
- Development
- General desktop use
- Servers
- Cloud learning

### Kali Linux

Designed mainly for:

- Cybersecurity
- Penetration testing
- Security research
- Digital forensics

> Kali Linux is not simply "a better Ubuntu". It is designed for a different purpose.

---

# 17. Linux Architecture — Basic Introduction

Linux can be understood as a layered system.

```text
+-----------------------------+
|        Applications         |
+-----------------------------+
|       Shell / Utilities     |
+-----------------------------+
|       System Libraries      |
+-----------------------------+
|          Kernel             |
+-----------------------------+
|          Hardware           |
+-----------------------------+
```

---

# 18. Kernel

The **kernel is the core of the Linux operating system**.

It manages resources and provides the interface between software and hardware.

### Kernel responsibilities

- Process management
- Memory management
- Device management
- File system support
- Networking
- Security mechanisms

Simple idea:

```text
Application
     ↓
Kernel
     ↓
Hardware
```

---

# 19. Shell

A **shell** is a command interpreter that allows users to interact with the operating system.

Example:

```bash
ls
```

You type a command in the shell.

The shell interprets the command and requests the required operation from the operating system.

### Common shells

- sh
- Bash
- Zsh
- Ksh
- Fish
- csh/tcsh

### Bash

**Bash = Bourne Again Shell**

It is one of the most widely used shells in Linux environments.

---

# 20. GUI vs CLI

Linux can be used through both GUI and CLI.

## GUI — Graphical User Interface

You interact using:

- Windows
- Icons
- Menus
- Mouse
- Applications

Examples:

- GNOME
- KDE Plasma
- XFCE

## CLI — Command Line Interface

You interact by entering commands.

Example:

```bash
pwd
ls
cd /var/log
```

### Why is CLI important?

In real server environments, you may have:

```text
Laptop
   ↓
SSH
   ↓
Remote Linux Server
```

There may be no graphical desktop.

Therefore:

> **For DevOps and server administration, CLI skills are essential.**

---

# 21. Linux File System — First Look

Linux uses a hierarchical filesystem starting from:

```text
/
```

This is called the **root directory**.

Example:

```text
/
├── home
├── etc
├── var
├── usr
├── tmp
├── dev
├── boot
└── opt
```

Important directories:

| Directory | Purpose |
|---|---|
| `/` | Root of the filesystem |
| `/home` | User home directories |
| `/etc` | Configuration files |
| `/var` | Variable data, logs, cache |
| `/tmp` | Temporary files |
| `/usr` | Programs, libraries, shared data |
| `/dev` | Device files |
| `/boot` | Boot-related files |
| `/root` | Root user's home directory |
| `/proc` | Virtual process/system information |
| `/sys` | Kernel/device information |

---

# 22. "Everything is a File" Concept

One famous Linux/Unix idea is:

> **"Everything is treated like a file"**

This is a simplified way of explaining how Linux exposes many resources through file-like interfaces.

Examples include:

- Regular files
- Directories
- Device files
- Virtual filesystems

For example:

```text
/dev
/proc
/sys
```

These concepts become very useful later when learning Linux administration.

---

# 23. Linux Package Management

Linux distributions use package managers to install, update, and remove software.

### Ubuntu / Debian

```bash
apt
```

Examples:

```bash
sudo apt update
sudo apt install nginx
```

### Fedora / RHEL family

Common package management tools include:

```bash
dnf
```

### Arch Linux

```bash
pacman
```

The exact tools and commands depend on the distribution.

---

# 24. Linux Permissions — First Introduction

Linux has a strong file permission system.

Basic permission types:

```text
r = read
w = write
x = execute
```

Permissions are associated with:

```text
User / Owner
Group
Others
```

Example:

```text
-rwxr-xr--
```

We will study this properly in the **Users, Groups & Permissions** lecture.

---

# 25. Linux in a Real DevOps Environment

Imagine a real production application:

```text
                 Internet
                    |
                    ↓
                  Load
                 Balancer
                    |
                    ↓
              Linux Servers
                    |
          ┌─────────┴─────────┐
          ↓                   ↓
       Docker              Nginx
          |
          ↓
      Application
          |
          ↓
       Database
```

A DevOps engineer may use Linux to:

- Connect to servers
- Deploy applications
- Check processes
- Read logs
- Configure Nginx
- Manage Docker
- Configure networking
- Create users
- Change permissions
- Automate deployments
- Troubleshoot failures

---

# 26. Linux Command-Line Examples

For the first practical introduction, demonstrate:

### Check current location

```bash
pwd
```

### List files

```bash
ls
```

### Detailed listing

```bash
ls -l
```

### Change directory

```bash
cd /tmp
```

### Go home

```bash
cd ~
```

### Create a directory

```bash
mkdir linux-demo
```

### Enter directory

```bash
cd linux-demo
```

### Create a file

```bash
touch hello.txt
```

### Display file

```bash
cat hello.txt
```

These commands will be covered in much more detail in the File & Directory Management lecture.

---

# 27. Linux Advantages — Quick Revision

### 1. Open Source

Source code is available under open-source licenses.

### 2. Cost Effective

Many distributions can be used without proprietary OS licensing fees.

### 3. Secure

Strong permissions and security tooling.

### 4. Stable

Widely used for long-running server workloads.

### 5. Fast and Efficient

Can run efficiently across many types of hardware.

### 6. Customizable

Administrators can customize the system extensively.

### 7. Developer Friendly

Excellent support for programming and automation tools.

### 8. Automation Friendly

Powerful CLI and shell scripting.

### 9. Huge Community

Large ecosystem of documentation, tools, and contributors.

---

# 28. Limitations / Things to Understand

Linux is powerful, but don't present it as magic.

Some challenges include:

- Learning the command line takes practice
- Different distributions have different tools and defaults
- Troubleshooting can require technical knowledge
- Some proprietary desktop software may not have native Linux versions
- Beginners may initially find permissions and configuration confusing

The goal is not to memorize 500 commands.

> **The goal is to understand what the system is doing.**

---

# 29. Important Terms to Remember

| Term | Meaning |
|---|---|
| OS | Operating System |
| UNIX | Influential operating system family |
| Linux Kernel | Core of Linux |
| Distribution | Complete OS built around the Linux kernel |
| Open Source | Source available under an open-source license |
| Shell | Command interpreter |
| CLI | Command Line Interface |
| GUI | Graphical User Interface |
| Root `/` | Top of Linux filesystem |
| Root user | Superuser/administrator account |
| Package Manager | Tool for managing software packages |
| Kernel | Manages system resources and hardware interaction |

---

# 30. Interview Questions

## Q1. What is Linux?

**Answer:**

Linux is an open-source operating system family built around the Linux kernel. Complete Linux operating systems are distributed as Linux distributions.

---

## Q2. Who created Linux?

**Answer:**

Linus Torvalds started developing the Linux kernel in 1991.

---

## Q3. What is UNIX?

**Answer:**

UNIX is a multi-user and multitasking operating system family that originated at AT&T Bell Labs and strongly influenced modern operating systems, including Linux.

---

## Q4. Is Linux an operating system or a kernel?

**Answer:**

Technically, Linux refers to the kernel. In everyday usage, "Linux" often refers to a complete Linux distribution containing the kernel and many other components.

---

## Q5. What is a Linux distribution?

**Answer:**

A Linux distribution is a complete operating system built around the Linux kernel and bundled with system libraries, utilities, package management tools, and applications.

---

## Q6. Why is Linux popular in DevOps?

**Answer:**

Linux is widely used on servers and cloud infrastructure and provides powerful command-line tools, automation capabilities, networking tools, permissions, scripting, and strong support for technologies such as Docker and Kubernetes.

---

## Q7. What is a shell?

**Answer:**

A shell is a command interpreter that allows users to communicate with the operating system by entering commands.

---

## Q8. What is Bash?

**Answer:**

Bash stands for Bourne Again Shell and is one of the most commonly used shells in Linux environments.

---

## Q9. What is the root directory?

**Answer:**

`/` is the top-level directory of the Linux filesystem.

---

# 31. Classroom Discussion Questions

Ask students:

### Question 1

> "If Windows exists, why do companies use Linux servers?"

Expected points:

- Cost
- Stability
- Security
- Performance
- Automation
- Server ecosystem
- Cloud/DevOps support

### Question 2

> "Is Kali Linux the only Linux distribution?"

Answer:

No.

There are many distributions such as:

- Ubuntu
- Debian
- Fedora
- RHEL
- Rocky Linux
- Arch
- Kali
- Linux Mint

### Question 3

> "Can Linux run without a GUI?"

Yes.

Linux servers commonly run without a desktop GUI and are managed through CLI/remote administration.

---

# 32. First Practical Task

## Task: Explore Linux

Open a Linux terminal and execute:

```bash
pwd
ls
ls -l
whoami
uname -a
```

Then:

```bash
mkdir linux-class
cd linux-class
touch first.txt
ls -l
```

### Students should understand:

```text
pwd     → Where am I?
ls      → What is here?
whoami  → Which user am I?
uname   → What system/kernel information do I have?
mkdir   → Create directory
cd      → Move between directories
touch   → Create an empty file
```

---

# 33. Mini Assignment

### Part A — Theory

Answer:

1. What is an operating system?
2. What is Linux?
3. What is the Linux kernel?
4. Who started Linux?
5. What is UNIX?
6. What does open source mean?
7. What is a Linux distribution?
8. Give five examples of Linux distributions.
9. Why is Linux important in DevOps?
10. What is the difference between CLI and GUI?

### Part B — Practical

Run:

```bash
pwd
whoami
uname -a
ls
mkdir practice
cd practice
touch file1.txt
touch file2.txt
ls -l
```

---

# 34. Final Revision

Remember this flow:

```text
                OPERATING SYSTEM
                       |
        ┌──────────────┼──────────────┐
        ↓              ↓              ↓
      Windows         macOS          Linux
                                      |
                                  Linux Kernel
                                      |
                        ┌─────────────┴─────────────┐
                        ↓                           ↓
                      Distros                    Tools
                        ↓                           ↓
              Ubuntu / Debian /             Bash / CLI /
              Fedora / Kali etc.            Utilities
```

### Most important points from Lecture 1

- Linux is built around the Linux kernel.
- Linux is open source.
- UNIX strongly influenced Linux.
- Linus Torvalds started Linux kernel development in 1991.
- A Linux distribution is a complete OS built around the Linux kernel.
- Linux is widely used in servers, cloud, DevOps, cybersecurity, embedded systems, and supercomputing.
- Linux provides both GUI and CLI environments.
- The command line is especially important for server administration and DevOps.
- `/` is the root of the Linux filesystem.
- Bash is one of the most commonly used Linux shells.
- Linux is important because it gives administrators powerful control, automation, security, and flexibility.

---

# Next Lecture Preview

## Linux File System & Basic Commands

Next, we will learn:

```text
/
├── home
├── etc
├── var
├── usr
├── tmp
├── dev
└── boot
```

And commands:

```bash
pwd
ls
cd
mkdir
rmdir
touch
cat
cp
mv
rm
find
grep
```

Then we will move toward:

```text
Users & Groups
      ↓
Permissions
      ↓
Processes
      ↓
Services
      ↓
Networking
      ↓
Shell Scripting
      ↓
Linux for DevOps
```

---

## Teacher Note

For beginners, don't start by making students memorize commands.

Build the mental model first:

> **Hardware → Kernel → Shell → Commands → Applications**

Once students understand **why Linux exists and how the pieces connect**, the commands become much easier to learn.
