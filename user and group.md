# Linux User & Group Management — Detailed Lecture

## 1. First Question: What is a User in Linux?

Before learning commands, students should understand **why Linux even has users**.

A **user** in Linux represents an identity that can log in to the system and perform actions.

For example, imagine a Linux server used by a company:

```text
Linux Server
│
├── root
├── sahil
├── developer1
├── developer2
├── tester1
└── nginx
```

Each identity can have different:

- Login access
- Files
- Permissions
- Ownership
- Privileges
- Processes
- Home directory
- Shell

### Simple real-life example

Think about a college computer lab.

There may be:

```text
Admin
Teacher
Student
Guest
```

Everyone uses the **same computer**, but everyone should not have the same access.

For example:

```text
Admin    → Can install software
Teacher  → Can manage files
Student  → Can use applications
Guest    → Limited access
```

Linux works in a similar way.

---

# 2. Why Does Linux Need Users?

Linux is designed as a **multi-user operating system**.

That means multiple users can use the same system while having controlled access.

### Without users

Suppose everyone had complete access:

```text
developer
   ↓
delete system files
   ↓
change configurations
   ↓
install anything
   ↓
break server
```

This would be dangerous.

With users:

```text
User
 ↓
Identity
 ↓
Permissions
 ↓
Allowed actions
```

Linux can control:

> Who can access what?

---

# 3. Why Do We Need User Management?

User management means:

> Creating, modifying, controlling, and deleting user accounts.

For example, when a new developer joins a company:

```text
New Employee
      ↓
Create Linux account
      ↓
Assign password/access
      ↓
Add to required groups
      ↓
Give required permissions
```

When the employee leaves:

```text
Employee leaves
      ↓
Disable/delete account
      ↓
Remove access
```

This is extremely important in:

- Linux servers
- AWS EC2
- DevOps
- Cloud environments
- Web servers
- Database servers
- CI/CD servers
- Production environments

---

# 4. Authentication vs Authorization

This is a **very important interview concept**.

## Authentication

Authentication answers:

> **Who are you?**

Example:

```text
Username: sahil
Password: ********
```

Linux checks your identity.

```text
Are you really Sahil?
       ↓
      YES
       ↓
Authentication successful
```

---

## Authorization

Authorization answers:

> **What are you allowed to do?**

For example:

```text
Sahil
 ↓
Can read project files
Can modify project files
Cannot modify /etc/passwd
Cannot restart some services
```

So:

```text
Authentication = Who are you?

Authorization = What can you do?
```

### Easy example

When you enter your college:

```text
ID Card → Authentication
Classroom permission → Authorization
```

---

# 5. What is a Group?

A **group** is a collection of users.

Instead of managing permissions for every user individually, Linux allows us to create a group.

Example:

```text
developers
│
├── sahil
├── developer1
└── developer2
```

Another group:

```text
testers
│
├── tester1
└── tester2
```

Now we can give permissions to the group.

Instead of:

```text
Give permission to sahil
Give permission to developer1
Give permission to developer2
```

we can do:

```text
Give permission to developers group
```

All members get the group-level access.

---

# 6. Why Are Groups Needed?

Imagine a company has:

```text
100 developers
50 testers
20 administrators
```

Suppose developers need access to:

```text
/opt/project
```

Without groups:

```text
User1 → permission
User2 → permission
User3 → permission
...
User100 → permission
```

Very difficult.

With groups:

```text
developers
     ↓
100 users
     ↓
/opt/project
```

Much easier.

### Main benefit

> **Groups make permission management easier and scalable.**

---

# 7. User + Group Relationship

The basic Linux access model can be understood as:

```text
USER
 ↓
GROUP
 ↓
OWNERSHIP
 ↓
PERMISSIONS
 ↓
ACCESS
```

For example:

```text
sahil
  ↓
developers
  ↓
/var/www/project
  ↓
read + write
```

---

# 8. Types of Linux Users

Linux generally has different categories of users.

The important ones for students are:

1. Root user
2. Regular/normal users
3. System users
4. Service users

---

# 9. Root User

The **root user** is the superuser in Linux.

Root has extremely high privileges.

Root's UID is:

```text
0
```

You can verify:

```bash
id root
```

Example:

```text
uid=0(root) gid=0(root) groups=0(root)
```

---

## Root's Privileges

Root can generally:

- Create users
- Delete users
- Change passwords
- Install software
- Remove software
- Modify system configuration
- Start/stop services
- Access protected files
- Change ownership
- Change permissions
- Mount filesystems
- Manage processes

Example:

```bash
useradd developer1
```

Normally this requires administrative privileges.

So we use:

```bash
sudo useradd developer1
```

or become root:

```bash
su -
```

---

# 10. Why Should We NOT Always Use Root?

This is an important security concept.

Suppose you are working as root:

```bash
rm -rf /some/path
```

A wrong command can cause serious damage.

Also, if an attacker gets root access:

```text
Attacker
   ↓
Root access
   ↓
Entire server compromised
```

Therefore:

> **Use the minimum privileges required to perform the task.**

This concept is called:

### Principle of Least Privilege

---

# 11. Regular Users

Regular users are normal human accounts.

Examples:

```text
sahil
developer1
tester1
student1
```

They usually have:

- UID greater than 0
- Home directory
- Login shell
- Limited privileges

Example:

```bash
id sahil
```

Possible output:

```text
uid=1000(sahil) gid=1000(sahil) groups=1000(sahil)
```

Here:

```text
uid=1000
```

means the user's ID is 1000.

---

# 12. What is UID?

UID means:

> **User ID**

Linux internally identifies users using numbers.

For example:

```text
sahil → UID 1000
developer1 → UID 1001
tester1 → UID 1002
```

Linux doesn't rely only on usernames.

Internally:

```text
sahil
 ↓
UID 1000
```

---

# 13. Why is UID Important?

Suppose a file belongs to:

```text
UID 1000
```

Linux can associate that UID with the corresponding user.

You can see UID using:

```bash
id username
```

Example:

```bash
id sahil
```

Output:

```text
uid=1000(sahil)
gid=1000(sahil)
```

---

# 14. GID

GID means:

> **Group ID**

Just like users have UIDs, groups have GIDs.

Example:

```text
developers → GID 1005
```

Check using:

```bash
getent group developers
```

Example:

```text
developers:x:1005:sahil,developer1
```

Here:

```text
1005 = GID
```

---

# 15. System Users

System users are generally created for system processes or applications rather than normal human login.

Examples can include:

```text
www-data
nginx
mysql
redis
daemon
```

These users help services run with controlled permissions.

For example:

```text
Nginx
 ↓
www-data
 ↓
Web files
```

Instead of running the web server as root, it can run under a restricted service identity.

This improves security.

---

# 16. Service Users

A service user is an account used by an application/service.

Example:

```text
nginx → nginx user
MySQL → mysql user
Redis → redis user
```

Conceptually:

```text
Application
     ↓
Service User
     ↓
Limited permissions
```

This prevents the service from automatically getting unnecessary root-level access.

---

# 17. Check Current User

Command:

```bash
whoami
```

Meaning:

> Who am I currently logged in as?

Example:

```bash
whoami
```

Output:

```text
sahil
```

If root:

```text
root
```

### When do we use it?

When you are unsure which account you're currently using.

---

# 18. `id` Command

The `id` command gives detailed information about a user.

```bash
id
```

Example:

```text
uid=1000(sahil) gid=1000(sahil) groups=1000(sahil),10(wheel),1005(developers)
```

Let's understand it:

```text
uid=1000(sahil)
```

User ID.

```text
gid=1000(sahil)
```

Primary group ID.

```text
groups=...
```

Groups the user belongs to.

---

# 19. Check Another User

```bash
id developer1
```

Example:

```text
uid=1001(developer1) gid=1001(developer1) groups=1001(developer1),1005(developers)
```

---

# 20. `who` Command

```bash
who
```

Shows users currently logged into the system.

Example:

```text
sahil    pts/0    2026-10-13 10:30
admin    pts/1    2026-10-13 10:45
```

Useful on multi-user servers.

---

# 21. `w` Command

```bash
w
```

Shows:

- Logged-in users
- Login time
- Idle time
- Current activity

Example:

```text
USER    TTY    FROM       LOGIN@   WHAT
sahil   pts/0  10.0.0.10 10:30    vim app.js
```

---

# 22. Important Linux User Files

Linux stores user and group information in important configuration files.

The four important files are:

```text
/etc/passwd
/etc/shadow
/etc/group
/etc/gshadow
```

---

# 23. `/etc/passwd`

This file contains information about user accounts.

View it:

```bash
cat /etc/passwd
```

A line might look like:

```text
sahil:x:1000:1000:Sahil:/home/sahil:/bin/bash
```

Format:

```text
username : password-placeholder : UID : GID : comment : home : shell
```

Let's understand:

```text
sahil
```

Username.

```text
x
```

Password information is stored separately in `/etc/shadow`.

```text
1000
```

UID.

```text
1000
```

Primary GID.

```text
Sahil
```

User information/comment.

```text
/home/sahil
```

Home directory.

```text
/bin/bash
```

Login shell.

---

# 24. `/etc/shadow`

This file contains password-related authentication information.

```bash
sudo cat /etc/shadow
```

Important:

> Normal users should not have unrestricted access to this file.

It is protected because it contains sensitive password hashes and password-aging information.

---

# 25. `/etc/group`

This file contains group information.

```bash
cat /etc/group
```

Example:

```text
developers:x:1005:sahil,developer1,developer2
```

Meaning:

```text
Group name = developers
GID = 1005
Members = sahil, developer1, developer2
```

---

# 26. `/etc/gshadow`

This contains protected group-related information.

```bash
sudo cat /etc/gshadow
```

For beginners, remember:

```text
/etc/passwd  → User account information
/etc/shadow  → Password/authentication information

/etc/group   → Group information
/etc/gshadow → Protected group information
```

---

# 27. Creating a User

The main command is:

```bash
useradd
```

Example:

```bash
sudo useradd developer1
```

But for a normal interactive user, it's usually better to create a home directory and specify a shell:

```bash
sudo useradd -m -s /bin/bash developer1
```

Here:

```text
-m → create home directory
-s → specify login shell
```

So:

```text
developer1
    ↓
/home/developer1
    ↓
/bin/bash
```

---

# 28. `adduser`

On some Linux distributions, you may also see:

```bash
sudo adduser developer1
```

`adduser` is generally a more interactive, user-friendly command on distributions that provide it.

It may ask:

```text
New password:
Full Name:
Room Number:
Work Phone:
```

For teaching:

```text
useradd → lower-level/common administrative command

adduser → interactive helper on supported distributions
```

---

# 29. Set User Password

After creating a user:

```bash
sudo passwd developer1
```

You'll be asked to enter the password.

---

# 30. Create a User Step-by-Step

Complete example:

```bash
sudo useradd -m -s /bin/bash developer1
sudo passwd developer1
```

Now verify:

```bash
id developer1
```

and:

```bash
ls -ld /home/developer1
```

---

# 31. Modify a User

Command:

```bash
usermod
```

This is one of the most important commands.

Syntax:

```bash
sudo usermod [options] username
```

---

# 32. Change User's Home Directory

```bash
sudo usermod -d /home/newhome developer1
```

But if you also want to move existing home contents:

```bash
sudo usermod -m -d /home/newhome developer1
```

Here:

```text
-d → new home directory
-m → move existing home contents
```

---

# 33. Change Login Shell

```bash
sudo usermod -s /bin/bash developer1
```

Another example:

```bash
sudo usermod -s /bin/zsh developer1
```

---

# 34. Add User to Group

Very important command:

```bash
sudo usermod -aG developers developer1
```

Meaning:

```text
-a → append
-G → supplementary groups
```

So:

```text
developer1
     ↓
developers group
```

### IMPORTANT

Do not casually use:

```bash
usermod -G developers developer1
```

because `-G` without `-a` can replace the user's existing supplementary groups.

Prefer:

```bash
usermod -aG group user
```

---

# 35. Verify Group Membership

```bash
groups developer1
```

or:

```bash
id developer1
```

---

# 36. Remove User From Group

One method:

```bash
sudo gpasswd -d developer1 developers
```

Then verify:

```bash
groups developer1
```

---

# 37. Delete a User

Basic:

```bash
sudo userdel developer1
```

This removes the account but may leave the home directory.

To remove the account and its home directory:

```bash
sudo userdel -r developer1
```

Be careful with `-r`.

It can remove the user's home directory and associated mail spool.

---

# 38. Lock a User

If you want to temporarily prevent password-based login:

```bash
sudo passwd -l developer1
```

`-l` means lock.

Check:

```bash
sudo passwd -S developer1
```

---

# 39. Unlock a User

```bash
sudo passwd -u developer1
```

`-u` means unlock.

---

# 40. Groups

Now move from **users → groups**.

Create group:

```bash
sudo groupadd developers
```

Create another:

```bash
sudo groupadd testers
```

---

# 41. Check Group

```bash
getent group developers
```

Example:

```text
developers:x:1005:
```

Here:

```text
developers → group name
1005 → GID
```

---

# 42. Add Users to Group

```bash
sudo usermod -aG developers sahil
```

```bash
sudo usermod -aG developers developer1
```

```bash
sudo usermod -aG developers developer2
```

Now:

```text
developers
│
├── sahil
├── developer1
└── developer2
```

---

# 43. Primary Group vs Secondary Group

This is important.

### Primary Group

Every user has a primary group.

Example:

```text
sahil
 ↓
Primary group = sahil
```

You can see it:

```bash
id sahil
```

Example:

```text
uid=1000(sahil) gid=1000(sahil)
```

The `gid` shown there is the primary group.

---

## Secondary Groups

Additional groups are called supplementary/secondary groups.

Example:

```text
sahil
│
├── Primary → sahil
├── developers
├── docker
└── testers
```

Check:

```bash
id sahil
```

---

# 44. Change Primary Group

Suppose:

```text
sahil → primary group = sahil
```

We want:

```text
sahil → primary group = developers
```

Command:

```bash
sudo usermod -g developers sahil
```

Remember:

```text
-g → primary group
-G → supplementary groups
```

Very important difference.

---

# 45. `su` Command

`su` means:

> Switch User

Example:

```bash
su developer1
```

This switches to `developer1`.

To switch with the user's login environment:

```bash
su - developer1
```

The `-` is important because it starts a login shell and loads the target user's environment.

Check:

```bash
whoami
```

---

# 46. `sudo`

`sudo` means:

> Run a command with another user's privileges, usually root.

Example:

```bash
sudo systemctl restart nginx
```

You remain logged in as your normal user, but the specific command runs with elevated privileges.

This is generally safer than working as root continuously.

---

# 47. `su` vs `sudo`

### `su`

```bash
su - root
```

You switch to another account.

### `sudo`

```bash
sudo systemctl restart nginx
```

You execute a particular command with elevated privileges.

Easy way to remember:

```text
su    → Switch User

sudo  → Execute command with elevated privilege
```

---

# 48. Sudoers

Linux controls who can use `sudo`.

Configuration is commonly managed through:

```text
/etc/sudoers
```

Do not casually edit it with a normal text editor.

Use:

```bash
sudo visudo
```

Why?

Because `visudo` checks the syntax before saving, reducing the risk of breaking sudo configuration.

---

# 49. Example Sudo Permission

Conceptually, you can give a user administrative access through appropriate sudo configuration.

For example:

```text
developer1 → allowed to restart nginx
```

Rather than:

```text
developer1 → unrestricted root access
```

This follows the principle of least privilege.

---

# 50. User and File Ownership

Now connect today's topic with your previous **Linux permissions** lecture.

Suppose:

```bash
ls -l project.txt
```

Output:

```text
-rw-r----- 1 sahil developers 1200 project.txt
```

Understand:

```text
sahil
   ↓
Owner

developers
   ↓
Group owner
```

Linux permissions then determine:

```text
Owner       → rw-
Group       → r--
Others      → ---
```

So user/group management directly connects with file permissions.

---

# 51. `chown`

`chown` means:

> Change ownership.

Example:

```bash
sudo chown developer1 project.txt
```

Now:

```text
developer1
    ↓
Owner
```

---

# 52. Change Owner and Group Together

```bash
sudo chown developer1:developers project.txt
```

Meaning:

```text
Owner → developer1
Group → developers
```

This is very commonly used in DevOps.

---

# 53. Change Group Only

```bash
sudo chgrp developers project.txt
```

Now the group owner becomes:

```text
developers
```

---

# 54. Recursive Ownership

Suppose:

```text
/var/www/project
```

contains hundreds of files.

You want everything to belong to:

```text
developer1:developers
```

Use:

```bash
sudo chown -R developer1:developers /var/www/project
```

Here:

```text
-R → recursive
```

Meaning apply to the directory and its contents.

---

# 55. Real-World DevOps Example

Imagine an AWS EC2 server.

```text
EC2 Linux Server
       │
       ├── developers
       │      ├── dev1
       │      └── dev2
       │
       ├── testers
       │      └── tester1
       │
       └── nginx
```

Project:

```text
/var/www/project
```

We can configure:

```text
developers group
       ↓
Project files
       ↓
Read/Write access
```

But testers might only need:

```text
Read access
```

And the web server might use:

```text
nginx/www-data
```

This is much better than giving everyone root access.

---

# 56. Docker Example

In many Linux environments, users may need Docker access.

For example:

```bash
sudo usermod -aG docker developer1
```

Then the user may need to log out and log back in for the new group membership to be reflected in the session.

Verify:

```bash
groups developer1
```

---

# 57. Complete Practical Lab

Now I would give students this practical.

## Step 1 — Create groups

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

## Step 2 — Create users

```bash
sudo useradd -m -s /bin/bash developer1
sudo useradd -m -s /bin/bash developer2
sudo useradd -m -s /bin/bash tester1
```

---

## Step 3 — Set passwords

```bash
sudo passwd developer1
sudo passwd developer2
sudo passwd tester1
```

---

## Step 4 — Add developers to developers group

```bash
sudo usermod -aG developers developer1
sudo usermod -aG developers developer2
```

---

## Step 5 — Add tester

```bash
sudo usermod -aG testers tester1
```

---

## Step 6 — Verify

```bash
id developer1
id developer2
id tester1
```

---

## Step 7 — Create project directory

```bash
sudo mkdir -p /opt/project
```

---

## Step 8 — Change group ownership

```bash
sudo chown root:developers /opt/project
```

---

## Step 9 — Give group access

For example:

```bash
sudo chmod 770 /opt/project
```

Now conceptually:

```
root        → full access
developers  → full access
others      → no access
```

---

# 58. Important Commands Cheat Sheet

| Command | Purpose |
|---|---|
| `whoami` | Current user |
| `id` | User UID/GID/groups |
| `who` | Logged-in users |
| `w` | Logged-in users + activity |
| `useradd` | Create user |
| `adduser` | Interactive user creation on supported systems |
| `passwd` | Set/change password |
| `usermod` | Modify user |
| `userdel` | Delete user |
| `groupadd` | Create group |
| `groupdel` | Delete group |
| `groups` | Show user's groups |
| `getent group` | Query group information |
| `su` | Switch user |
| `sudo` | Execute command with elevated privileges |
| `visudo` | Safely edit sudo configuration |
| `chown` | Change owner/group ownership |
| `chgrp` | Change group ownership |
| `passwd -l` | Lock account password |
| `passwd -u` | Unlock account password |
| `chage` | Password aging/account expiry management |

---

# 59. Commands Students Must Remember

If I were taking the lecture, I'd tell students to remember these first:

```
whoami
id
who
w

sudo useradd -m -s /bin/bash username
sudo passwd username

sudo usermod -aG group username

sudo userdel -r username

sudo groupadd groupname
sudo groupdel groupname

groups username

su - username
sudo command

sudo visudo

sudo chown user:group file
sudo chgrp group file
```

---

# 60. The Big Picture

Students should leave today's lecture understanding this:

```
                 LINUX SYSTEM
                      │
          ┌───────────┴───────────┐
          │                       │
        USERS                   GROUPS
          │                       │
    ┌─────┼─────┐          ┌──────┼──────┐
    │     │     │          │      │      │
  root  dev1  tester     devs   testers admins
    │     │     │          │
    └─────┴─────┴──────────┘
              │
          OWNERSHIP
              │
         PERMISSIONS
              │
            ACCESS
```

And the core concept is:

> **User identifies who you are. Group organizes users. Ownership identifies who owns a resource. Permissions decide what they can do.**

---

# 61. Interview Questions for Today's Lecture

### Beginner

1. What is a user in Linux?
2. Why does Linux need users?
3. What is a group?
4. Why are groups used?
5. What is the root user?
6. What is UID?
7. What is GID?
8. What is the difference between a user and a group?
9. What is `/etc/passwd`?
10. What is `/etc/shadow`?
11. What is `/etc/group`?

### Intermediate

12. How do you create a user?
13. How do you delete a user?
14. How do you add a user to a group?
15. What is the difference between `-g` and `-G`?
16. Why is `-a` used with `-G`?
17. What is a primary group?
18. What is a secondary group?
19. What is the difference between `su` and `sudo`?
20. What is `/etc/sudoers`?
21. Why should we use `visudo`?
22. How do you lock a user?
23. How do you unlock a user?
24. How do you change file ownership?
25. Difference between `chown` and `chgrp`?

### DevOps-level

26. Why shouldn't applications run as root?
27. Why are service accounts used?
28. How would you give developers access to a project directory?
29. How would you remove a developer's access when they leave?
30. Why is group-based permission management better than individual permissions?
31. What is the Principle of Least Privilege?
32. Why would you use a dedicated user for Nginx/MySQL/Redis?
33. How would you troubleshoot a user who cannot access a file even though they belong to the expected group?

---

## Final Revision Formula

Make students remember this one line:

```
USER
 ↓
UID
 ↓
GROUP
 ↓
GID
 ↓
OWNERSHIP
 ↓
PERMISSIONS
 ↓
ACCESS
```

And in a real DevOps environment:

```
Employee
   ↓
Linux User
   ↓
Required Groups
   ↓
Required Permissions
   ↓
Required Resources
   ↓
No unnecessary Root Access
```
