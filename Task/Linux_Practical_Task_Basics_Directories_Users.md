# Linux Practical Session — Basics, Directories & User Management

## Industry-Style Practical Lab

**Level:** Beginner → Intermediate  
**Topics:** Linux Basics, Directories & Filesystem, Users & Groups  
**Environment:** Ubuntu/Debian Linux VM, WSL, or AWS EC2 Linux instance  
**Mode:** Hands-on practical  
**Rule:** Perform every task from the terminal.

---

## 1. Practical Objective

Work like a junior Linux/DevOps engineer managing a Linux server.

You will practice:

- Linux navigation and basic commands
- Linux filesystem and directory structure
- Creating/managing directories and files
- Copying, moving, renaming and finding files
- Checking disk/filesystem information
- Creating users and groups
- Managing UID/GID and group membership
- Configuring ownership and basic permissions
- Verifying and troubleshooting the final setup

---

## 2. Industry Scenario

You have joined a company as a **Junior DevOps Engineer**.

Your team has given you access to a Linux server that will host an application called:

```text
employee-portal
```

Your responsibilities:

1. Inspect the Linux server.
2. Create an application directory structure.
3. Create application, configuration, log and backup files.
4. Create development and testing users.
5. Create developer and tester groups.
6. Assign users to the correct groups.
7. Configure ownership and permissions.
8. Verify the final configuration.

---

# PART A — LINUX BASICS

## Task 1 — Identify the Current User

Find which Linux user you are currently using.

Use the appropriate command and record the output.

---

## Task 2 — Check User Information

Display your:

- UID
- GID
- Groups

---

## Task 3 — Check Current Location

Find the current working directory.

Answer:

- What directory are you currently inside?
- Is it an absolute or relative path?

---

## Task 4 — Inspect the Root Filesystem

List the contents of:

```text
/
```

Identify at least **10 important directories** and write the purpose of each.

---

## Task 5 — Explore Important Linux Directories

Explore:

```text
/etc
/home
/var
/tmp
/usr
/opt
```

For each:

1. List some contents.
2. Explain why it is important.

---

## Task 6 — Check System Information

Find:

- Linux distribution
- Kernel version
- Hostname

---

# PART B — DIRECTORY & FILE MANAGEMENT

## Task 7 — Create an Application Workspace

Inside your home directory, create:

```text
linux-practical/
├── application/
├── configuration/
├── logs/
├── backup/
└── documentation/
```

---

## Task 8 — Create Application Files

Inside `application/`, create:

```text
app.py
requirements.txt
README.md
```

---

## Task 9 — Create Configuration Files

Inside `configuration/`, create:

```text
app.conf
database.conf
```

---

## Task 10 — Create Log Files

Inside `logs/`, create:

```text
application.log
error.log
access.log
```

---

## Task 11 — Create Documentation

Inside `documentation/`, create:

```text
deployment.md
operations.md
```

---

## Task 12 — Verify the Directory Structure

Display the complete project structure.

Expected:

```text
linux-practical/
├── application/
│   ├── app.py
│   ├── requirements.txt
│   └── README.md
├── configuration/
│   ├── app.conf
│   └── database.conf
├── logs/
│   ├── access.log
│   ├── application.log
│   └── error.log
├── backup/
└── documentation/
    ├── deployment.md
    └── operations.md
```

---

# PART C — FILE OPERATIONS

## Task 13 — Add Sample Data

Put sample content into:

```text
application/app.py
```

Put this configuration into:

```text
configuration/app.conf
```

```text
APP_NAME=employee-portal
APP_ENV=development
APP_PORT=8080
```

---

## Task 14 — Read Files

Display:

```text
application/app.py
configuration/app.conf
```

---

## Task 15 — Copy a Configuration File

Copy:

```text
configuration/app.conf
```

to:

```text
backup/app.conf.backup
```

---

## Task 16 — Move a Documentation File

Rename/move:

```text
documentation/operations.md
```

to:

```text
documentation/runbook.md
```

---

## Task 17 — Rename a Log File

Rename:

```text
logs/error.log
```

to:

```text
logs/application-error.log
```

---

## Task 18 — Find Files

Using `find`, locate:

1. All `.log` files.
2. All `.conf` files.
3. `app.py`.
4. All files/directories inside `linux-practical`.

---

# PART D — LINUX FILESYSTEM PRACTICAL

## Task 19 — Check Disk Space

Display filesystem disk usage in human-readable format.

Record:

- Root filesystem
- Total space
- Used space
- Available space
- Usage percentage

---

## Task 20 — Check Filesystem Type

Display:

- Filesystem type
- Mount point
- Disk usage

Identify whether the root filesystem is `ext4`, `xfs`, `btrfs`, or another type.

---

## Task 21 — Check Block Devices

Display available disks and partitions.

Identify:

- Main disk
- Partitions
- Root mount point

---

## Task 22 — Check Directory Size

Find the size of:

```text
linux-practical/
logs/
application/
```

---

# PART E — LINUX USERS

## Task 23 — Create Groups

Create:

```text
developers
testers
```

Verify both groups exist.

---

## Task 24 — Create Development Users

Create:

```text
developer1
developer2
```

Each must have:

- A home directory
- Bash as login shell

---

## Task 25 — Create Testing User

Create:

```text
tester1
```

with a home directory and Bash shell.

---

## Task 26 — Set Passwords

Set passwords for:

```text
developer1
developer2
tester1
```

Use strong passwords in your lab.

---

# PART F — GROUP MANAGEMENT

## Task 27 — Add Developers to Developers Group

Add:

```text
developer1
developer2
```

to:

```text
developers
```

---

## Task 28 — Add Tester to Testers Group

Add:

```text
tester1
```

to:

```text
testers
```

---

## Task 29 — Verify Group Membership

Check groups for:

```text
developer1
developer2
tester1
```

---

## Task 30 — Check UID and GID

For each user find:

- Username
- UID
- Primary GID
- Supplementary groups
- Home directory
- Login shell

---

# PART G — USERS + DIRECTORIES + OWNERSHIP

## Task 31 — Create an Industry Application Directory

Create:

```text
/opt/employee-portal
```

with:

```text
/opt/employee-portal/
├── app
├── config
├── logs
└── backup
```

---

## Task 32 — Create Application Files

Create:

```text
/opt/employee-portal/app/app.py
/opt/employee-portal/config/app.conf
/opt/employee-portal/logs/app.log
```

---

## Task 33 — Configure Group Ownership

Make:

```text
developers
```

the group owner of:

```text
/opt/employee-portal
```

and its contents.

---

## Task 34 — Configure User Ownership

Make:

```text
developer1
```

the owner of the application directory and its contents.

---

## Task 35 — Configure Basic Permissions

Configure:

```text
Owner     → Full access
Developers → Full access
Others    → No access
```

Understand what each permission means before applying it.

---

# PART H — VERIFICATION

## Task 36 — Verify Directory Ownership

Check `/opt/employee-portal` and confirm:

- Owner
- Group
- Permissions

---

## Task 37 — Verify All Files

Check ownership and permissions recursively.

Confirm:

```text
developer1 → owner
developers → group
```

---

## Task 38 — Verify User Configuration

For:

```text
developer1
developer2
tester1
```

verify:

- UID
- GID
- Home directory
- Login shell
- Group membership

---

# PART I — TROUBLESHOOTING SCENARIOS

## Scenario 1 — Permission Denied

A developer runs:

```bash
cd /opt/employee-portal
```

and gets:

```text
Permission denied
```

What would you check first?

List the commands you would use.

---

## Scenario 2 — User Not in Group

`developer2` says:

> I was added to the developers group, but I still don't have access.

Answer:

1. How do you verify membership?
2. Why might the current terminal session not show the new group?
3. What should the user do?

---

## Scenario 3 — Disk Full

The application stops with:

```text
No space left on device
```

Which commands would you use to investigate?

Think about:

```text
df
du
/var/log
```

---

## Scenario 4 — Application Logs

The application is running but users report errors.

Find the application's log and monitor it in real time using:

```text
tail -f
```

---

# PART J — FINAL INDUSTRY CHALLENGE

Your DevOps lead says:

> "Set up a basic Linux environment for our Employee Portal. Developers need to work on the application, testers need access to testing resources, and users must not receive unnecessary privileges."

Complete the following without looking at a solution:

### Create groups

```text
developers
testers
```

### Create users

```text
developer1
developer2
tester1
```

### Assign

```text
developer1 → developers
developer2 → developers
tester1     → testers
```

### Create

```text
/opt/employee-portal
```

with:

```text
app/
config/
logs/
backup/
```

### Create appropriate files.

### Configure

```text
Owner → developer1
Group → developers
```

### Configure permissions

```text
Owner      → Full access
Developers → Full access
Others     → No access
```

Finally verify everything.

---

# Submission Checklist

- [ ] Current user identified
- [ ] UID/GID checked
- [ ] Root filesystem explored
- [ ] Important directories identified
- [ ] Project workspace created
- [ ] Files created
- [ ] Files copied
- [ ] Files moved/renamed
- [ ] `find` used
- [ ] Disk usage checked
- [ ] Filesystem type checked
- [ ] Block devices checked
- [ ] Groups created
- [ ] Users created
- [ ] Passwords configured
- [ ] Users added to groups
- [ ] UID/GID verified
- [ ] `/opt/employee-portal` created
- [ ] Ownership configured
- [ ] Permissions configured
- [ ] Final setup verified
- [ ] Troubleshooting attempted

---

## Expected Final Architecture

```text
Linux Server
│
├── Users
│   ├── developer1
│   ├── developer2
│   └── tester1
│
├── Groups
│   ├── developers
│   │   ├── developer1
│   │   └── developer2
│   │
│   └── testers
│       └── tester1
│
└── /opt/employee-portal
    ├── app
    │   └── app.py
    ├── config
    │   └── app.conf
    ├── logs
    │   └── app.log
    └── backup
```

## Learning Outcome

By completing this practical, you should be able to manage a basic Linux server as a junior DevOps engineer:

```text
Linux Server
     ↓
Filesystem
     ↓
Directories
     ↓
Files
     ↓
Users
     ↓
Groups
     ↓
Ownership
     ↓
Permissions
     ↓
Secure Access
```
