# Linux Users & Groups — Practical Guide

> Beginner → Intermediate practical guide for Linux user and group management.

## 1. What are Users and Groups?

Linux is a multi-user operating system. Users identify people or services, while groups make permission management easier.

```text
User → Group → Permissions → Files / Directories
```

### Real-life example

A company may have:

```text
developers
  ├── rahul
  ├── priya
  └── amit

devops
  ├── sahil
  └── neha

database
  ├── rohan
  └── pooja
```

Instead of giving permissions to every person individually, permissions can be assigned to groups.

---

# 2. Important Linux Account Files

| File | Purpose |
|---|---|
| `/etc/passwd` | Basic user account information |
| `/etc/shadow` | Password/authentication information |
| `/etc/group` | Group information |
| `/etc/gshadow` | Group authentication information |

View users:

```bash
cat /etc/passwd
```

List only usernames:

```bash
cut -d: -f1 /etc/passwd
```

View groups:

```bash
cat /etc/group
```

---

# 3. Check Current User

```bash
whoami
```

Example:

```text
ec2-user
```

Check detailed information:

```bash
id
```

Check another user:

```bash
id rahul
```

---

# 4. Create a User

Basic:

```bash
sudo useradd rahul
```

Recommended for a normal login user:

```bash
sudo useradd -m -s /bin/bash rahul
```

Explanation:

```text
-m        → create home directory
-s        → choose login shell
/bin/bash → Bash shell
rahul     → username
```

The home directory will normally be:

```text
/home/rahul
```

Verify:

```bash
id rahul
ls /home
```

---

# 5. Set User Password

```bash
sudo passwd rahul
```

Test the user:

```bash
su - rahul
```

Then:

```bash
whoami
pwd
```

Exit:

```bash
exit
```

---

# 6. Create a Group

```bash
sudo groupadd developers
```

Verify:

```bash
getent group developers
```

Create more groups:

```bash
sudo groupadd devops
sudo groupadd testers
sudo groupadd database
```

---

# 7. Add a User to a Group

Use:

```bash
sudo usermod -aG developers rahul
```

Explanation:

```text
usermod → modify user
-a       → append
-G       → supplementary group
```

Verify:

```bash
id rahul
```

or:

```bash
groups rahul
```

## Important: Why `-a`?

Prefer:

```bash
sudo usermod -aG developers rahul
```

When adding a group.

Be careful with:

```bash
sudo usermod -G developers rahul
```

because `-G` without `-a` can replace the user's existing supplementary group memberships.

---

# 8. Add a User to Multiple Groups

```bash
sudo usermod -aG developers,devops rahul
```

Verify:

```bash
id rahul
```

---

# 9. Add Multiple Users to a Group

```bash
sudo usermod -aG developers rahul
sudo usermod -aG developers priya
sudo usermod -aG developers amit
```

Verify:

```bash
groups rahul
groups priya
groups amit
```

---

# 10. Remove a User From a Group

A clear method is:

```bash
sudo gpasswd -d rahul developers
```

Verify:

```bash
groups rahul
```

If the user was already logged in, start a new login session before checking the normal shell's group list.

---

# 11. Rename a User

Syntax:

```bash
sudo usermod -l NEW_NAME OLD_NAME
```

Example:

```bash
sudo usermod -l rohit rohan
```

Verify:

```bash
id rohit
```

### Important

Avoid renaming a user while that user is actively logged in. For production, use another administrative account and verify active sessions/processes first.

---

# 12. Rename/Move a User's Home Directory

After renaming a user, you may also want the home directory to match.

Example:

```bash
sudo usermod -d /home/rohit -m rohit
```

Explanation:

```text
-d /home/rohit → new home directory
-m             → move existing home contents
```

Verify:

```bash
getent passwd rohit
ls /home
```

---

# 13. Change a User's Login Shell

```bash
sudo usermod -s /bin/bash rahul
```

Verify:

```bash
getent passwd rahul
```

---

# 14. Lock a User

Temporarily lock password authentication:

```bash
sudo usermod -L rahul
```

Check:

```bash
sudo passwd -S rahul
```

---

# 15. Unlock a User

```bash
sudo usermod -U rahul
```

Check:

```bash
sudo passwd -S rahul
```

---

# 16. Delete a User

Delete account:

```bash
sudo userdel rahul
```

Delete account and its home directory:

```bash
sudo userdel -r rahul
```

### WARNING

`userdel -r` can permanently remove the user's home-directory data. Back up or transfer important files before using it on a real server.

---

# 17. Delete a Group

```bash
sudo groupdel developers
```

Verify:

```bash
getent group developers
```

Deleting a group does not automatically delete files that were associated with that group's numeric GID.

---

# 18. Check Whether a User Exists

```bash
id rahul
```

or:

```bash
getent passwd rahul
```

---

# 19. Check Whether a Group Exists

```bash
getent group developers
```

---

# 20. Sudo Access

Some Linux distributions use the `sudo` group for administrative access. Amazon Linux commonly uses `wheel`.

For Amazon Linux, if the system is configured to grant sudo through `wheel`:

```bash
sudo usermod -aG wheel rahul
```

Then start a new login session and test:

```bash
sudo whoami
```

Expected:

```text
root
```

On Debian/Ubuntu systems, the administrative group is commonly:

```bash
sudo
```

Example:

```bash
sudo usermod -aG sudo rahul
```

Always verify your distribution's sudo configuration before changing privileged access.

---

# 21. Real-Life Example — Developer Team

Create users:

```bash
sudo useradd -m -s /bin/bash rahul
sudo useradd -m -s /bin/bash priya
sudo useradd -m -s /bin/bash amit
```

Set passwords:

```bash
sudo passwd rahul
sudo passwd priya
sudo passwd amit
```

Create group:

```bash
sudo groupadd developers
```

Add users:

```bash
sudo usermod -aG developers rahul
sudo usermod -aG developers priya
sudo usermod -aG developers amit
```

Verify:

```bash
groups rahul
groups priya
groups amit
```

---

# 22. Real-Life Example — DevOps Team

Create group:

```bash
sudo groupadd devops
```

Create user:

```bash
sudo useradd -m -s /bin/bash sahil
```

Add user:

```bash
sudo usermod -aG devops sahil
```

If administrative access is required and the system uses `wheel`:

```bash
sudo usermod -aG wheel sahil
```

Verify:

```bash
id sahil
```

---

# 23. Real-Life Example — Shared Project Directory

Create directory:

```bash
sudo mkdir -p /opt/project
```

Create group:

```bash
sudo groupadd developers
```

Set group ownership:

```bash
sudo chown root:developers /opt/project
```

Give the group access and enable setgid:

```bash
sudo chmod 2775 /opt/project
```

Check:

```bash
ls -ld /opt/project
```

You may see:

```text
drwxrwsr-x
```

The `s` indicates the setgid bit.

Add developers:

```bash
sudo usermod -aG developers rahul
sudo usermod -aG developers priya
```

Start a new login session for the users so their new group membership is reflected.

---

# 24. Users + Groups + Permissions

Example:

```bash
ls -l index.html
```

Possible output:

```text
-rw-r--r-- 1 rahul developers 1200 Oct 17 index.html
```

Here:

```text
Owner = rahul
Group = developers
```

Linux permissions have three main subjects:

```text
Owner | Group | Others
```

Example:

```text
rwx | rwx | ---
```

Meaning:

```text
Owner  → read + write + execute
Group  → read + write + execute
Others → no permissions
```

---

# 25. Change File Owner

```bash
sudo chown rahul index.html
```

Change owner and group:

```bash
sudo chown rahul:developers index.html
```

Change only group:

```bash
sudo chgrp developers index.html
```

Check:

```bash
ls -l index.html
```

---

# 26. Complete Practical Lab

## Scenario

A company has:

```text
Development Team
DevOps Team
Testing Team
```

Create groups:

```text
developers
devops
testers
```

Create users:

```text
rahul
priya
amit
neha
rohan
```

---

## Step 1 — Create Groups

```bash
sudo groupadd developers
sudo groupadd devops
sudo groupadd testers
```

Verify:

```bash
getent group developers
getent group devops
getent group testers
```

---

## Step 2 — Create Users

```bash
sudo useradd -m -s /bin/bash rahul
sudo useradd -m -s /bin/bash priya
sudo useradd -m -s /bin/bash amit
sudo useradd -m -s /bin/bash neha
sudo useradd -m -s /bin/bash rohan
```

---

## Step 3 — Set Passwords

```bash
sudo passwd rahul
sudo passwd priya
sudo passwd amit
sudo passwd neha
sudo passwd rohan
```

---

## Step 4 — Assign Groups

Developers:

```bash
sudo usermod -aG developers rahul
sudo usermod -aG developers priya
```

DevOps:

```bash
sudo usermod -aG devops amit
sudo usermod -aG devops neha
```

Testing:

```bash
sudo usermod -aG testers rohan
```

---

## Step 5 — Verify

```bash
id rahul
id priya
id amit
id neha
id rohan
```

Also:

```bash
groups rahul
groups priya
groups amit
groups neha
groups rohan
```

---

# 27. Organization Change Example

Suppose Amit moves from DevOps to Developers.

Remove from DevOps:

```bash
sudo gpasswd -d amit devops
```

Add to Developers:

```bash
sudo usermod -aG developers amit
```

Verify:

```bash
groups amit
```

---

# 28. Employee Offboarding Example

Suppose Amit leaves the company.

First inspect:

```bash
id amit
groups amit
```

Remove access from relevant groups:

```bash
sudo gpasswd -d amit developers
sudo gpasswd -d amit devops
```

Then, if company policy allows account deletion:

```bash
sudo userdel -r amit
```

In a real organization, follow the account-offboarding, backup, audit, and data-retention policy before deleting anything.

---

# 29. Practical Assignment

Complete this yourself before checking any solution.

### Task 1

Create groups:

```text
frontend
backend
devops
```

### Task 2

Create users:

```text
frontend1
frontend2
backend1
backend2
devops1
```

### Task 3

Assign:

```text
frontend1 → frontend
frontend2 → frontend
backend1  → backend
backend2  → backend
devops1   → devops
```

### Task 4

Add `backend1` to `devops` as well.

### Task 5

Remove `backend2` from `backend`.

### Task 6

Rename:

```text
frontend1 → webadmin
```

### Task 7

Create:

```text
/opt/company-app
```

Make `backend` the group owner.

### Task 8

Verify:

```bash
id username
groups username
getent group groupname
ls -ld /opt/company-app
```

### Task 9

Delete one test user and its home directory.

### Task 10

Delete a test group after confirming it is no longer required.

---

# 30. Troubleshooting

## Error: User already exists

```text
useradd: user 'rahul' already exists
```

Check:

```bash
id rahul
```

If it exists, don't create it again.

---

## Error: Group already exists

```text
groupadd: group 'developers' already exists
```

Check:

```bash
getent group developers
```

---

## New group is not visible

Run:

```bash
groups username
```

If the user was already logged in, start a new login session.

Then check again:

```bash
groups
```

---

## Permission denied

Check:

```bash
ls -l filename
id username
```

Then inspect:

- file owner
- group owner
- permissions
- user's group membership
- parent directory permissions

Use `sudo` only when elevated privileges are actually required.

---

# 31. Security Best Practices

### Individual accounts

Avoid shared accounts:

```text
bad:
everyone → admin
```

Prefer:

```text
rahul → individual account
priya → individual account
amit  → individual account
```

### Use groups

Manage team access with groups rather than manually changing permissions for every user.

### Least privilege

Give only the access required:

```text
Developer ≠ Database Administrator
Tester ≠ Server Administrator
```

### Protect privileged access

Do not give sudo access unnecessarily.

### Protect secrets

Never upload these to public GitHub repositories:

```text
Passwords
AWS access keys
AWS secret keys
SSH private keys
.pem files
.env files containing secrets
```

---

# 32. Command Cheat Sheet

| Task | Command |
|---|---|
| Current user | `whoami` |
| User information | `id username` |
| User groups | `groups username` |
| Create user | `sudo useradd username` |
| Create user + home | `sudo useradd -m username` |
| Create user + Bash | `sudo useradd -m -s /bin/bash username` |
| Set password | `sudo passwd username` |
| Create group | `sudo groupadd groupname` |
| Add user to group | `sudo usermod -aG group user` |
| Remove user from group | `sudo gpasswd -d user group` |
| Rename user | `sudo usermod -l new old` |
| Change/move home | `sudo usermod -d /new/home -m user` |
| Change shell | `sudo usermod -s /bin/bash user` |
| Lock user | `sudo usermod -L user` |
| Unlock user | `sudo usermod -U user` |
| Delete user | `sudo userdel user` |
| Delete user + home | `sudo userdel -r user` |
| Delete group | `sudo groupdel group` |
| List usernames | `cut -d: -f1 /etc/passwd` |
| List groups | `getent group` |
| Check group | `getent group groupname` |
| File ownership | `ls -l` |
| Change owner | `sudo chown user file` |
| Change owner + group | `sudo chown user:group file` |
| Change group | `sudo chgrp group file` |

---

# 33. Interview Questions

### Q1. What is a Linux user?

A user is an account used to identify and authenticate a person or service on a Linux system.

### Q2. What is a Linux group?

A group is a collection of users used to manage permissions collectively.

### Q3. How do you create a user?

```bash
sudo useradd -m username
```

### Q4. How do you create a group?

```bash
sudo groupadd developers
```

### Q5. How do you add a user to a group?

```bash
sudo usermod -aG developers username
```

### Q6. Why is `-a` important?

It appends the new supplementary group instead of replacing existing supplementary group memberships.

### Q7. How do you remove a user from a group?

```bash
sudo gpasswd -d username developers
```

### Q8. How do you delete a user?

```bash
sudo userdel username
```

### Q9. How do you delete a user and home directory?

```bash
sudo userdel -r username
```

### Q10. How do you check a user's groups?

```bash
groups username
```

or:

```bash
id username
```

---

# 34. Final Mental Model

Remember:

```text
USER
  ↓
belongs to
  ↓
GROUP
  ↓
GROUP has permissions
  ↓
USER gets access through GROUP
  ↓
FILES / DIRECTORIES
```

DevOps server model:

```text
Linux Server
│
├── Users
│   ├── Developers
│   ├── DevOps Engineers
│   └── Testers
│
├── Groups
│   ├── developers
│   ├── devops
│   └── testers
│
└── Permissions
    ├── Read
    ├── Write
    └── Execute
```

The practical DevOps workflow is:

```text
Create User
    ↓
Assign Group / Role
    ↓
Give Required Access
    ↓
Verify
    ↓
Modify When Role Changes
    ↓
Remove Access When No Longer Needed
```

> Practice destructive commands such as `userdel -r` and `groupdel` on a test EC2 instance before using them on production systems.
