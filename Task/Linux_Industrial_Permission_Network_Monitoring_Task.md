# 🛡️ Linux Infrastructure Operations Task
## Real-Time Industrial Scenario: Permission Management + Network Management + Monitoring

> **Role:** Junior Linux / Cloud / DevOps Engineer  
> **Environment:** Ubuntu Server / RHEL-compatible Linux  
> **Difficulty:** Intermediate → Professional  
> **Mode:** Hands-on Industrial Simulation  
> **Goal:** Secure, configure, troubleshoot, and monitor a production-like Linux application server.

---

## 🏢 1. Industrial Scenario

You have joined **TechNova Solutions** as a Junior Linux/DevOps Engineer.

The company runs an internal **Employee Management Application** on a Linux server.

The server contains:

```text
Application
├── /opt/technova/app
├── /opt/technova/logs
├── /opt/technova/backups
└── /opt/technova/scripts
```

The server is accessed by different teams:

| Team | Users | Responsibility |
|---|---|---|
| Developers | `dev01`, `dev02` | Application code |
| Operations | `ops01`, `ops02` | Logs and operations |
| Database | `db01` | Database-related files |
| Security | `sec01` | Security auditing |
| Management | `manager01` | Read-only reporting |

### 🚨 Current Problems

The previous administrator configured the server poorly:

- Application files are readable/writable by the wrong users.
- Developers can accidentally modify production logs.
- Operations cannot access some application logs.
- Backup files are accessible to normal users.
- Network configuration has not been documented.
- The server's listening ports are unknown.
- There may be unnecessary services running.
- Network connectivity must be tested.
- System/network activity must be monitored.
- The team needs evidence that the server is secure and healthy.

Your task is to fix the environment as a junior infrastructure engineer.

---

# 🎯 2. Mission

Complete the following three phases:

```text
PHASE 1
Permission Management
        ↓
PHASE 2
Network Management
        ↓
PHASE 3
Network + System Monitoring
        ↓
Final Infrastructure Audit
```

You must use Linux commands, not only GUI tools.

---

# 👥 3. Users and Groups

Create these groups:

```text
developers
operations
database
security
management
```

Create these users:

```text
dev01
dev02
ops01
ops02
db01
sec01
manager01
```

Assign users to appropriate groups.

### Expected Membership

```text
developers
├── dev01
└── dev02

operations
├── ops01
└── ops02

database
└── db01

security
└── sec01

management
└── manager01
```

### Your Tasks

1. Create all groups.
2. Create all users.
3. Assign users to groups.
4. Verify group membership.
5. Set appropriate login shells.
6. Verify user IDs and group IDs.

### Evidence Required

Run commands that prove:

```text
User exists
User belongs to correct group
Group exists
UID/GID are correct
```

---

# 🔐 PHASE 1 — Permission Management

## 4. Create the Application Structure

Create:

```text
/opt/technova/
├── app/
├── logs/
├── backups/
└── scripts/
```

Create sample files:

```text
app/
├── app.py
├── config.conf
└── README.md

logs/
├── application.log
└── error.log

backups/
└── database.sql

scripts/
├── deploy.sh
└── backup.sh
```

Add realistic sample content to each file.

---

# 5. Define the Security Policy

The company wants the following access model:

### Application

```text
Developers:
    Read + Write

Operations:
    Read

Security:
    Read

Management:
    Read

Database:
    No access
```

### Logs

```text
Developers:
    Read only

Operations:
    Read + Write

Security:
    Read

Management:
    Read

Database:
    No access
```

### Backups

```text
Database:
    Read + Write

Operations:
    Read

Security:
    Read

Developers:
    No access

Management:
    No access
```

### Scripts

```text
Developers:
    Read

Operations:
    Read + Execute

Security:
    Read

Management:
    No access
```

---

# 6. Implement Linux Permissions

Use appropriate:

```text
chown
chgrp
chmod
umask
```

You may use:

```text
numeric permissions
```

and/or:

```text
symbolic permissions
```

Where normal UNIX permissions are insufficient, investigate and implement:

```text
ACLs
```

using:

```text
setfacl
getfacl
```

---

# 🧪 7. Permission Testing

You are not finished after running `chmod`.

You must test the policy.

For each user:

```text
dev01
dev02
ops01
ops02
db01
sec01
manager01
```

attempt appropriate:

```text
read
write
execute
create
delete
```

operations.

### Example Test Questions

Can `dev01`:

- Modify `app.py`?
- Modify `application.log`?
- Read `application.log`?
- Read `database.sql`?
- Execute `deploy.sh`?

Can `ops01`:

- Read logs?
- Modify logs?
- Read application files?
- Modify application source code?
- Read backups?

Can `manager01`:

- Read reports?
- Modify application files?
- Modify logs?
- Access backups?

Document every result.

---

# 📋 8. Permission Audit Table

Create a table similar to:

| User | Resource | Read | Write | Execute | Expected |
|---|---|---:|---:|---:|---|
| dev01 | app | ✅ | ✅ | ❌ | Pass |
| dev01 | logs | ✅ | ❌ | ❌ | Pass |
| dev01 | backups | ❌ | ❌ | ❌ | Pass |
| ops01 | logs | ✅ | ✅ | ❌ | Pass |
| db01 | backups | ✅ | ✅ | ❌ | Pass |
| manager01 | logs | ✅ | ❌ | ❌ | Pass |

Add your actual results.

---

# 🌐 PHASE 2 — Network Management

## 9. Network Discovery

Before changing anything, investigate the server.

Find:

- Hostname
- IP address
- Network interface
- MAC address
- Default gateway
- Routing table
- DNS configuration
- Active interfaces
- Listening ports
- Active network connections

Useful commands to investigate:

```bash
hostname
hostnamectl
ip addr
ip link
ip route
ip neigh
ss
resolvectl
ping
```

Do not blindly copy commands. Understand what each command tells you.

---

# 10. Network Documentation

Create a network inventory:

```text
Hostname:
Primary Interface:
IPv4 Address:
IPv6 Address:
MAC Address:
Default Gateway:
DNS Server:
Network:
Listening Ports:
```

Example format:

```text
Server: app-server-01
Interface: eth0
IPv4: 192.168.10.20/24
Gateway: 192.168.10.1
DNS: 192.168.10.10
```

> Use values from your lab environment. Do not blindly use the example IPs.

---

# 11. Connectivity Testing

Test connectivity at multiple levels.

### Level 1 — Local Interface

Verify the interface is:

```text
UP
```

### Level 2 — Gateway

Test the default gateway.

### Level 3 — Internet / External Network

Test a known external destination if your lab permits it.

### Level 4 — DNS

Verify hostname resolution.

Test:

```text
domain → IP
IP → hostname
```

### Level 5 — Application Port

Verify whether the application port is reachable/listening.

Document:

```text
Test
Command
Expected Result
Actual Result
Status
```

---

# 🔎 12. Port and Service Investigation

Identify:

- Listening TCP ports
- Listening UDP ports
- Process owning each port
- Service associated with each port

Investigate using:

```bash
ss
lsof
systemctl
ps
```

Create a table:

| Port | Protocol | Process | Service | Required? |
|---:|---|---|---|---|
| 22 | TCP | sshd | SSH | Yes |
| XXXX | TCP | application | App | Yes |
| XXXX | TCP | unknown | Unknown | Investigate |

> Do not assume a port is safe simply because it is open.

---

# 🧯 13. Network Troubleshooting Incident

## Scenario

Users report:

> "The application server is online, but the application cannot be reached."

You must investigate.

Check:

```text
1. Is the network interface UP?
2. Does the server have an IP?
3. Is the route correct?
4. Can the gateway be reached?
5. Can DNS resolve?
6. Is the application process running?
7. Is the application port listening?
8. Is the firewall blocking traffic?
9. Are there failed connections?
10. Are there relevant system logs?
```

Do not restart random services.

Follow a structured troubleshooting process.

---

# 🔥 14. Firewall Investigation

Determine whether a firewall is enabled.

Depending on your Linux distribution, investigate tools such as:

```text
ufw
firewalld
nftables
iptables
```

Your objective:

```text
Allow only required traffic.
```

At minimum, determine:

```text
SSH
Application
DNS
Other required services
```

Do not disable the firewall simply to make the application work.

---

# 📊 PHASE 3 — Monitoring

## 15. System Monitoring

Monitor:

```text
CPU
Memory
Disk
Load Average
Processes
Network
Open Connections
```

Investigate commands such as:

```bash
top
htop
free
uptime
ps
df
du
vmstat
```

Record:

```text
CPU Usage:
Memory Usage:
Disk Usage:
Load Average:
Top Processes:
```

---

# 🌐 16. Network Monitoring

Monitor:

```text
Incoming connections
Outgoing connections
Packets
Errors
Dropped packets
Network interface statistics
Listening ports
```

Investigate:

```bash
ip -s link
ss -s
ss -tulnp
```

If available in your lab, investigate tools such as:

```text
iftop
nload
tcpdump
```

### Important

Use packet capture responsibly.

Capture only traffic required for your lab investigation.

Do not capture credentials or private production traffic.

---

# 📜 17. Log Monitoring

Inspect relevant system logs.

Depending on the distribution, investigate:

```text
journalctl
/var/log/
```

Look for:

```text
authentication failures
service failures
network errors
permission errors
application errors
```

Find recent errors and categorize them:

```text
Authentication
Network
Permission
Application
System
```

---

# 🚨 18. Monitoring Incident

## Scenario

At 10:30 AM the monitoring team reports:

> "The application server is responding slowly."

Your job is to identify the cause.

Investigate:

```text
CPU
Memory
Disk
Load
Processes
Network
Application logs
System logs
```

Possible causes:

```text
CPU-heavy process
Memory exhaustion
Disk nearly full
Network issue
Application failure
Too many connections
```

### Incident Report

Document:

```text
Incident:
Start Time:
Symptoms:
Commands Used:
Evidence:
Root Cause:
Fix:
Validation:
Preventive Action:
```

---

# 🧩 19. Real-World Failure Injection

Your instructor can intentionally introduce one or more problems:

### Failure A

Wrong permissions on:

```text
/opt/technova/logs
```

### Failure B

A user is added to the wrong group.

### Failure C

A required service is stopped.

### Failure D

An application port is no longer listening.

### Failure E

A firewall rule blocks the application.

### Failure F

A disk is filled with dummy log data.

### Failure G

A script loses execute permission.

### Failure H

A network interface is brought down in a safe lab environment.

Your job is to:

```text
Detect → Investigate → Fix → Verify → Document
```

---

# 📝 20. Final Infrastructure Audit

Create a final audit report containing:

## User & Group Audit

```text
[ ] All required users created
[ ] All required groups created
[ ] Correct group memberships
[ ] UID/GID verified
```

## Permission Audit

```text
[ ] Application permissions correct
[ ] Log permissions correct
[ ] Backup permissions correct
[ ] Script permissions correct
[ ] ACLs reviewed
[ ] Unauthorized access tested
```

## Network Audit

```text
[ ] Hostname verified
[ ] Interfaces verified
[ ] IP address documented
[ ] Gateway verified
[ ] DNS verified
[ ] Routes verified
[ ] Listening ports reviewed
[ ] Firewall reviewed
```

## Monitoring Audit

```text
[ ] CPU checked
[ ] Memory checked
[ ] Disk checked
[ ] Processes checked
[ ] Network statistics checked
[ ] Active connections checked
[ ] Logs reviewed
```

---

# 🏆 21. Final Deliverables

Submit a Git repository or folder with:

```text
linux-infrastructure-task/
│
├── README.md
│
├── 01-users-groups.md
├── 02-permission-policy.md
├── 03-permission-testing.md
├── 04-network-inventory.md
├── 05-network-troubleshooting.md
├── 06-firewall-audit.md
├── 07-system-monitoring.md
├── 08-network-monitoring.md
├── 09-incident-report.md
│
├── evidence/
│   ├── users.txt
│   ├── groups.txt
│   ├── permissions.txt
│   ├── acl.txt
│   ├── network.txt
│   ├── ports.txt
│   └── monitoring.txt
│
└── screenshots/
    ├── permissions/
    ├── network/
    └── monitoring/
```

---

# 💼 22. Interview Questions You Must Be Able to Answer

### Permissions

1. What is the difference between `chmod`, `chown`, and `chgrp`?
2. What is an ACL?
3. When would you use ACLs?
4. What is the difference between `755`, `644`, and `700`?
5. Why should production application files not be writable by everyone?
6. What is the purpose of `umask`?
7. What is the difference between user, group, and other permissions?

### Network

8. How do you find a Linux server's IP address?
9. How do you find the default gateway?
10. How do you check listening ports?
11. What is the difference between `ss` and `ping`?
12. How do you troubleshoot DNS?
13. How do you identify which process owns a port?
14. How would you investigate an unreachable application?

### Monitoring

15. How do you check memory usage?
16. How do you check disk usage?
17. How do you identify a CPU-heavy process?
18. How do you monitor network connections?
19. Where would you look for Linux system logs?
20. What steps would you follow when a production server becomes slow?

---

# ⭐ 23. Evaluation Criteria

| Area | Weight |
|---|---:|
| User & Group Management | 15% |
| Permission Configuration | 20% |
| Permission Testing | 10% |
| Network Discovery | 10% |
| Network Troubleshooting | 15% |
| Firewall Understanding | 10% |
| System Monitoring | 10% |
| Documentation & Evidence | 10% |
| **Total** | **100%** |

### Bonus

Add:

```text
+5  Automated audit script
+5  Bash health-check script
+5  Permission verification script
+5  Network monitoring script
```

---

# 🚀 Final Challenge

Imagine you are on-call at **2:00 AM**.

A production application is down.

You receive only this message:

> **"Users cannot access the application server."**

You have no GUI dashboard.

You only have SSH access.

Your job is to determine:

```text
Is it a permission problem?
        ↓
Is it a process problem?
        ↓
Is it a network problem?
        ↓
Is it a firewall problem?
        ↓
Is it a resource problem?
        ↓
Is it an application problem?
```

You must not guess.

Use:

```text
Evidence → Diagnosis → Fix → Verification → Documentation
```

### 🎯 Success Condition

The task is complete only when you can prove that:

> **The right users have the right permissions, the server has a correctly functioning network, unnecessary access is restricted, services are reachable as expected, and system/network health can be monitored and explained.**

---

## 🧠 Golden Rule for the Lab

> **Never fix what you have not investigated.**
>
> **Never change permissions without testing them.**
>
> **Never declare a network problem without checking the network path.**
>
> **Never declare a server healthy without evidence.**

**Think like an administrator. Troubleshoot like an engineer. Document like a professional.**
