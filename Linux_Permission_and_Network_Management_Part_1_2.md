# Linux Permission & Network Management — Part 1 and Part 2

> Beginner-friendly, industry-oriented Linux practical guide  
> Learning order: **Users & Groups → Permissions → Networking**

---

# PART 1 — Linux Permission Management

## 1. What is Permission Management?

Linux is a multi-user operating system. Multiple users, applications, services, and administrators can work on the same server.

Permission management controls:

- Who can access a file or directory
- What they can do
- Whether they can read, modify, execute, create, delete, or access resources

Basic model:

```text
User → Group → Ownership → Permissions → Access
```

## 2. Why Do We Need Permissions?

Imagine a production server:

```text
/opt/company-app/
├── app/
├── config/
├── logs/
├── backup/
└── scripts/
```

If everyone has full access, a tester could modify production code or an intern could read sensitive configuration.

With permissions:

```text
Developers → read/write application
Testers    → read application
Admins     → full access
Others     → no access
```

This follows the **Principle of Least Privilege**: give users only the access they need.

## 3. Real Industry Project Structure

Example e-commerce application:

```text
/opt/ecommerce/
├── application/
│   ├── app.py
│   └── requirements.txt
├── config/
│   ├── app.conf
│   └── database.conf
├── logs/
│   ├── app.log
│   └── error.log
├── backup/
└── scripts/
    ├── deploy.sh
    └── backup.sh
```

Users:

```text
dev1
dev2
tester1
ops1
```

Groups:

```text
developers
testers
operations
```

Possible access:

| Resource | Developers | Testers | Operations |
|---|---|---|---|
| application | Read/Write | Read | Full |
| config | Limited | Read | Full |
| logs | Read/Write | Read | Full |
| backup | No access | No access | Full |
| scripts | Execute | No access | Full |

---

## 4. Permission Types

```text
r = Read
w = Write
x = Execute
```

For files:

| Permission | Meaning |
|---|---|
| `r` | Read contents |
| `w` | Modify contents |
| `x` | Execute |

For directories:

| Permission | Meaning |
|---|---|
| `r` | List contents |
| `w` | Create/delete/rename entries |
| `x` | Enter/traverse |

## 5. Owner, Group and Others

Example:

```bash
ls -l app.py
```

Possible output:

```text
-rwxr-x---
```

Breakdown:

```text
-   rwx   r-x   ---
    │      │      │
    │      │      └── Others
    │      └───────── Group
    └──────────────── Owner
```

## 6. `ls -l`

```bash
ls -l
ls -la
```

Example:

```text
-rw-r----- 1 developer1 developers 1200 app.py
```

Important information:

```text
permissions
owner
group
size
filename
```

## 7. Permission Numbers

```text
r = 4
w = 2
x = 1
```

Therefore:

```text
rwx = 7
rw- = 6
r-x = 5
r-- = 4
--- = 0
```

Common examples:

```text
755 = rwxr-xr-x
644 = rw-r--r--
700 = rwx------
750 = rwxr-x---
770 = rwxrwx---
```

## 8. `chmod`

Change permissions:

```bash
chmod 644 app.conf
chmod 755 deploy.sh
chmod 640 database.conf
```

Symbolic mode:

```bash
chmod u+x deploy.sh
chmod g+w app.py
chmod o-r app.conf
chmod g-w app.py
chmod u+rwx,g+rx,o-rwx deploy.sh
```

Where:

```text
u = owner
g = group
o = others
a = all
```

## 9. `chown`

Change owner:

```bash
sudo chown developer1 app.py
```

Change owner and group:

```bash
sudo chown developer1:developers app.py
```

Recursive:

```bash
sudo chown -R developer1:developers /opt/company-app
```

Use recursive ownership changes carefully on production systems.

## 10. `chgrp`

```bash
sudo chgrp developers app.py
sudo chgrp -R developers /opt/company-app/app
```

Remember:

```text
chmod → permissions
chown → owner
chgrp → group
```

## 11. Group Commands

```bash
groups
groups developer1
id developer1
getent group developers
```

Add a user to a supplementary group:

```bash
sudo usermod -aG developers developer1
```

Multiple groups:

```bash
sudo usermod -aG developers,appusers developer1
```

Important: `-a` means append. Without it, existing supplementary groups can be replaced.

Remove a user:

```bash
sudo gpasswd -d developer1 developers
```

Add a user:

```bash
sudo gpasswd -a developer1 developers
```

After group changes, start a new login session. For a test session:

```bash
newgrp developers
```

## 12. `umask`

Check:

```bash
umask
```

Example:

```text
0022
```

`umask` controls which permissions are removed from newly created files/directories.

Test:

```bash
touch test.txt
mkdir testdir
ls -ld testdir test.txt
```

## 13. `stat`

Detailed file metadata:

```bash
stat app.py
```

Useful for:

- Size
- Owner
- Group
- Permissions
- Timestamps
- Inode

## 14. `namei`

Very useful for permission troubleshooting:

```bash
namei -l /opt/company-app/config/database.conf
```

It shows permissions for each directory in the path.

## 15. Special Permissions

### SUID

Allows an executable to run with the file owner's privileges.

```bash
ls -l /usr/bin/passwd
chmod u+s program
```

Use carefully.

### SGID

On directories, SGID helps newly created files inherit the directory's group.

```bash
sudo chmod g+s /opt/company-app
```

### Sticky Bit

Common on shared directories such as `/tmp`.

```bash
chmod +t shared/
ls -ld shared/
```

## 16. ACL

Normal permissions provide:

```text
Owner
Group
Others
```

ACL can give more specific access.

View:

```bash
getfacl app.py
```

Give a user read permission:

```bash
sudo setfacl -m u:tester1:r app.py
```

Give a group access:

```bash
sudo setfacl -m g:testers:rx app-directory
```

Remove a user ACL:

```bash
sudo setfacl -x u:tester1 app.py
```

## 17. Permission Command Cheat Sheet

| Command | Purpose |
|---|---|
| `ls -l` | View permissions |
| `ls -la` | Include hidden files |
| `chmod` | Change permissions |
| `chown` | Change owner |
| `chgrp` | Change group |
| `groups` | Show group membership |
| `id` | Show UID/GID/groups |
| `getent group` | Query group information |
| `usermod -aG` | Add user to group |
| `gpasswd` | Manage group membership |
| `umask` | Default permission mask |
| `stat` | Detailed metadata |
| `namei -l` | Inspect path permissions |
| `getfacl` | View ACL |
| `setfacl` | Modify ACL |

# 18. PART 1 Practical — Company Application Server

## Scenario

You are a junior Linux/DevOps engineer.

Create:

```text
/opt/company-app/
├── app/
├── config/
├── logs/
├── backup/
└── scripts/
```

Users:

```text
developer1
developer2
tester1
ops1
```

Groups:

```text
developers
testers
operations
```

Access policy:

```text
developers:
    app      → read/write
    logs     → read/write
    config   → read

testers:
    app      → read
    logs     → read

operations:
    everything → full access

others:
    no access
```

### Step 1 — Create groups

```bash
sudo groupadd developers
sudo groupadd testers
sudo groupadd operations
```

Verify:

```bash
getent group developers
getent group testers
getent group operations
```

### Step 2 — Create users

```bash
sudo useradd -m -s /bin/bash developer1
sudo useradd -m -s /bin/bash developer2
sudo useradd -m -s /bin/bash tester1
sudo useradd -m -s /bin/bash ops1
```

Set passwords:

```bash
sudo passwd developer1
sudo passwd developer2
sudo passwd tester1
sudo passwd ops1
```

### Step 3 — Add users to groups

```bash
sudo usermod -aG developers developer1
sudo usermod -aG developers developer2
sudo usermod -aG testers tester1
sudo usermod -aG operations ops1
```

Verify:

```bash
id developer1
id developer2
id tester1
id ops1
```

### Step 4 — Create project structure

```bash
sudo mkdir -p /opt/company-app/{app,config,logs,backup,scripts}
```

Create files:

```bash
sudo touch /opt/company-app/app/app.py
sudo touch /opt/company-app/config/app.conf
sudo touch /opt/company-app/config/database.conf
sudo touch /opt/company-app/logs/app.log
sudo touch /opt/company-app/scripts/deploy.sh
sudo touch /opt/company-app/scripts/backup.sh
```

### Step 5 — Initial ownership

```bash
sudo chown -R ops1:operations /opt/company-app
```

### Step 6 — Shared group ownership

```bash
sudo chgrp -R developers /opt/company-app/app
sudo chgrp -R developers /opt/company-app/logs
sudo chgrp -R developers /opt/company-app/config
```

### Step 7 — Directory permissions

```bash
sudo chmod 2770 /opt/company-app/app
sudo chmod 2770 /opt/company-app/logs
sudo chmod 2750 /opt/company-app/config
sudo chmod 2770 /opt/company-app/backup
sudo chmod 2770 /opt/company-app/scripts
```

The leading `2` enables SGID.

### Step 8 — File permissions

Normal application/config/log files:

```bash
sudo chmod 660 /opt/company-app/app/app.py
sudo chmod 640 /opt/company-app/config/app.conf
sudo chmod 640 /opt/company-app/config/database.conf
sudo chmod 660 /opt/company-app/logs/app.log
```

Executable scripts:

```bash
sudo chmod 770 /opt/company-app/scripts/deploy.sh
sudo chmod 770 /opt/company-app/scripts/backup.sh
```

> Do not blindly use `chmod -R 777`. Production permissions should follow least privilege.

### Step 9 — Verify

```bash
ls -l /opt/company-app/app
ls -l /opt/company-app/config
ls -l /opt/company-app/logs
ls -l /opt/company-app/scripts
namei -l /opt/company-app/config/database.conf
```

### Step 10 — Test as users

```bash
su - developer1
cat /opt/company-app/config/app.conf
echo "test" >> /opt/company-app/app/app.py
exit
```

Then:

```bash
su - tester1
cat /opt/company-app/app/app.py
exit
```

Verify that the actual results match the access policy.

---

# PART 2 — Linux Network Management

## 19. What is Network Management?

Network management in Linux means configuring, monitoring, testing, and troubleshooting connectivity.

It includes:

```text
Network Interface
IP Address
Subnet
Gateway
Routing
DNS
Ports
Connections
Firewall
SSH
Network Services
```

## 20. Why Do We Need Network Management?

A real application is rarely isolated:

```text
User
 ↓
Internet
 ↓
Load Balancer
 ↓
Web Server
 ↓
Application Server
 ↓
Database Server
```

If networking fails:

```text
Website unavailable
API unavailable
Database unreachable
SSH unavailable
Docker service unreachable
Monitoring unavailable
```

Therefore networking is a core Linux/DevOps skill.

## 21. Real Industry Example

```text
                Internet
                    |
                    ↓
              Load Balancer
                    |
          -------------------
          |                 |
          ↓                 ↓
       Web-01            Web-02
          |
          ↓
       App Server
          |
      ------------
      |          |
      ↓          ↓
   Redis      PostgreSQL
```

Typical questions:

```text
Can Web-01 reach App Server?
Is PostgreSQL listening on 5432?
Can DNS resolve the hostname?
Is the route correct?
Is a firewall blocking traffic?
```

## 22. Network Interface Commands

```bash
ip link
ip -br link
```

Common interfaces:

```text
lo
eth0
ens33
enp0s3
```

## 23. IP Address Commands

```bash
ip addr
ip -br addr
hostname -I
```

## 24. Routing

```bash
ip route
```

Example:

```text
default via 192.168.1.1 dev eth0
192.168.1.0/24 dev eth0
```

The default route tells Linux where to send traffic for other networks.

## 25. Connectivity Testing

```bash
ping 8.8.8.8
ping google.com
ping -c 4 8.8.8.8
```

Important: ping failure does not always mean the application is unavailable because ICMP may be blocked.

## 26. DNS

```bash
cat /etc/resolv.conf
getent hosts google.com
nslookup google.com
dig google.com
dig google.com A
```

Concept:

```text
Domain name → DNS → IP address
```

## 27. Ports and Sockets

```bash
ss -tuln
sudo ss -tulpn
ss -tan
```

Options:

```text
t → TCP
u → UDP
l → listening
n → numeric output
p → process
a → all
```

Common ports:

| Port | Service |
|---:|---|
| 22 | SSH |
| 53 | DNS |
| 80 | HTTP |
| 443 | HTTPS |
| 3306 | MySQL |
| 5432 | PostgreSQL |
| 6379 | Redis |
| 8080 | Common application port |

## 28. `curl`

```bash
curl http://example.com
curl -I https://example.com
curl http://localhost:8080
curl http://localhost:8080/api/users
```

`curl` is extremely useful for API and application troubleshooting.

## 29. `wget`

```bash
wget https://example.com/file.zip
wget -O application.zip https://example.com/file.zip
```

## 30. `traceroute` / `tracepath`

```bash
traceroute google.com
tracepath google.com
```

Useful for understanding network hops.

## 31. `nc` — Netcat

Test a TCP port:

```bash
nc -zv 192.168.1.20 8080
nc -zv database-server 5432
```

## 32. Hostname

```bash
hostname
hostnamectl
```

Change hostname:

```bash
sudo hostnamectl set-hostname app-server-01
```

## 33. `/etc/hosts`

View:

```bash
cat /etc/hosts
```

Example:

```text
10.0.1.20 app-server
10.0.1.30 db-server
```

Then:

```bash
ping app-server
```

## 34. NetworkManager / `nmcli`

```bash
nmcli device status
nmcli connection show
nmcli general status
```

Detailed connection:

```bash
nmcli connection show "connection-name"
```

## 35. Firewall — UFW

```bash
sudo ufw status
sudo ufw status verbose
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw allow 8080/tcp
sudo ufw delete allow 8080/tcp
sudo ufw enable
```

> On a remote server, ensure SSH access is allowed before enabling a firewall.

## 36. SSH

Connect:

```bash
ssh username@server-ip
```

Example:

```bash
ssh ubuntu@10.0.1.20
```

Custom port:

```bash
ssh -p 2222 ubuntu@10.0.1.20
```

Verbose troubleshooting:

```bash
ssh -v ubuntu@10.0.1.20
```

## 37. SCP

Local → server:

```bash
scp app.conf ubuntu@10.0.1.20:/home/ubuntu/
```

Server → local:

```bash
scp ubuntu@10.0.1.20:/home/ubuntu/app.log .
```

Directory:

```bash
scp -r project/ ubuntu@10.0.1.20:/home/ubuntu/
```

## 38. Network Command Cheat Sheet

| Command | Purpose |
|---|---|
| `ip link` | Network interfaces |
| `ip addr` | IP addresses |
| `ip route` | Routing table |
| `hostname -I` | Local IP |
| `ping` | Connectivity |
| `getent hosts` | Name resolution |
| `nslookup` | DNS lookup |
| `dig` | Detailed DNS |
| `ss` | Ports/connections |
| `curl` | HTTP/API testing |
| `wget` | Download |
| `traceroute` | Network path |
| `tracepath` | Network path |
| `nc` | Port testing |
| `hostnamectl` | Hostname/system information |
| `nmcli` | NetworkManager |
| `ufw` | Firewall |
| `ssh` | Remote login |
| `scp` | Secure file copy |

# 39. PART 2 Practical — Application Server Troubleshooting

## Scenario

Application server:

```text
app-server-01
10.0.1.20:8080
```

Database:

```text
10.0.2.30:5432
```

Redis:

```text
10.0.2.40:6379
```

Your job is to verify connectivity.

### Step 1 — Identify server

```bash
hostname
hostnamectl
```

### Step 2 — Check interfaces and IP

```bash
ip -br link
ip -br addr
hostname -I
```

Record:

```text
Interface:
IP:
```

### Step 3 — Check routing

```bash
ip route
```

### Step 4 — Test Internet

```bash
ping -c 4 8.8.8.8
ping -c 4 google.com
```

If IP works but hostname fails, investigate DNS.

### Step 5 — Test DNS

```bash
getent hosts google.com
nslookup google.com
dig google.com
```

### Step 6 — Check application port

```bash
sudo ss -tulpn
```

Look for:

```text
:8080
```

If nothing listens on 8080, investigate the application.

### Step 7 — Test locally

```bash
curl http://localhost:8080
curl http://localhost:8080/health
```

### Step 8 — Test using server IP

```bash
curl http://10.0.1.20:8080
```

If localhost works but server IP does not, investigate:

- Application bind address
- Linux firewall
- Network configuration

### Step 9 — Test PostgreSQL

```bash
nc -zv 10.0.2.30 5432
```

### Step 10 — Test Redis

```bash
nc -zv 10.0.2.40 6379
```

### Step 11 — Check firewall

```bash
sudo ufw status verbose
```

Open a port only when required:

```bash
sudo ufw allow 8080/tcp
```

---

# 40. Industry Troubleshooting Decision Tree

When an application is not reachable:

```text
Application unavailable
        ↓
Is the process running?
        ↓
Is the port listening?
        ↓
ss -tulpn
        ↓
Can localhost access it?
        ↓
curl localhost:8080
        ↓
Can server IP access it?
        ↓
curl SERVER_IP:8080
        ↓
Check route
        ↓
ip route
        ↓
Check DNS
        ↓
dig / nslookup
        ↓
Check Linux firewall
        ↓
ufw
        ↓
Check cloud firewall / Security Group
```

# 41. Combined Permission + Network Real-World Project

Architecture:

```text
                    Internet
                        |
                        ↓
                  Load Balancer
                        |
                        ↓
                  app-server-01
                   10.0.1.20
                        |
          -------------------------
          |                       |
          ↓                       ↓
     PostgreSQL                 Redis
     10.0.2.30                 10.0.2.40
```

Linux users:

```text
developer1
developer2
tester1
ops1
```

Groups:

```text
developers
testers
operations
```

Application:

```text
/opt/company-app/
├── app/
├── config/
├── logs/
├── backup/
└── scripts/
```

Students must:

1. Create users.
2. Create groups.
3. Add users to groups.
4. Create the project structure.
5. Configure ownership.
6. Configure permissions.
7. Verify permissions.
8. Check IP address.
9. Check interfaces.
10. Check routing.
11. Check DNS.
12. Check listening ports.
13. Test the application with `curl`.
14. Test PostgreSQL connectivity with `nc`.
15. Test Redis connectivity.
16. Check firewall.
17. Troubleshoot a deliberately broken permission.
18. Troubleshoot a deliberately broken network/port configuration.

# 42. Production Rules

Never blindly use:

```bash
chmod 777
```

Prefer the minimum required permission.

Do not blindly use:

```bash
chown -R
```

on production paths.

Do not expose unnecessary ports.

Example:

```text
22   → SSH
80   → HTTP
443  → HTTPS
8080 → only if required
5432 → only where required
6379 → only where required
```

# 43. Final Mental Model

```text
                 LINUX SERVER
                      |
       -------------------------------
       |                             |
       ↓                             ↓
  ACCESS CONTROL                NETWORKING
       |                             |
       ↓                             ↓
    Users                         Interface
       ↓                             ↓
    Groups                           IP
       ↓                             ↓
  Ownership                        Route
       ↓                             ↓
 Permissions                      Gateway
       ↓                             ↓
    ACL                              DNS
       |                             |
       ↓                             ↓
 Who can access?              Where can it connect?
       |                             |
       ----------- APPLICATION -------
                      |
                      ↓
                    PORT
```

DevOps troubleshooting model:

```text
WHO?
 ↓
User / Group

WHAT?
 ↓
Permission / Ownership

WHERE?
 ↓
File / Directory

IS IT RUNNING?
 ↓
Process / Service

WHICH PORT?
 ↓
ss

WHICH IP?
 ↓
ip addr

WHICH ROUTE?
 ↓
ip route

CAN I REACH IT?
 ↓
ping / nc / curl

CAN I RESOLVE IT?
 ↓
DNS / dig

IS IT BLOCKED?
 ↓
Firewall / Security Group
```

This foundation prepares students for:

```text
Linux Process Management
        ↓
Services
        ↓
Package Management
        ↓
Logs & Troubleshooting
        ↓
Shell Scripting
        ↓
SSH
        ↓
AWS EC2
        ↓
Security Groups
        ↓
VPC
        ↓
Docker Networking
        ↓
Kubernetes Networking
```
