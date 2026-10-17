# Linux Practical Session — Solutions

> These are instructor solutions. Exact UID/GID numbers, disk sizes, device names, filesystem types, and directory contents vary by Linux distribution and machine.

---

# PART A — LINUX BASICS

## Task 1

```bash
whoami
```

---

## Task 2

```bash
id
```

Example:

```text
uid=1000(sahil) gid=1000(sahil) groups=1000(sahil),27(sudo)
```

---

## Task 3

```bash
pwd
```

Example:

```text
/home/sahil
```

This is an absolute path because it starts with `/`.

---

## Task 4

```bash
ls /
```

Common directories include:

```text
/bin
/boot
/dev
/etc
/home
/lib
/media
/mnt
/opt
/proc
/root
/run
/sbin
/srv
/sys
/tmp
/usr
/var
```

Important meanings:

```text
/      → Root of filesystem hierarchy
/etc   → System configuration
/home  → Normal users' home directories
/root  → Root user's home
/var   → Variable/changing data and logs
/tmp   → Temporary data
/usr   → User-space programs/resources
/opt   → Optional/add-on applications
/dev   → Device files
/proc  → Process/kernel information
/sys   → Kernel/device information
/boot  → Boot-related files
```

---

## Task 5

```bash
ls -la /etc
ls -la /home
ls -la /var
ls -la /tmp
ls -la /usr
ls -la /opt
```

Students should explain the purpose of each rather than memorize the contents.

---

## Task 6

Distribution:

```bash
cat /etc/os-release
```

Kernel:

```bash
uname -r
```

Hostname:

```bash
hostname
```

Useful combined command:

```bash
hostnamectl
```

---

# PART B — DIRECTORY & FILE MANAGEMENT

## Task 7

```bash
mkdir -p ~/linux-practical/{application,configuration,logs,backup,documentation}
```

---

## Task 8

```bash
touch ~/linux-practical/application/app.py
touch ~/linux-practical/application/requirements.txt
touch ~/linux-practical/application/README.md
```

---

## Task 9

```bash
touch ~/linux-practical/configuration/app.conf
touch ~/linux-practical/configuration/database.conf
```

---

## Task 10

```bash
touch ~/linux-practical/logs/application.log
touch ~/linux-practical/logs/error.log
touch ~/linux-practical/logs/access.log
```

---

## Task 11

```bash
touch ~/linux-practical/documentation/deployment.md
touch ~/linux-practical/documentation/operations.md
```

---

## Task 12

If `tree` is installed:

```bash
tree ~/linux-practical
```

Otherwise:

```bash
find ~/linux-practical -print
```

Expected structure:

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

## Task 13

Example `app.py`:

```bash
cat > ~/linux-practical/application/app.py <<'EOF'
print("Employee Portal application started")
EOF
```

Configuration:

```bash
cat > ~/linux-practical/configuration/app.conf <<'EOF'
APP_NAME=employee-portal
APP_ENV=development
APP_PORT=8080
EOF
```

---

## Task 14

```bash
cat ~/linux-practical/application/app.py
cat ~/linux-practical/configuration/app.conf
```

---

## Task 15

```bash
cp ~/linux-practical/configuration/app.conf    ~/linux-practical/backup/app.conf.backup
```

Verify:

```bash
ls -l ~/linux-practical/backup
```

---

## Task 16

```bash
mv ~/linux-practical/documentation/operations.md    ~/linux-practical/documentation/runbook.md
```

---

## Task 17

```bash
mv ~/linux-practical/logs/error.log    ~/linux-practical/logs/application-error.log
```

---

## Task 18

All logs:

```bash
find ~/linux-practical -type f -name "*.log"
```

All configuration files:

```bash
find ~/linux-practical -type f -name "*.conf"
```

Find `app.py`:

```bash
find ~/linux-practical -type f -name "app.py"
```

Everything:

```bash
find ~/linux-practical
```

---

# PART D — LINUX FILESYSTEM

## Task 19

```bash
df -h
```

Root filesystem:

```bash
df -h /
```

Record the values shown by your own machine.

---

## Task 20

```bash
df -T
```

Or:

```bash
df -T /
```

Example:

```text
Filesystem     Type  Size  Used Avail Use% Mounted on
/dev/sda1      ext4   50G   20G   28G  42% /
```

Your result may be `xfs`, `ext4`, `btrfs`, etc.

---

## Task 21

```bash
lsblk
```

More detail:

```bash
lsblk -f
```

Identify the disk, partitions, filesystem and root mount point.

---

## Task 22

```bash
du -sh ~/linux-practical
du -sh ~/linux-practical/logs
du -sh ~/linux-practical/application
```

---

# PART E — USERS

## Task 23

```bash
sudo groupadd developers
sudo groupadd testers
```

Verify:

```bash
getent group developers
getent group testers
```

---

## Task 24

```bash
sudo useradd -m -s /bin/bash developer1
sudo useradd -m -s /bin/bash developer2
```

Verify:

```bash
id developer1
id developer2
ls -ld /home/developer1 /home/developer2
```

---

## Task 25

```bash
sudo useradd -m -s /bin/bash tester1
```

Verify:

```bash
id tester1
ls -ld /home/tester1
```

---

## Task 26

```bash
sudo passwd developer1
sudo passwd developer2
sudo passwd tester1
```

---

# PART F — GROUP MANAGEMENT

## Task 27

```bash
sudo usermod -aG developers developer1
sudo usermod -aG developers developer2
```

Verify:

```bash
id developer1
id developer2
```

---

## Task 28

```bash
sudo usermod -aG testers tester1
```

Verify:

```bash
id tester1
```

---

## Task 29

```bash
groups developer1
groups developer2
groups tester1
```

Or:

```bash
id developer1
id developer2
id tester1
```

---

## Task 30

```bash
id developer1
id developer2
id tester1
```

For account information:

```bash
getent passwd developer1
getent passwd developer2
getent passwd tester1
```

Example:

```text
developer1:x:1001:1001::/home/developer1:/bin/bash
```

---

# PART G — USERS + DIRECTORIES + OWNERSHIP

## Task 31

```bash
sudo mkdir -p /opt/employee-portal/{app,config,logs,backup}
```

Verify:

```bash
sudo find /opt/employee-portal -maxdepth 2 -print
```

---

## Task 32

```bash
sudo touch /opt/employee-portal/app/app.py
sudo touch /opt/employee-portal/config/app.conf
sudo touch /opt/employee-portal/logs/app.log
```

---

## Task 33

Set the group ownership:

```bash
sudo chown -R :developers /opt/employee-portal
```

---

## Task 34

Set owner and group together:

```bash
sudo chown -R developer1:developers /opt/employee-portal
```

---

## Task 35

Required model:

```text
Owner      → rwx
Group      → rwx
Others     → ---
```

Command:

```bash
sudo chmod -R 770 /opt/employee-portal
```

Verify:

```bash
ls -ld /opt/employee-portal
```

Expected pattern:

```text
drwxrwx---
```

---

# PART H — VERIFICATION

## Task 36

```bash
ls -ld /opt/employee-portal
```

Expected concept:

```text
Owner → developer1
Group → developers
Permissions → rwxrwx---
```

---

## Task 37

```bash
sudo ls -lR /opt/employee-portal
```

Or:

```bash
sudo find /opt/employee-portal -ls
```

---

## Task 38

```bash
id developer1
getent passwd developer1

id developer2
getent passwd developer2

id tester1
getent passwd tester1
```

Check home directories:

```bash
ls -ld /home/developer1
ls -ld /home/developer2
ls -ld /home/tester1
```

---

# PART I — TROUBLESHOOTING

## Scenario 1 — Permission Denied

Start with:

```bash
ls -ld /opt/employee-portal
```

Check the user:

```bash
id developer1
```

Check the complete path:

```bash
namei -l /opt/employee-portal
```

Check group membership:

```bash
groups developer1
```

Potential causes:

```text
Wrong owner
Wrong group
Missing group membership
Missing execute permission on a directory
Insufficient permissions on a parent directory
```

---

## Scenario 2 — Group Membership Not Visible

Check:

```bash
id developer2
```

If membership was just changed, the current login session may not have the updated supplementary groups.

Recommended:

```text
Log out → Log in again
```

For a controlled test session:

```bash
newgrp developers
```

Then:

```bash
id
```

---

## Scenario 3 — Disk Full

Start:

```bash
df -h
```

Then investigate:

```bash
sudo du -xhd1 / 2>/dev/null
```

Check `/var`:

```bash
sudo du -sh /var/* 2>/dev/null
```

Check logs:

```bash
sudo du -sh /var/log/*
```

Do not blindly delete files. Identify the cause first.

---

## Scenario 4 — Application Logs

```bash
ls -lh /opt/employee-portal/logs
```

Monitor:

```bash
tail -f /opt/employee-portal/logs/app.log
```

Stop with:

```text
Ctrl + C
```

---

# PART J — FINAL INDUSTRY CHALLENGE

One valid implementation:

## 1. Create groups

```bash
sudo groupadd developers
sudo groupadd testers
```

## 2. Create users

```bash
sudo useradd -m -s /bin/bash developer1
sudo useradd -m -s /bin/bash developer2
sudo useradd -m -s /bin/bash tester1
```

## 3. Set passwords

```bash
sudo passwd developer1
sudo passwd developer2
sudo passwd tester1
```

## 4. Add users to groups

```bash
sudo usermod -aG developers developer1
sudo usermod -aG developers developer2
sudo usermod -aG testers tester1
```

## 5. Create application structure

```bash
sudo mkdir -p /opt/employee-portal/{app,config,logs,backup}
```

## 6. Create files

```bash
sudo touch /opt/employee-portal/app/app.py
sudo touch /opt/employee-portal/config/app.conf
sudo touch /opt/employee-portal/logs/app.log
```

## 7. Set ownership

```bash
sudo chown -R developer1:developers /opt/employee-portal
```

## 8. Set permissions

```bash
sudo chmod -R 770 /opt/employee-portal
```

## 9. Verify

```bash
ls -ld /opt/employee-portal
sudo ls -lR /opt/employee-portal

id developer1
id developer2
id tester1
```

---

# Instructor Verification Checklist

| Area | Command | Expected |
|---|---|---|
| Current user | `whoami` | Correct user |
| User info | `id` | UID/GID/groups |
| Current directory | `pwd` | Valid path |
| Root filesystem | `ls /` | Standard directories |
| Project | `find` | Required structure |
| Disk | `df -h` | Usage visible |
| Filesystem | `df -T` | Type visible |
| Devices | `lsblk` | Disk/partition structure |
| Groups | `getent group developers` | Group exists |
| Users | `id developer1` | User exists |
| Membership | `id developer1` | developers visible |
| App directory | `ls -ld /opt/employee-portal` | Correct owner/group |
| Permissions | `ls -ld /opt/employee-portal` | Intended permissions |

---

# Final Concept

```text
LINUX BASICS
      ↓
FILESYSTEM
      ↓
DIRECTORIES
      ↓
FILES
      ↓
USERS
      ↓
GROUPS
      ↓
OWNERSHIP
      ↓
PERMISSIONS
      ↓
SECURE ACCESS
```

The goal is not command memorization. The goal is understanding how a Linux/DevOps engineer prepares a server, organizes application files, creates users/groups, and controls access.
