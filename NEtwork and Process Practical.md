# Linux Practical Lab: Network & Process Management

Practical 01

Beginner to Advanced

# Linux Network & Process Troubleshooting

[Role of chmod, chown, chgrp and umask — Understanding File Permissions in Linux | by Raghavendra V. Konkathi | Medium](https://images.openai.com/static-rsc-4/T1pVQN2qF81bNq4jxU6ml2t-raLUbAsnTMhwqpOCFlbOlCgXLrW4GWHyRsGw7S0p8VtYIlOs6YDtv6cEfI3NLsfydYQ9apmNrg_kQ6-qIffnTcSmAOfAIzlTG5hdyjxgMfBzc5iVC6CoJ8MOSACWOqw8DWCN3SYBhs_pT6JU8Gg?purpose=inline)

[medium.com](https://medium.com/%40raghavendra.v.konkathi/role-of-chmod-chown-chgrp-and-umask-understanding-file-permissions-in-linux-875b09b59b9a?utm_source=chatgpt.com)

OPERATING SYSTEM

Ubuntu Linux

LAB DURATION

2–3 Hours

DIFFICULTY

Beginner → Advanced

DOMAIN

Linux / DevOps / SRE

Topics covered

- Network interfaces and IP configuration
- IP addresses, routing and DNS
- Ports and listening services
- Network connectivity troubleshooting
- Linux processes and process states
- CPU and memory monitoring
- Process priorities and signals
- Service management and log investigation
- Real-world troubleshooting

## 1. Real-world scenario

Imagine that you are working as a Linux System Administrator in a company.

Your company hosts an e-commerce application on an Ubuntu Linux server.

One Monday morning, users start reporting:

1. The website takes 20–30 seconds to load.
2. Some users cannot access the application.
3. The server CPU utilization is unusually high.
4. The application sometimes stops responding.
5. Port 8080 is not responding consistently.
6. You suspect that an application process is consuming excessive resources.

Your manager assigns you this task:

"Investigate the Linux server, identify the network and process-related problems, fix them, and submit a troubleshooting report."

Your job is to investigate rather than immediately restart the server.

## 2. Understand the troubleshooting workflow

This is the workflow you will follow throughout the practical.

Step 1 — Identify the server

OS, hostname, IP address and network interfaces

Step 2 — Troubleshoot networking

DNS, routing, connectivity and ports

Step 3 — Investigate processes

CPU, memory, PID and process states

Step 4 — Fix the problem

Manage processes, signals and services

Step 5 — Verify and document

Retest connectivity, ports, application and logs

## 3. Lab environment setup

Before starting, understand what environment we are using.

For this practical, we'll use Ubuntu 22.04/24.04 or another Ubuntu-based Linux distribution.

### Step 1: Open your Linux terminal

If you're using Ubuntu:

- Press `Ctrl + Alt + T`.
- Your terminal will open.
- You can execute all the commands below.

If you're using Windows, you can use Ubuntu through WSL2, a Linux virtual machine, or an AWS EC2 Ubuntu instance.

### Step 2: Update package information

```
sudo apt update
```

What does it do?

- `sudo`: Executes the command with administrative privileges.
- `apt`: Ubuntu's package management tool.
- `update`: Downloads the latest package information from configured repositories.

What happens when you run it?

Your system checks available software repositories and refreshes its package index. It does not upgrade installed software.

### Step 3: Install the required troubleshooting tools

```
sudo apt install -y curl iproute2 iputils-ping dnsutils traceroute net-tools procps psmisc lsof tcpdump tree
```

| Package      | Purpose                       |
| ------------ | ----------------------------- |
| curl         | Test HTTP/HTTPS requests      |
| iproute2     | IP addresses and routing      |
| iputils-ping | Test network connectivity     |
| dnsutils     | DNS troubleshooting           |
| traceroute   | Trace network paths           |
| net-tools    | Legacy network commands       |
| procps       | Process monitoring            |
| psmisc       | Process utilities             |
| lsof         | Identify open files and ports |
| tcpdump      | Capture network packets       |
| tree         | Display directory structure   |

`-y` automatically confirms package installation.

Verify installations:

```
curl --version
ip -V
ping -V
ss --version
ps --version
tcpdump --version
```

### Step 4: Identify your Linux system

Execute these commands one by one:

Command 1:

```
hostname
```

Why do we use it?

It displays the hostname of your Linux server.

Example output:

```
ubuntu-server
```

Command 2:

```
hostnamectl
```

Why?

It provides additional system identification details, such as:

- Static hostname
- Operating system
- Kernel version
- Architecture
- Virtualization information (when available)

Command 3:

```
cat /etc/os-release
```

Why?

It displays Linux distribution information.

Example:

```
PRETTY_NAME="Ubuntu 24.04 LTS"
VERSION_ID="24.04"
ID=ubuntu
```

Command 4:

```
uname -a
```

Why?

It displays kernel-related information.

Remember:

- Linux distribution = Ubuntu.
- Kernel = Core component responsible for managing hardware and system resources.

Command 5:

```
uptime
```

Why?

It displays:

- Current system time
- How long the server has been running
- Number of logged-in users
- Load averages

Example:

```
19:20:10 up 5 days, 2 users, load average: 0.45, 0.60, 0.52
```

Load average represents system load over 1, 5 and 15 minutes. It is not directly equivalent to CPU utilization.

# Part A — Network Management

## 4. Network interface investigation

Scenario: Users report that they cannot access your e-commerce server. First, you need to check whether the Linux server has a working network interface and IP address.

### Practical 1: Identify network interfaces

Command 1:

```
ip link show
```

What does `ip` mean?

It is the modern Linux utility for network configuration and inspection.

What does `link` mean?

It deals with network interfaces such as Ethernet and Wi-Fi.

What does `show` mean?

It displays the current interface information.

Example output:

```
1: lo: <LOOPBACK,UP,LOWER_UP>
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP>
```

Understand the output:

| Interface | Meaning                                 |
| --------- | --------------------------------------- |
| lo        | Loopback interface                      |
| ens33     | Example Ethernet interface              |
| UP        | Interface administratively enabled      |
| LOWER_UP  | Physical or virtual link is operational |

Important: `lo` is the loopback interface. It allows a machine to communicate with itself.

Command 2:

```
ip addr show
```

Purpose: Display IP addresses assigned to network interfaces.

Example:

```
2: ens33:
    inet 192.168.1.105/24
```

Explanation:

- `192.168.1.105` = Server IP address.
- `/24` = Network prefix length.
- `/24` is equivalent to subnet mask `255.255.255.0`.

Command 3:

```
ip -br addr
```

What is `-br`?

It means brief output.

Instead of displaying lengthy information, it displays interfaces in a compact format.

Example:

```
lo       UNKNOWN  127.0.0.1/8
ens33    UP       192.168.1.105/24
```

Command 4:

```
ip addr show ens33
```

Purpose: Display IP information for a particular interface.

Replace `ens33` with the actual interface name displayed on your server.

Command 5:

```
ip link show ens33
```

Purpose: Check the operational and administrative state of a specific network interface.

### Practical task

Try answering these questions:

1. What is the name of your primary network interface?
2. What is your server's IPv4 address?
3. Is your network interface UP or DOWN?
4. What is the purpose of the loopback interface?

## 5. IP address and routing investigation

Scenario: Your server has an IP address, but it cannot communicate with external networks.

You need to check the routing table.

### Command 1: Display routing information

```
ip route show
```

Why do we use it?

A routing table tells Linux where to send network traffic.

Example:

```
default via 192.168.1.1 dev ens33
192.168.1.0/24 dev ens33 proto kernel scope link
```

Understand the output:

| Output          | Explanation       |
| --------------- | ----------------- |
| default         | Default route     |
| via 192.168.1.1 | Gateway address   |
| dev ens33       | Network interface |
| 192.168.1.0/24  | Local network     |

What is a gateway?

A gateway is the network device that forwards traffic from your local network to other networks.

For example:

```
Your Ubuntu Server
       |
       |
192.168.1.105
       |
       |
Router / Gateway
192.168.1.1
       |
       |
Internet
```

### Command 2: Check your default gateway

```
ip route | grep default
```

Let's break it down:

- `ip route`: Displays routing information.
- `|`: Pipe operator. Passes output to another command.
- `grep`: Searches for matching text.
- `default`: Text we want to find.

### Command 3: Check the route to a destination

```
ip route get 8.8.8.8
```

Purpose:

It shows which interface and gateway Linux would use to reach Google's public DNS IP.

This checks the routing decision, not whether the destination is actually reachable.

### Command 4: Check connectivity with the gateway

```
ping -c 4 192.168.1.1
```

Replace the IP address with your actual gateway.

Explanation:

- `ping`: Sends ICMP Echo Requests.
- `-c`: Specifies the number of packets.
- `4`: Sends four requests.

Possible output:

```
64 bytes from 192.168.1.1: icmp_seq=1 ttl=64 time=1.2 ms
```

What does it tell you?

The destination responded to your ICMP request.

If it fails, investigate the gateway, network interface, local firewall and network configuration. A failed ping alone does not prove the gateway is down because ICMP can be filtered.

### Command 5: Test external connectivity

```
ping -c 4 8.8.8.8
```

Why `8.8.8.8`?

It is a public DNS resolver operated by Google.

Interpretation:

- Gateway ping works, public IP ping works: Basic IP connectivity is working.
- Gateway ping works, public IP ping fails: Investigate upstream routing, firewall and internet connectivity.
- Both fail: Investigate local network configuration and gateway connectivity.

## 6. DNS troubleshooting

Scenario: Users can access websites using IP addresses but cannot access them using domain names.

This may indicate a DNS resolution issue.

What is DNS?

DNS stands for Domain Name System.

It translates domain names into IP addresses.

Example:

```
google.com
     |
     | DNS Resolution
     |
142.250.x.x
```

### Command 1: Check DNS configuration

```
cat /etc/resolv.conf
```

Why?

This file contains DNS resolver configuration or a link to resolver-managed configuration.

Example:

```
nameserver 127.0.0.53
```

This address commonly represents Ubuntu's local `systemd-resolved` stub resolver.

### Command 2: Check DNS resolution

```
nslookup google.com
```

What happens?

Linux sends a DNS query to a configured DNS resolver and displays the returned records.

### Command 3: Use dig

```
dig google.com
```

`dig` means Domain Information Groper.

It is useful for detailed DNS investigation.

Important sections:

- QUESTION SECTION: The requested domain.
- ANSWER SECTION: The returned DNS records.
- SERVER: The resolver that answered the query.
- Query time: Time taken to receive the response.

### Command 4: Get only the IP address

```
dig +short google.com
```

Purpose:

Displays a concise result instead of the complete DNS response.

### Command 5: Test DNS using another resolver

```
dig @8.8.8.8 google.com
```

Here:

- `@8.8.8.8` specifies the DNS server.
- `google.com` is the domain being queried.

This helps compare your configured resolver against Google's public resolver. Network policy may block direct external DNS queries.

### Command 6: Test DNS through getent

```
getent hosts google.com
```

Why is this useful?

It tests hostname resolution through the system's configured Name Service Switch mechanism, which is closer to how many Linux applications resolve hostnames.

## 7. Check listening ports and network connections

Scenario: Your application is supposed to run on port 8080, but customers cannot access it.

First, check whether anything is listening on port 8080.

### Command 1: Display listening ports

```
sudo ss -tulnp
```

This is one of the most important Linux networking commands for DevOps engineers.

Let's understand every option:

| Option | Meaning                                            |
| ------ | -------------------------------------------------- |
| `-t`   | TCP sockets                                        |
| `-u`   | UDP sockets                                        |
| `-l`   | Listening sockets                                  |
| `-n`   | Numeric addresses and ports                        |
| `-p`   | Show associated process information when permitted |

Example:

```
Netid State  Local Address:Port
tcp   LISTEN 0.0.0.0:22
tcp   LISTEN 0.0.0.0:8080
```

Explanation:

- Port 22: Commonly used by SSH.
- Port 8080: Your application port.
- `0.0.0.0`: Listening on all IPv4 interfaces.

A process listening on `127.0.0.1:8080` accepts connections only through the local loopback interface, not directly through the server's external IPv4 interface.

### Command 2: Check a specific port

```
sudo ss -tulnp | grep 8080
```

Purpose:

Searches the socket information for port 8080.

If nothing is returned, there may be no listener on that port.

### Command 3: Display established connections

```
ss -tn state established
```

Purpose:

Displays established TCP connections.

This is useful when investigating active connections to your server.

### Command 4: Identify which process is using a port

```
sudo lsof -i :8080
```

Why?

`lsof` stands for List Open Files.

On Linux, network sockets are represented as file descriptors, so `lsof` can help identify processes associated with a port.

Example:

```
COMMAND  PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
node    2450 ubuntu 21u  IPv4  ...         TCP *:8080 (LISTEN)
```

Here:

- `node`: Application process.
- `2450`: Process ID (PID).
- `ubuntu`: Process owner.
- `8080`: Application port.

### Command 5: Check using fuser

```
sudo fuser -v 8080/tcp
```

Purpose:

Identifies processes using TCP port 8080.

Important: Don't kill a process just because it is using a port. First establish whether that process belongs to the application.

## 8. Test HTTP application connectivity

Now test the application itself.

### Command 1: Test local application access

```
curl -I http://localhost:8080
```

Explanation:

- `curl`: Transfers data to or from a URL.
- `-I`: Requests HTTP response headers only.
- `localhost`: Refers to your own machine.
- `8080`: Application port.

Possible output:

```
HTTP/1.1 200 OK
Content-Type: application/json
```

What does HTTP 200 mean?

The server successfully processed the request.

Other useful status codes:

| Code | Meaning               |
| ---- | --------------------- |
| 200  | OK                    |
| 301  | Permanent redirect    |
| 400  | Bad request           |
| 401  | Unauthorized          |
| 403  | Forbidden             |
| 404  | Not found             |
| 500  | Internal server error |
| 502  | Bad gateway           |
| 503  | Service unavailable   |

### Command 2: Display response details

```
curl -v http://localhost:8080
```

`-v` enables verbose output.

It shows useful details about:

- Connection establishment
- HTTP request headers
- HTTP response headers
- Connection failures

### Command 3: Test with a timeout

```
curl --connect-timeout 5 http://localhost:8080
```

This allows up to five seconds for the connection to be established.

### Command 4: Test the server through its IP address

```
curl -I http://192.168.1.105:8080
```

Replace the address with your server's actual IP.

This helps determine whether the application can be reached through its network interface.

# Part B — Process Management

## 9. Understand Linux processes

Scenario: Your application is running, but the server has become slow. You suspect that one or more processes are consuming excessive CPU or memory.

Before troubleshooting, understand what a process is.

A process is an instance of a running program.

For example:

- Running `nginx` creates an Nginx process.
- Running `python3 app.py` creates a Python process.
- Running `node server.js` creates a Node.js process.

Every process has a Process ID (PID).

Example:

```
PID     COMMAND
1       systemd
850     sshd
1200    nginx
2450    node
```

### Important process terminology

| Term | Meaning               |
| ---- | --------------------- |
| PID  | Process ID            |
| PPID | Parent Process ID     |
| UID  | User ID               |
| GID  | Group ID              |
| CPU  | Processor utilization |
| RSS  | Resident memory       |
| VSZ  | Virtual memory size   |
| TTY  | Associated terminal   |
| STAT | Process state         |

### Common process states

| State | Meaning               |
| ----- | --------------------- |
| R     | Running or runnable   |
| S     | Interruptible sleep   |
| D     | Uninterruptible sleep |
| T     | Stopped or traced     |
| Z     | Zombie process        |
| I     | Idle kernel thread    |

A zombie process has completed execution, but its parent has not yet collected its exit status.

## 10. List and inspect processes

### Command 1: Display all processes

```
ps aux
```

This is one of the most frequently used process management commands.

Let's break it down:

- `ps`: Process status.
- `a`: Show processes associated with terminals across users.
- `u`: Display user-oriented information.
- `x`: Include processes without controlling terminals.

Example:

```
USER     PID  %CPU %MEM  VSZ    RSS   STAT COMMAND
root       1   0.0  0.2  ...    ...   Ss   systemd
ubuntu  2450  85.0  4.5  ...    ...   R    node server.js
```

Here, the Node.js application is consuming 85% CPU.

### Command 2: Display processes in full format

```
ps -ef
```

Explanation:

- `-e`: Select every process.
- `-f`: Display full-format listing.

Example:

```
UID      PID  PPID  C STIME TTY CMD
root       1     0  0 09:00 ?   /sbin/init
ubuntu  2450  1200 85 09:15 ?   node server.js
```

Notice:

`PPID` identifies the parent process.

### Command 3: Search for a specific process

```
ps aux | grep node
```

Purpose:

Finds processes whose displayed command lines contain `node`.

Note: This may also display the `grep` command itself.

A better alternative:

```
pgrep -a node
```

This displays matching process IDs and command lines.

### Command 4: Display process hierarchy

```
pstree -p
```

Why?

It displays processes in a tree structure and includes PIDs.

Example:

```
systemd(1)
 ├─sshd(850)
 │  └─bash(1200)
 └─node(2450)
```

This helps you understand parent-child relationships.

### Command 5: Display a process by PID

```
ps -p 2450 -f
```

Replace `2450` with the actual PID.

What happens?

Linux displays information about that particular process.

### Command 6: Find the parent process

```
ps -o pid,ppid,cmd -p 2450
```

This displays:

- PID: Process ID.
- PPID: Parent process ID.
- CMD: Executed command.

## 11. Real-time process monitoring using top

Scenario: Your manager wants to know why the server is becoming slow.

You need to monitor CPU and memory usage in real time.

### Command 1: Start top

```
top
```

[How to View Running Processes on Linux](https://images.openai.com/static-rsc-4/itDJfVajUisDOZkR9b7cZ-dPEv0eMZdUqzcdT7REgoc_TpJ1rTvQEBNr3POvj0x5VY--IRktpo98lljxsuWH8V3w9-x7DMVrNJjF9DFG_8mRskIEj8VB4R5NDH5DqNSGI8RLjBLYXH60eCWEZIc46--xxmirZTYCpqXNEHvUpq8?purpose=inline)

[howtoforge.com](https://www.howtoforge.com/how-to-view-running-processes-on-linux/?utm_source=chatgpt.com)

What does it do?

`top` continuously refreshes process and system resource information.

Important information:

- CPU utilization
- Memory utilization
- Running processes
- Process IDs
- CPU consumption
- Memory consumption
- Load averages

Useful keyboard shortcuts inside `top`:

| Key | Action                     |
| --- | -------------------------- |
| `q` | Quit                       |
| `P` | Sort by CPU                |
| `M` | Sort by memory             |
| `1` | Show individual CPU cores  |
| `k` | Send a signal to a process |
| `r` | Change process priority    |

### Command 2: Monitor using htop

First install it:

```
sudo apt install htop -y
```

Run:

```
htop
```

`htop` provides an interactive process-monitoring interface.

It makes it easier to identify processes consuming excessive resources.

### Command 3: Display CPU information

```
lscpu
```

Why?

Displays processor architecture, CPU cores, threads and other CPU details.

### Command 4: Display memory usage

```
free -h
```

Explanation:

- `free`: Displays memory usage.
- `-h`: Human-readable units such as MB and GB.

Example:

```
               total   used   free  shared  buff/cache available
Mem:             15Gi   5Gi    3Gi    300Mi      7Gi       9Gi
Swap:             2Gi   0Gi    2Gi
```

Important:

Don't judge memory pressure only by the `free` column. Linux uses available memory for caching. The `available` column is generally more useful for understanding how much memory can be allocated without swapping.

### Command 5: Display CPU and process statistics

```
vmstat 2 5
```

Explanation:

- `vmstat`: Virtual memory statistics.
- `2`: Refresh every two seconds.
- `5`: Display five reports.

It displays information about:

- Processes
- Memory
- Swap
- I/O
- System activity
- CPU utilization

### Command 6: Check disk I/O statistics

```
iostat
```

If the command is unavailable:

```
sudo apt install sysstat -y
```

Then:

```
iostat -xz 2 5
```

This provides detailed disk utilization and I/O performance information.

## 12. Process management: Start, stop and restart

Now let's manage processes.

For this exercise, we'll create our own test processes. Never experiment with killing important production processes.

### Practical 2: Create a background process

Step 1: Create a directory

```
mkdir -p ~/linux-network-process-lab
```

Explanation:

- `mkdir`: Creates a directory.
- `-p`: Creates missing parent directories without failing if the directory already exists.
- `~`: Your home directory.

Step 2: Enter the directory

```
cd ~/linux-network-process-lab
```

Step 3: Create a test script

```
nano cpu-test.sh
```

Add this content:

```
#!/bin/bash

echo "CPU test process started"

while true
do
    :
done
```

Save the file using:

- `Ctrl + O`
- Enter
- `Ctrl + X`

Understand the script:

| Code          | Meaning                         |
| ------------- | ------------------------------- |
| `#!/bin/bash` | Uses Bash to execute the script |
| `echo`        | Prints a message                |
| `while true`  | Creates an infinite loop        |
| `do`          | Starts the loop body            |
| `:`           | Bash no-operation command       |
| `done`        | Ends the loop                   |

This script intentionally creates a CPU-intensive process.

Resource warning

Run only one test process, preferably on a VM or disposable lab machine. It can consume a full CPU core until stopped.

Step 4: Make the script executable

```
chmod +x cpu-test.sh
```

What does it do?

- `chmod`: Changes file permissions.
- `+x`: Adds execute permission.

Step 5: Run the script

```
./cpu-test.sh
```

The terminal will remain occupied because the script has an infinite loop.

Press:

```
Ctrl + C
```

This sends an interrupt signal to stop the foreground process.

### Practical 3: Run a process in the background

Run:

```
./cpu-test.sh &
```

What does `&` mean?

It runs the process in the background, allowing you to continue using the terminal.

Example:

```
[1] 3500
```

Here:

- `1` = Shell job number.
- `3500` = Process ID.

### Command 2: Display background jobs

```
jobs -l
```

Purpose:

Shows jobs started from the current shell, including their process IDs.

### Command 3: Find the test process

```
pgrep -af cpu-test
```

Purpose:

Searches for the running test script.

### Command 4: Inspect its CPU consumption

```
ps -p 3500 -o pid,ppid,stat,%cpu,%mem,cmd
```

Replace `3500` with your actual PID.

### Command 5: Stop the process gracefully

```
kill -TERM 3500
```

What is `TERM`?

`SIGTERM` asks the process to terminate gracefully.

It gives the application an opportunity to clean up resources and close connections.

### Command 6: Verify that it stopped

```
pgrep -af cpu-test
```

If the process has terminated, it should no longer appear.

If you started it from the current shell, you can also inspect:

```
jobs -l
```

### Understanding important signals

| Signal  | Number                         | Purpose                      |
| ------- | ------------------------------ | ---------------------------- |
| SIGHUP  | 1                              | Hangup                       |
| SIGINT  | 2                              | Interrupt                    |
| SIGKILL | 9                              | Force termination            |
| SIGTERM | 15                             | Request graceful termination |
| SIGSTOP | 19 on common x86 Linux systems | Stop a process               |
| SIGCONT | 18 on common x86 Linux systems | Continue a stopped process   |

Important distinction:

```
kill -15 PID
```

Requests graceful termination.

```
kill -9 PID
```

Forcibly terminates a process.

Professional rule: Always try `SIGTERM` first. Use `SIGKILL` only when necessary because the process cannot handle or ignore it and gets no opportunity to clean up.

## 13. Pause and resume processes

Sometimes you don't want to terminate a process. You only want to temporarily stop its execution.

Start your test process again:

```
./cpu-test.sh &
```

Find its PID:

```
pgrep -af cpu-test
```

Assume its PID is `3600`.

### Stop the process temporarily

```
kill -STOP 3600
```

What happens?

The process is suspended by the kernel.

### Check its state

```
ps -o pid,stat,cmd -p 3600
```

You should see `T` in its process state.

### Resume the process

```
kill -CONT 3600
```

What happens?

The process resumes execution.

### Finally terminate it

```
kill -TERM 3600
```

## 14. Process priority management

Scenario: Your server is running multiple processes. One background task is consuming CPU, but your application should receive a higher scheduling priority.

Linux provides two important commands:

- `nice`
- `renice`

### Understand nice values

Linux scheduling nice values generally range from:

\\[ -20 \text{ to } +19 \\]

| Nice value | Meaning                     |
| ---------- | --------------------------- |
| -20        | Highest scheduling priority |
| 0          | Default priority            |
| +10        | Lower scheduling priority   |
| +19        | Lowest scheduling priority  |

### Command 1: Start a low-priority process

```
nice -n 10 ./cpu-test.sh &
```

Explanation:

- `nice`: Starts a command with an adjusted scheduling niceness.
- `-n 10`: Sets the nice value to 10.
- `&`: Runs it in the background.

### Command 2: Check the nice value

```
ps -o pid,ni,pri,cmd -C bash
```

For a more precise check, use the actual PID:

```
ps -o pid,ni,pri,cmd -p 3700
```

### Command 3: Change an existing process priority

```
renice 15 -p 3700
```

Explanation:

- `renice`: Changes the nice value of an existing process.
- `15`: New nice value.
- `-p`: Specifies a PID.

Unprivileged users can generally increase nice values (lower scheduling priority) for their own processes. Lowering the nice value to increase priority generally requires elevated privileges.

Important: Nice values affect CPU scheduling priority. They do not directly limit CPU usage or guarantee application performance.

## 15. Process resource limits

Linux also allows administrators to inspect and control resource limits.

### Command 1: Display current shell limits

```
ulimit -a
```

Why?

It displays resource limits for the current shell and its child processes.

Examples include:

- Maximum open files
- Maximum user processes
- Maximum stack size
- Core dump size

### Command 2: Display maximum open files

```
ulimit -n
```

This displays the maximum number of file descriptors available to the current shell.

### Command 3: Check process file descriptors

```
ls -l /proc/3700/fd
```

Replace `3700` with the PID.

What is `/proc`?

It is a virtual filesystem that exposes kernel and process information.

The `fd` directory contains symbolic links representing the process's open file descriptors.

### Command 4: Display process status

```
cat /proc/3700/status
```

This displays detailed process information, including:

- Name
- State
- PID
- Parent PID
- Thread count
- Memory information

# Part C — Linux Service Management

## 16. Manage services using systemctl

Scenario: Your e-commerce application is configured as a Linux service. You need to inspect its status and control it.

Linux commonly uses `systemd` as its service and system manager.

The command used to manage systemd services is `systemctl`.

### Command 1: Check SSH service

```
sudo systemctl status ssh
```

Why?

Displays the status of the SSH service.

You may see:

```
Active: active (running)
```

This means the SSH service is currently running.

### Command 2: Start a service

```
sudo systemctl start ssh
```

Starts the SSH service if it is not already running.

### Command 3: Stop a service

```
sudo systemctl stop ssh
```

Stops the SSH service.

Caution

Never stop SSH on a remote server unless you have confirmed an alternative access method. You could lock yourself out.

### Command 4: Restart a service

```
sudo systemctl restart ssh
```

Stops and starts the service again.

### Command 5: Reload a service

```
sudo systemctl reload ssh
```

Requests the service to reload its configuration, if it supports reload.

### Command 6: Enable service at boot

```
sudo systemctl enable ssh
```

Configures the service to start automatically during boot.

### Command 7: Disable automatic startup

```
sudo systemctl disable ssh
```

Removes its automatic startup configuration.

It does not necessarily stop an already running service.

### Command 8: Display failed services

```
systemctl --failed
```

Why?

It displays units that have entered a failed state.

### Command 9: Check service logs

```
sudo journalctl -u ssh
```

Why?

`journalctl` reads logs from the systemd journal.

`-u ssh` filters logs for the SSH service.

Display the latest 50 lines:

```
sudo journalctl -u ssh -n 50
```

Follow logs in real time:

```
sudo journalctl -u ssh -f
```

Press `Ctrl + C` to stop following logs.

# Part D — Advanced Network Troubleshooting

## 17. Trace network connectivity

Scenario: Your Ubuntu server can reach its local gateway, but you cannot access an external application server.

You need to identify where packets are getting delayed or dropped.

### Command 1: Trace the network path

```
traceroute google.com
```

What happens?

It attempts to identify the intermediate network hops between your server and the destination.

Example:

```
1  192.168.1.1
2  10.10.0.1
3  ...
4  destination
```

Each line represents a network hop.

### Command 2: Trace using ICMP

```
traceroute -I google.com
```

`-I` uses ICMP Echo Requests rather than the default UDP probes.

Some networks filter traceroute traffic, so `* * *` does not necessarily indicate a broken connection.

### Command 3: Test a TCP port

```
sudo apt install netcat-openbsd -y
```

Now execute:

```
nc -vz google.com 443
```

Explanation:

- `nc`: Netcat utility.
- `-v`: Verbose output.
- `-z`: Scan without sending application data.
- `443`: HTTPS port.

Possible output:

```
Connection to google.com 443 port [tcp/https] succeeded!
```

This confirms that a TCP connection to that destination and port succeeded.

### Command 4: Test your own application port

```
nc -vz localhost 8080
```

If successful, your server has a reachable TCP listener on port 8080 from the local machine.

### Command 5: Inspect network connections

```
sudo ss -antp
```

This displays TCP sockets, including connection states and associated process information when available.

Common states:

| State      | Meaning                                               |
| ---------- | ----------------------------------------------------- |
| LISTEN     | Waiting for incoming connections                      |
| ESTAB      | Connection established                                |
| TIME-WAIT  | Waiting before connection cleanup                     |
| CLOSE-WAIT | Waiting for the local application to close its socket |
| SYN-SENT   | Connection request sent                               |
| SYN-RECV   | Connection request received                           |

A large number of `CLOSE-WAIT` connections may indicate that an application is not closing sockets properly. Investigate the application before taking action.

## 18. Network packet monitoring using tcpdump

This is an important skill for advanced Linux troubleshooting.

Scenario: Your application is not receiving expected requests. You want to inspect packets reaching port 8080.

### Command 1: Identify network interfaces

```
ip -br link
```

### Command 2: Capture packets on port 8080

```
sudo tcpdump -i any port 8080
```

Explanation:

- `tcpdump`: Network packet analyzer.
- `-i`: Selects the interface.
- `any`: Captures on available interfaces.
- `port 8080`: Filters packets using port 8080.

### Command 3: Capture only TCP traffic

```
sudo tcpdump -i any tcp port 8080
```

### Command 4: Display packet details

```
sudo tcpdump -nn -i any tcp port 8080
```

- First `-n`: Avoids hostname resolution.
- Second `-n`: Avoids service-name resolution.

### Command 5: Save captured packets

```
sudo tcpdump -i any port 8080 -w network-lab.pcap
```

This stores packets in a file that can be inspected using Wireshark.

Stop the capture with `Ctrl + C`.

### Command 6: Read the saved capture

```
sudo tcpdump -nn -r network-lab.pcap
```

This reads previously captured packets instead of capturing new traffic.

Security note: Packet captures can contain sensitive information. Only capture traffic you are authorized to inspect, and protect the resulting files.

# Part E — Complete Real-world Integrated Practical

Now let's combine everything into one exercise.

This is the most important part of the lab.

## 19. Create your own simulated e-commerce application

We will create a small Python HTTP server on port 8080 and intentionally introduce a CPU-intensive background process.

This is a safe, controlled way to reproduce the symptoms in the scenario.

### Step 1: Verify Python installation

```
python3 --version
```

If Python is missing:

```
sudo apt install python3 -y
```

### Step 2: Create the application directory

```
mkdir -p ~/ecommerce-lab
cd ~/ecommerce-lab
```

### Step 3: Create the application

```
nano app.py
```

Add:

```
from http.server import BaseHTTPRequestHandler, HTTPServerimport jsonimport timeclass EcommerceHandler(BaseHTTPRequestHandler):    def do_GET(self):        if self.path == "/health":            response = {                "status": "healthy",                "application": "ecommerce",                "timestamp": time.time()            }            self.send_response(200)            self.send_header("Content-Type", "application/json")            self.end_headers()            self.wfile.write(json.dumps(response).encode())        elif self.path == "/":            self.send_response(200)            self.end_headers()            self.wfile.write(b"Ecommerce Application Running")
```

Save the file.

### Understand the code

| Code                     | Explanation                        |
| ------------------------ | ---------------------------------- |
| `http.server`            | Python built-in HTTP server module |
| `BaseHTTPRequestHandler` | Handles incoming HTTP requests     |
| `HTTPServer`             | Creates an HTTP server             |
| `do_GET()`               | Handles HTTP GET requests          |
| `/health`                | Health-check endpoint              |
| `/`                      | Application home endpoint          |
| `200`                    | Successful response                |
| `404`                    | Resource not found                 |
| `0.0.0.0`                | Listen on all IPv4 interfaces      |
| `8080`                   | Application port                   |
| `serve_forever()`        | Keeps the server running           |

We are using Python's built-in server for learning, not as a production web server.

### Step 4: Start the application

```
python3 app.py
```

Expected output:

```
Ecommerce server running on port 8080
```

Keep this terminal open.

### Step 5: Open another terminal

Test the application:

```
curl http://localhost:8080
```

Expected output:

```
Ecommerce Application Running
```

Test the health endpoint:

```
curl http://localhost:8080/health
```

Expected output:

```
{
  "status": "healthy",
  "application": "ecommerce",
  "timestamp": 1793810000
}
```

The timestamp will differ.

### Step 6: Identify the running application process

```
pgrep -af app.py
```

Find its PID.

Then:

```
ps -p YOUR_PID -o pid,ppid,user,stat,%cpu,%mem,cmd
```

Replace `YOUR_PID` with the actual number.

### Step 7: Check the application port

```
sudo ss -tulnp | grep 8080
```

You should find Python listening on port 8080.

### Step 8: Generate test traffic

Open another terminal and run:

```
for i in {1..20}
do
    curl -s http://localhost:8080/health
    echo
done
```

What happens?

- The loop runs 20 times.
- Every iteration makes an HTTP request.
- `-s` suppresses curl's progress output.
- The application returns a health response.

### Step 9: Introduce the CPU issue

Create a background CPU-intensive process:

```
cd ~/linux-network-process-lab
./cpu-test.sh &
```

Find its PID:

```
pgrep -af cpu-test
```

Monitor:

```
top
```

You should observe the CPU-consuming test process.

### Step 10: Investigate the incident

Run the following commands in order:

```
uptime
```

Check the system load.

```
free -h
```

Check memory.

```
ps aux --sort=-%cpu | head -10
```

Identify the top ten CPU-consuming processes.

```
ps aux --sort=-%mem | head -10
```

Identify the top ten memory-consuming processes.

```
sudo ss -tulnp | grep 8080
```

Check whether the application is listening.

```
curl -I http://localhost:8080/health
```

Check the HTTP response.

```
sudo lsof -i :8080
```

Identify the process using the application port.

### Step 11: Resolve the CPU issue

Find the CPU test process:

```
pgrep -af cpu-test
```

Terminate it gracefully:

```
kill -TERM YOUR_CPU_PID
```

Check again:

```
ps aux --sort=-%cpu | head -10
```

The artificial CPU load should disappear.

### Step 12: Verify application health

```
curl http://localhost:8080/health
```

Check the port:

```
sudo ss -tulnp | grep 8080
```

Check CPU and memory:

```
top
```

Expected result

- CPU-intensive test process has terminated.
- Python application is still running.
- Port 8080 is listening.
- Health endpoint returns HTTP 200.
- The application is accessible locally.

Important distinction: In this simulated incident, we intentionally created CPU load. We did not intentionally break DNS, routing or the network interface. Your task is to use the same investigation commands to determine whether those other layers are working.

# Part F — Troubleshooting Reference

## 20. Common problems and their solutions

| Problem               | Investigation                    | Possible resolution                          |
| --------------------- | -------------------------------- | -------------------------------------------- |
| No IP address         | `ip addr`                        | Investigate interface and DHCP configuration |
| Gateway unreachable   | `ip route`, `ping`               | Investigate gateway and network              |
| DNS not resolving     | `dig`, `getent`                  | Investigate resolver configuration           |
| Port not listening    | `ss -tulnp`                      | Start or repair application                  |
| Port already occupied | `lsof -i :8080`                  | Identify the owner before changing the port  |
| High CPU              | `top`, `ps aux`                  | Investigate the responsible process          |
| High memory           | `free -h`, `ps aux --sort=-%mem` | Investigate memory usage                     |
| Application stopped   | `systemctl status`               | Inspect logs and restart if appropriate      |
| Too many connections  | `ss -antp`                       | Investigate connection handling              |
| Service failing       | `journalctl`                     | Find the actual service error                |

## 21. Important commands cheat sheet

Keep this table as a quick reference during your Linux practice.

| Category | Command      | Purpose                              |
| -------- | ------------ | ------------------------------------ |
| Network  | `ip addr`    | Display IP addresses                 |
| Network  | `ip link`    | Display interfaces                   |
| Network  | `ip route`   | Display routes                       |
| Network  | `ping`       | Test ICMP connectivity               |
| Network  | `traceroute` | Trace network path                   |
| Network  | `nslookup`   | DNS lookup                           |
| Network  | `dig`        | Detailed DNS query                   |
| Network  | `ss -tulnp`  | Inspect listening sockets            |
| Network  | `curl`       | Test HTTP requests                   |
| Network  | `nc`         | Test TCP connections                 |
| Network  | `tcpdump`    | Capture packets                      |
| Process  | `ps aux`     | List processes                       |
| Process  | `top`        | Monitor processes                    |
| Process  | `htop`       | Interactive monitoring               |
| Process  | `pgrep`      | Find process IDs                     |
| Process  | `pstree`     | Display process hierarchy            |
| Process  | `kill`       | Send signals                         |
| Process  | `nice`       | Start process with adjusted priority |
| Process  | `renice`     | Change process priority              |
| Process  | `free -h`    | Check memory                         |
| Process  | `vmstat`     | Check system statistics              |
| Process  | `lsof`       | List open files                      |
| Process  | `ulimit`     | Inspect resource limits              |
| Service  | `systemctl`  | Manage services                      |
| Logs     | `journalctl` | Read systemd logs                    |

# Part G — Student Assessment and Interview Practice

## 22. Practical tasks without looking at the solution

Now test yourself. These are the tasks I would give to a junior Linux Administrator.

Level 1 — Beginner

10 Tasks

0/10 completed

0%

1\. Display your Linux hostname.

2\. Identify your Ubuntu version.

3\. Display all network interfaces.

4\. Find your server's IPv4 address.

5\. Check the default gateway.

6\. Ping your default gateway four times.

7\. Find the DNS resolver configured on your machine.

8\. Display all running processes.

9\. Find the PID of the SSH service.

10\. Display available RAM and swap memory.

Level 2 — Intermediate

10 Tasks

0/10 completed

1\. Find which process is consuming the most CPU.

2\. Find the top five memory-consuming processes.

3\. Identify which process is listening on port 8080.

4\. Display all established TCP connections.

5\. Start a process in the background.

6\. Stop a process using SIGTERM.

7\. Pause and resume a process using signals.

8\. Change the nice value of a test process.

9\. Identify processes using a particular file.

10\. Display the last 50 SSH service log entries.

Level 3 — Advanced

5 Tasks

0/5 completed

1\. Investigate a server that has an IP address but cannot resolve domain names.

2\. Find the application responsible for occupying port 8080.

3\. Capture and inspect TCP packets on port 8080.

4\. Investigate a service that repeatedly fails to start.

5\. Prepare a complete incident report with root cause, resolution and verification.

Overall progress

# 0/25

Complete the tasks in your own terminal, not just by reading the commands.

## 23. Linux interview questions

Try answering these without looking at the explanations.

1. What is the difference between a process and a thread?
2. What is the difference between `ps aux` and `ps -ef`?
3. What is the difference between `kill -9` and `kill -15`?
4. What is a zombie process?
5. How do you find which process is using port 8080?
6. What is the difference between TCP and UDP?
7. What is the difference between `ip addr` and `ip route`?
8. What happens when you execute `ping 8.8.8.8`?
9. What is DNS and why do we need it?
10. What is the difference between `ss` and `netstat`?
11. How do you troubleshoot a high CPU process?
12. What is the difference between `nice` and `renice`?
13. What is the purpose of `/proc`?
14. What is the difference between `systemctl restart` and `systemctl reload`?
15. How do you investigate why an application is not listening on port 8080?

## 24. Final incident report template

In real DevOps and SRE jobs, troubleshooting is not finished when the command works. You must document what happened.

Use this template:

# Linux Network & Process Management — Incident Report

## 1. Incident Information

- Server hostname:
- Operating system:
- Incident date:
- Application name:
- Application port: 8080
- Incident severity:

## 2. Problem Description

Describe the reported issue:

- Application response time:
- Network connectivity:
- CPU utilization:
- Memory utilization:
- Application availability:

## 3. Investigation Performed

| Investigation            | Command          | Result |
| ------------------------ | ---------------- | ------ |
| Network interface        | `ip addr`        | <br>   |
| Routing                  | `ip route`       | <br>   |
| DNS                      | `dig google.com` | <br>   |
| Listening ports          | `ss -tulnp`      | <br>   |
| CPU utilization          | `top`            | <br>   |
| Process inspection       | `ps aux`         | <br>   |
| Application connectivity | `curl`           | <br>   |
| Application logs         | `journalctl`     | <br>   |

## 4. Root Cause

Describe the actual reason for the incident.

## 5. Resolution

List the commands and actions used to resolve the issue.

## 6. Verification

- Network connectivity verified
- DNS resolution verified
- Application port verified
- CPU utilization checked
- Memory utilization checked
- Application health endpoint verified
- Application logs reviewed

## 7. Preventive Measures

Describe what can be implemented to prevent similar issues.

## 8. Final Status

Incident status: Resolved / Unresolved

Engineer name: \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

## 25. Cleanup — Remove your lab processes and files

After completing the practical, clean up your test environment.

Stop the test process if it is still running:

```
pkill -TERM -f '[c]pu-test.sh'
```

Stop the Python application using `Ctrl + C` in its terminal.

Check that the test process has stopped:

```
pgrep -af cpu-test
```

Check that port 8080 is no longer listening:

```
sudo ss -tulnp | grep 8080
```

Remove your practice directories:

```
rm -rf ~/linux-network-process-lab
rm -rf ~/ecommerce-lab
```

Only run these removal commands after confirming that these are your dedicated lab directories.

## 26. What you should learn next

Once you complete this practical, follow this progression:

1. Linux Networking Advanced

   Subnetting, CIDR, routing, DNS, DHCP, firewalls, NAT, iptables and nftables.
2. Linux Process Advanced

   Threads, signals, zombie processes, orphan processes, cgroups and resource limits.
3. Linux Performance Monitoring

   CPU bottlenecks, memory leaks, disk I/O, load averages and performance analysis.
4. Linux Production Troubleshooting

   systemd, journalctl, SSH, firewall, service failures and incident response.
5. DevOps and SRE Integration

   Prometheus, Grafana, Node Exporter, alerting and automated remediation.

My recommendation: Don't try to memorize 50 commands in one day. Run each command, observe the output, deliberately create a small problem in your lab, and then troubleshoot it. That's how you develop actual Linux administration skills.
