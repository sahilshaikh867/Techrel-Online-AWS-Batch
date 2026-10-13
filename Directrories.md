# Linux Directories & File System — Detailed Lecture

## 1. First Question: What is a File?

A **file** is a container used to store information/data.

Examples:

```text
notes.txt
student.csv
app.py
index.html
config.json
photo.jpg
database.sql
```

A file can contain:

- Text
- Code
- Configuration
- Images
- Logs
- Data
- Executable programs

### Real-world example

Think of a notebook.

```text
Notebook → File
Pages    → File contents
```

Linux stores information using files.

---

# 2. What is a Directory?

A **directory** is a container used to organize files and other directories.

In Windows, we usually call it a **folder**.

In Linux, the proper term is:

> **Directory**

Example:

```text
project/
│
├── index.html
├── style.css
├── app.js
│
└── images/
    ├── logo.png
    └── banner.jpg
```

Here:

```text
project  → Directory
images   → Directory

index.html → File
style.css  → File
app.js     → File
logo.png   → File
```

---

# 3. Why Do We Need Directories?

Imagine putting 10,000 files into one location:

```text
server/
├── file1
├── file2
├── file3
├── ...
└── file10000
```

Finding anything becomes difficult.

Instead:

```text
server/
│
├── applications/
├── logs/
├── backups/
├── configuration/
├── users/
└── projects/
```

Directories provide **organization and structure**.

---

# 4. What is a File System?

This is the most important question.

A **file system** is the method used by an operating system to organize, store, locate, and manage data on storage devices.

In simple words:

> File system tells Linux how files and directories are stored and managed on disk.

Think of a library.

```text
Library
│
├── Section
├── Shelf
├── Book
└── Page
```

Linux:

```text
File System
│
├── Directory
├── Subdirectory
├── File
└── Data
```

---

# 5. File System vs Directory

Students often confuse these.

### Directory

A directory is one part of the file system.

Example:

```text
/home/sahil
```

### File System

The complete structure used to organize data on storage.

Example:

```text
Disk
 ↓
File System
 ↓
Directories
 ↓
Files
```

---

# 6. Windows vs Linux File Structure

This is a good point to explain.

Windows commonly uses:

```text
C:\
D:\
E:\
```

Linux uses:

```text
/
```

The `/` is called:

> **Root directory**

Important:

**Root directory `/` is NOT the same as root user.**

---

# 7. Root Directory `/`

Linux starts its directory hierarchy from:

```text
/
```

Think of `/` as the top of the entire Linux directory tree.

Example:

```text
/
├── bin
├── boot
├── dev
├── etc
├── home
├── lib
├── opt
├── proc
├── root
├── run
├── sbin
├── tmp
├── usr
└── var
```

Everything starts from `/`.

---

# 8. Very Important: `/` vs `/root`

Students must understand this.

### `/`

Root directory.

```text
/
```

### `/root`

Home directory of the root user.

```text
/root
```

So:

```text
/       → Entire Linux filesystem
/root   → Root user's home directory
```

And:

```text
/home/sahil
```

would normally be Sahil's home directory.

---

# 9. Linux Directory Structure

The main directories students should know are:

```text
/
├── /bin
├── /boot
├── /dev
├── /etc
├── /home
├── /lib
├── /media
├── /mnt
├── /opt
├── /proc
├── /root
├── /run
├── /sbin
├── /srv
├── /tmp
├── /usr
└── /var
```

Now explain **each one individually**.

---

# 10. `/bin`

`/bin` contains essential executable commands/programs.

Examples traditionally include:

```text
ls
cp
mv
cat
mkdir
rm
```

Students can check:

```bash
ls /bin
```

Modern Linux distributions may merge `/bin` into `/usr/bin` through a `/usr`-merged layout, so explain that `/bin` remains an important path conceptually, even though its implementation may differ.

---

# 11. `/sbin`

`/sbin` traditionally contains important system-administration commands.

Examples include tools related to:

- System management
- Networking
- Filesystems
- Boot/recovery

Check:

```bash
ls /sbin
```

Again, on modern distributions, `/sbin` may be linked/merged with `/usr/sbin`.

---

# 12. `/boot`

Contains files required for the system boot process.

Example:

```text
/boot
├── kernel
├── initramfs
└── bootloader files
```

You can show:

```bash
ls -lh /boot
```

### Real-world meaning

When the computer starts:

```text
Power ON
   ↓
Firmware
   ↓
Bootloader
   ↓
Kernel
   ↓
Linux
```

`/boot` contains important boot-related files.

---

# 13. `/etc`

This is one of the **most important directories for DevOps**.

`/etc` contains system-wide configuration files.

Examples:

```text
/etc/passwd
/etc/shadow
/etc/group
/etc/hosts
/etc/hostname
/etc/fstab
/etc/ssh/
```

Remember:

> `/etc` = system configuration

---

## Real DevOps Example

Suppose you configure SSH.

Important configuration:

```text
/etc/ssh/sshd_config
```

Nginx configuration might be under:

```text
/etc/nginx/
```

So:

```text
/etc
 ↓
Configuration
 ↓
Services
```

---

# 14. `/home`

Contains home directories of normal users.

Example:

```text
/home
├── sahil
├── developer1
└── tester1
```

For Sahil:

```text
/home/sahil
```

This is normally where the user's personal files are stored.

Example:

```text
/home/sahil/
├── Documents
├── Downloads
├── projects
└── notes
```

---

# 15. `/root`

This is the home directory of the root user.

```text
/root
```

Don't confuse:

```text
/       → Root directory

/root   → Root user's home
```

---

# 16. `/var`

Very important for DevOps.

`/var` contains **variable/changing data**.

Examples:

```text
/var/log
/var/cache
/var/lib
/var/tmp
```

---

# 17. `/var/log`

Contains system and application logs.

Example:

```bash
ls /var/log
```

You may find logs such as:

```text
syslog
auth.log
kern.log
```

Depending on the Linux distribution.

### Why important?

When a service fails:

```text
Application
   ↓
Error
   ↓
Check logs
   ↓
/var/log
```

DevOps engineers frequently investigate logs.

---

# 18. `/var/lib`

Contains persistent application/system state.

Examples can include data used by:

- Package managers
- Databases
- Docker
- Other services

For example, depending on configuration, Docker data may be under:

```text
/var/lib/docker
```

This makes `/var/lib` particularly relevant in DevOps.

---

# 19. `/tmp`

Used for temporary files.

```text
/tmp
```

Applications can create temporary files here.

Important concept:

> Do not treat `/tmp` as permanent storage.

Temporary files may be removed during cleanup or reboot depending on the system configuration.

---

# 20. `/usr`

This is a very important directory.

`/usr` contains a large portion of user-space programs, libraries, documentation, and other resources.

Common directories:

```text
/usr/bin
/usr/sbin
/usr/lib
/usr/share
```

For example:

```bash
ls /usr/bin
```

You'll find many installed commands/programs.

---

# 21. `/opt`

`/opt` is commonly used for optional/add-on application software.

Example:

```text
/opt
└── myapplication
```

You may see application deployments organized here.

For example:

```text
/opt/myapp
```

---

# 22. `/dev`

This is a special directory containing device files.

Examples can include:

```text
/dev/sda
/dev/nvme0n1
/dev/null
/dev/zero
```

Linux follows the concept:

> **Everything is represented as a file or file-like object where practical.**

For example:

```bash
echo "hello" > /dev/null
```

`/dev/null` discards data.

---

# 23. `/proc`

`/proc` is a virtual filesystem that exposes information about:

- Running processes
- Kernel
- Memory
- CPU
- System state

Example:

```bash
ls /proc
```

Check CPU information:

```bash
cat /proc/cpuinfo
```

Memory:

```bash
cat /proc/meminfo
```

Running process:

```bash
ls /proc
```

You'll see numeric directories representing process IDs.

---

# 24. `/sys`

`/sys` is another virtual filesystem that exposes information about devices, kernel subsystems, and hardware-related state.

Example:

```bash
ls /sys
```

For beginner students, remember:

```text
/proc → process/kernel information

/sys  → devices/kernel subsystem information
```

---

# 25. `/run`

Contains runtime data created since the system booted.

Examples include:

- Process IDs
- Sockets
- Runtime state

You can check:

```bash
ls /run
```

Important:

> `/run` is generally temporary runtime data, not permanent application storage.

---

# 26. `/mnt`

Traditionally used as a temporary mount point.

Example:

```bash
sudo mount /dev/sdb1 /mnt
```

Now the filesystem/device becomes accessible through:

```text
/mnt
```

---

# 27. `/media`

Commonly used for automatically mounted removable media.

For example:

```text
USB drive
   ↓
/media/username/USB
```

Exact behavior depends on the Linux distribution and desktop environment.

---

# 28. `/srv`

Contains data intended to be served by services.

Example:

```text
/srv/www
/srv/ftp
```

It's less commonly used in beginner DevOps setups than `/var/www` or application-specific directories, but students should know its purpose.

---

# 29. Complete Directory Structure

Give students this diagram:

```text
/
│
├── bin       → Essential commands
├── boot      → Boot files
├── dev       → Device files
├── etc       → Configuration
├── home      → Normal users' home directories
├── lib       → Essential libraries
├── media     → Removable media
├── mnt       → Mount point
├── opt       → Optional/add-on applications
├── proc      → Process/kernel information
├── root      → Root user's home
├── run       → Runtime data
├── sbin      → System administration commands
├── srv       → Service data
├── sys       → Kernel/device information
├── tmp       → Temporary files
├── usr       → User-space programs/resources
└── var       → Variable/changing data
```

---

# 30. Absolute Path

An **absolute path** starts from `/`.

Example:

```text
/home/sahil/project/app.py
```

It tells Linux the complete location.

```text
/
└── home
    └── sahil
        └── project
            └── app.py
```

---

# 31. Relative Path

A relative path starts from the current directory.

Suppose:

```bash
pwd
```

returns:

```text
/home/sahil
```

And you have:

```text
/home/sahil/project/app.py
```

You can access it using:

```text
project/app.py
```

This is a relative path.

---

# 32. `pwd`

`pwd` means:

> Print Working Directory

Command:

```bash
pwd
```

Example:

```text
/home/sahil/projects
```

It tells you:

> Where am I currently?

---

# 33. `ls`

`ls` means:

> List directory contents.

```bash
ls
```

Show detailed information:

```bash
ls -l
```

Show hidden files:

```bash
ls -a
```

Both:

```bash
ls -la
```

Human-readable sizes:

```bash
ls -lh
```

Very useful:

```bash
ls -lah
```

---

# 34. Hidden Files

Linux files beginning with `.` are hidden from normal `ls`.

Example:

```text
.bashrc
.profile
.gitconfig
```

Show them:

```bash
ls -la
```

Important:

> Hidden does not mean secure.

It's simply a naming convention for normal directory listings.

---

# 35. `cd`

`cd` means:

> Change Directory

Example:

```bash
cd /etc
```

Go to home:

```bash
cd ~
```

Go back to previous directory:

```bash
cd -
```

Go to parent:

```bash
cd ..
```

---

# 36. `.` and `..`

These are very important.

`.` means:

> Current directory

`..` means:

> Parent directory

Example:

```bash
cd ..
```

means:

```text
Current directory
      ↓
Parent directory
```

---

# 37. `~`

`~` represents the current user's home directory.

If user is:

```text
sahil
```

then:

```text
~
```

usually represents:

```text
/home/sahil
```

So:

```bash
cd ~
```

takes you to your home directory.

---

# 38. `mkdir`

`mkdir` means:

> Make Directory

Example:

```bash
mkdir project
```

Create multiple directories:

```bash
mkdir frontend backend database
```

Nested directories:

```bash
mkdir -p project/src/components
```

`-p` creates missing parent directories as needed.

---

# 39. `touch`

`touch` can create an empty file if it doesn't exist.

```bash
touch app.py
```

Multiple files:

```bash
touch index.html style.css app.js
```

It can also update timestamps of an existing file.

---

# 40. `cat`

Used to display file contents.

```bash
cat app.py
```

Example:

```bash
cat /etc/hostname
```

---

# 41. `less`

For reading large files:

```bash
less /var/log/syslog
```

Useful because you don't need to load/display the entire file at once in the terminal.

Navigation includes:

```text
Space → next page
b     → previous page
q     → quit
```

---

# 42. `head`

Shows the beginning of a file.

```bash
head file.txt
```

First 20 lines:

```bash
head -n 20 file.txt
```

---

# 43. `tail`

Shows the end of a file.

```bash
tail file.txt
```

Very important in DevOps:

```bash
tail -f application.log
```

`-f` follows the file as new log entries are written.

This is commonly used for live log monitoring.

---

# 44. `cp`

`cp` means:

> Copy

Example:

```bash
cp file.txt backup.txt
```

Copy directory:

```bash
cp -r project project_backup
```

`-r` means recursive.

---

# 45. `mv`

`mv` is used to:

- Move files/directories
- Rename files/directories

Rename:

```bash
mv old.txt new.txt
```

Move:

```bash
mv app.py /tmp/
```

---

# 46. `rm`

Remove a file:

```bash
rm file.txt
```

Remove empty directory:

```bash
rmdir olddir
```

Remove directory and contents:

```bash
rm -r project
```

Be extremely careful with:

```bash
rm -rf
```

Especially on production servers.

---

# 47. `find`

`find` is extremely useful for DevOps.

Example:

```bash
find /home -name "app.py"
```

Find all `.log` files:

```bash
find /var/log -name "*.log"
```

Find files larger than 100 MB:

```bash
find /var -type f -size +100M
```

---

# 48. `file`

The `file` command tells you what kind of file something is.

```bash
file app.py
```

Example:

```text
Python script
```

Try:

```bash
file image.jpg
file archive.tar
file executable
```

---

# 49. File Types in Linux

Linux supports several file types.

You can identify them using:

```bash
ls -l
```

The first character is important.

Example:

```text
-rw-r--r--
```

First character:

```text
-
```

means regular file.

Directory:

```text
drwxr-xr-x
```

First character:

```text
d
```

means directory.

---

# 50. Important File Types

| Symbol | Meaning |
|---|---|
| `-` | Regular file |
| `d` | Directory |
| `l` | Symbolic link |
| `c` | Character device |
| `b` | Block device |
| `s` | Socket |
| `p` | Named pipe/FIFO |

For beginners, focus first on:

```text
- → File
d → Directory
l → Symbolic link
```

---

# 51. Inode — Important Concept

This is a slightly advanced but important topic.

Linux filesystems use **inodes** to store metadata about files.

An inode can contain information such as:

- File type
- Permissions
- Owner
- Group
- File size
- Timestamps
- Links to data blocks

Check inode:

```bash
ls -i file.txt
```

Example:

```text
123456 file.txt
```

Here:

```text
123456
```

is the inode number.

---

# 52. What is Actually Stored on Disk?

Simplified concept:

```text
Filename
   ↓
Directory entry
   ↓
Inode
   ↓
Data blocks
   ↓
Actual file content
```

Students don't need filesystem-internals depth initially, but this concept is useful later.

---

# 53. Linux File System Types

Linux supports multiple filesystems.

Common examples:

### ext4

Very common general-purpose Linux filesystem.

```text
ext4
```

### XFS

Common in enterprise Linux environments.

```text
XFS
```

### Btrfs

Advanced filesystem with features such as snapshots and subvolumes.

```text
Btrfs
```

### tmpfs

Memory-backed temporary filesystem commonly used for runtime/temporary data.

---

# 54. Check File System Type

Use:

```bash
df -T
```

Example:

```text
Filesystem     Type
/dev/sda1      ext4
```

---

# 55. `df`

`df` means:

> Disk Free

Check disk space:

```bash
df -h
```

`-h` means human-readable.

Example:

```text
Filesystem      Size  Used Avail Use%
/dev/sda1        50G   20G   30G  40%
```

Very important for DevOps troubleshooting.

---

# 56. `du`

`du` means:

> Disk Usage

Check directory size:

```bash
du -sh /var/log
```

Check current directory:

```bash
du -sh .
```

Find which directories consume space:

```bash
du -h --max-depth=1 /
```

Exact supported options can vary by implementation, so students should check:

```bash
du --help
```

---

# 57. Mounting

A filesystem must be **mounted** before its contents can normally be accessed through a directory.

Concept:

```text
Disk/Partition
      ↓
Filesystem
      ↓
Mount
      ↓
Directory
```

Example:

```bash
sudo mount /dev/sdb1 /mnt
```

Now:

```text
/dev/sdb1
     ↓
   /mnt
```

---

# 58. Check Mounted Filesystems

Use:

```bash
mount
```

or:

```bash
findmnt
```

And:

```bash
df -h
```

For a cleaner filesystem overview:

```bash
lsblk
```

---

# 59. `lsblk`

Very important for DevOps and cloud servers.

```bash
lsblk
```

It shows block devices and their relationships.

Example:

```text
NAME   SIZE TYPE MOUNTPOINT
sda     50G disk
└─sda1  50G part /
sdb    100G disk
```

This helps you understand:

```text
Disk
 ↓
Partition
 ↓
Filesystem
 ↓
Mount point
```

---

# 60. `/etc/fstab`

This file contains filesystem mount configuration.

```bash
cat /etc/fstab
```

It can define filesystems that should be mounted automatically.

For example:

```text
Device/UUID
Mount point
Filesystem type
Options
Dump
Pass
```

### Important warning

A mistake in `/etc/fstab` can prevent normal booting or cause mount problems.

So always edit carefully and validate your configuration.

---

# 61. Symbolic Links

A symbolic link is like a pointer/reference to another path.

Create:

```bash
ln -s /var/log/app.log app.log
```

Now:

```text
app.log
   ↓
/var/log/app.log
```

Check:

```bash
ls -l
```

You may see:

```text
app.log -> /var/log/app.log
```

---

# 62. Hard Link

A hard link is another directory entry referring to the same underlying inode/data.

Example:

```bash
ln file1.txt file2.txt
```

For beginners:

```text
Symbolic link → points to a path
Hard link     → another name for the same inode
```

Hard links have restrictions, such as generally not crossing filesystem boundaries and normally not being created for directories.

---

# 63. Permissions Connection

Now connect this lecture to the previous one.

Suppose:

```bash
ls -l project.txt
```

Output:

```text
-rw-r----- 1 sahil developers 1000 project.txt
```

We have:

```text
File
 ↓
Owner = sahil
 ↓
Group = developers
 ↓
Permissions
```

So today's topic is directly connected to:

```text
Users
 ↓
Groups
 ↓
Files
 ↓
Directories
 ↓
Ownership
 ↓
Permissions
```

---

# 64. Practical Lab

Give students this task.

### Step 1

Check where you are:

```bash
pwd
```

### Step 2

Explore root:

```bash
ls /
```

### Step 3

Explore `/etc`:

```bash
ls /etc
```

### Step 4

Go to home:

```bash
cd ~
```

### Step 5

Create project:

```bash
mkdir linux-project
```

### Step 6

Enter it:

```bash
cd linux-project
```

### Step 7

Create directories:

```bash
mkdir frontend backend database logs
```

### Step 8

Create files:

```bash
touch frontend/index.html
touch frontend/style.css
touch backend/app.py
touch database/schema.sql
touch logs/app.log
```

Now structure:

```text
linux-project/
├── frontend/
│   ├── index.html
│   └── style.css
├── backend/
│   └── app.py
├── database/
│   └── schema.sql
└── logs/
    └── app.log
```

### Step 9

Check:

```bash
find .
```

### Step 10

Check permissions:

```bash
ls -l
```

---

# 65. DevOps Practical

Ask students to imagine:

```text
/opt/myapp
```

Create:

```bash
sudo mkdir -p /opt/myapp/{app,config,logs,backup}
```

Result:

```text
/opt/myapp
├── app
├── config
├── logs
└── backup
```

Create files:

```bash
sudo touch /opt/myapp/app/app.py
sudo touch /opt/myapp/config/app.conf
sudo touch /opt/myapp/logs/app.log
```

Then:

```bash
sudo ls -R /opt/myapp
```

Now students can see how a real application directory might be organized.

---

# 66. Troubleshooting Scenario

Give students this situation:

> **Your Linux server says "No space left on device". What will you check?**

They should think:

```bash
df -h
```

Then:

```bash
df -T
```

Then identify large directories:

```bash
du -h --max-depth=1 /
```

Check logs:

```bash
du -sh /var/log/*
```

This is a very practical DevOps troubleshooting flow.

---

# 67. Another Scenario

> **An application is failing. Where would you look for logs?**

Possible places:

```text
/var/log
```

and application-specific directories.

Commands:

```bash
ls /var/log
```

```bash
tail -f /var/log/<log-file>
```

---

# 68. Another Scenario

> **You don't know where `nginx.conf` is located.**

Use:

```bash
find /etc -name "nginx.conf"
```

or, if appropriate:

```bash
sudo find / -name "nginx.conf" 2>/dev/null
```

Explain:

```text
2>/dev/null
```

redirects error messages away from the terminal.

---

# 69. Important Commands Cheat Sheet

| Command | Purpose |
|---|---|
| `pwd` | Show current directory |
| `ls` | List contents |
| `ls -l` | Detailed listing |
| `ls -a` | Show hidden files |
| `ls -lh` | Human-readable sizes |
| `cd` | Change directory |
| `mkdir` | Create directory |
| `touch` | Create/update file |
| `cat` | Display file |
| `less` | Read file interactively |
| `head` | Show beginning |
| `tail` | Show end |
| `tail -f` | Follow changing log |
| `cp` | Copy |
| `mv` | Move/rename |
| `rm` | Remove |
| `find` | Search files/directories |
| `file` | Identify file type |
| `df -h` | Disk space |
| `df -T` | Disk space + filesystem type |
| `du -sh` | Directory size |
| `lsblk` | Block devices |
| `mount` | Show/manage mounts |
| `findmnt` | Show mounted filesystems |
| `ln -s` | Create symbolic link |

---

# 70. What Students Must Remember

At the end, put this on the board:

```text
Linux
 │
 └── /
     │
     ├── etc    → Configuration
     ├── home   → Users
     ├── var    → Logs / changing data
     ├── tmp    → Temporary data
     ├── usr    → Programs / resources
     ├── opt    → Optional applications
     ├── dev    → Devices
     ├── proc   → Processes / kernel information
     ├── sys    → Kernel / devices
     ├── boot   → Boot files
     └── root   → Root user's home
```

And this:

```
Storage
   ↓
Filesystem
   ↓
Mount
   ↓
Directory
   ↓
File
   ↓
Ownership
   ↓
Permissions
   ↓
User Access
```
