# Linux Practical Task — Process Management & Cron Jobs

## Scenario
You are a Linux Application Support Engineer. Manage processes on an application server and automate health checks, backups, cleanup and resource reports with cron.

## Objectives
- Understand processes, PID and PPID
- Monitor CPU/memory
- Use foreground/background jobs
- Use `ps`, `top`, `pgrep`, `pidof`
- Control processes with `kill`, `pkill`
- Understand SIGTERM/SIGKILL
- Use `nice`, `renice`, `nohup`
- Create and troubleshoot cron jobs
- Automate health checks, backups, cleanup and reports

---

## Part 1 — Process Management

### Task 1: Identify your user
```bash
whoami
id
```
Record username, UID, primary GID and supplementary groups.

### Task 2: Explore processes
```bash
ps
ps -f
ps aux
ps -ef
```
Answer: What are PID and PPID? Which process is your shell?

### Task 3: Find processes
```bash
ps aux | grep ssh
pgrep ssh
pidof sshd
```
If SSH is unavailable, use another running process.

### Task 4: Monitor processes
```bash
top
```
Exit with `q`. If available:
```bash
htop
```
Identify the highest CPU and memory consumers.

### Task 5: Continuous process report
```bash
watch -n 2 "ps -eo pid,ppid,user,%cpu,%mem,stat,cmd --sort=-%cpu | head -15"
```
Stop with `Ctrl+C`.

### Task 6: Foreground/background
```bash
sleep 300
```
Stop with `Ctrl+C`.

Then:
```bash
sleep 300 &
jobs
ps
```
Explain the difference between job ID and PID.

### Task 7: Stop/resume jobs
```bash
sleep 300
```
Press `Ctrl+Z`, then:
```bash
jobs
bg
fg
```
Finally press `Ctrl+C`.

### Task 8: Terminate a process
```bash
sleep 500 &
pgrep sleep
kill <PID>
pgrep sleep
```

Compare:
```bash
kill -15 <PID>
kill -9 <PID>
```
Explain SIGTERM vs SIGKILL and why `kill -9` should not be the first option.

### Task 9: Process priority
```bash
ps -eo pid,ni,pri,cmd | head
nice -n 10 sleep 300 &
pgrep sleep
ps -p <PID> -o pid,ni,pri,cmd
renice 15 -p <PID>
```

### Task 10: nohup
```bash
mkdir -p ~/process-lab
nohup bash -c 'while true; do date; sleep 10; done' > ~/process-lab/app-monitor.log 2>&1 &
```
Check:
```bash
pgrep -af "while true"
tail -f ~/process-lab/app-monitor.log
```
Stop `tail` with `Ctrl+C`, then terminate the monitor process safely.

---

# Part 2 — Cron Scheduling

## Task 11: Check cron
Ubuntu/Debian:
```bash
systemctl status cron
```
RHEL/CentOS/Fedora:
```bash
systemctl status crond
```

## Task 12: Understand cron syntax
```text
* * * * * command
│ │ │ │ │
│ │ │ │ └ day of week
│ │ │ └── month
│ │ └──── day of month
│ └────── hour
└──────── minute
```

Interpret:
```text
* * * * *
*/5 * * * *
0 * * * *
0 2 * * *
0 0 * * 0
```

## Task 13: First cron job
```bash
mkdir -p ~/cron-lab
crontab -e
```
Add, replacing `YOUR_USERNAME`:
```cron
* * * * * date >> /home/YOUR_USERNAME/cron-lab/cron-test.log 2>&1
```
Wait 2–3 minutes:
```bash
cat ~/cron-lab/cron-test.log
```
Remove the test entry afterward.

## Task 14: Application health check
Create `~/cron-lab/health-check.sh` that:
1. records timestamp;
2. checks a target process;
3. writes `UP` or `DOWN` to `~/cron-lab/health.log`.

Make executable:
```bash
chmod +x ~/cron-lab/health-check.sh
```
Schedule every 5 minutes.

## Task 15: Automated backup
Create:
```bash
mkdir -p ~/cron-lab/application-data ~/cron-lab/backups
echo "Application configuration" > ~/cron-lab/application-data/config.txt
echo "Customer data sample" > ~/cron-lab/application-data/customers.txt
echo "Application log sample" > ~/cron-lab/application-data/app.log
```
Create `backup.sh` that creates:
```text
application-backup-YYYY-MM-DD-HHMMSS.tar.gz
```
under `~/cron-lab/backups/` and logs success/failure. Schedule daily at 02:00.

## Task 16: Log cleanup
Create `~/cron-lab/test-logs/` and a `cleanup.sh` that deletes `.log` files older than 7 days using `find`. Schedule Sunday at 03:00.
Do not test deletion against `/` or system directories.

## Task 17: Resource report
Create `resource-report.sh` that records:
- date/time
- memory (`free -h`)
- disk (`df -h`)
- uptime
- top 5 CPU-consuming processes
- top 5 memory-consuming processes

Store reports in `~/cron-lab/reports/` and schedule hourly.

## Task 18: Cron troubleshooting
Use:
```bash
crontab -l
journalctl -u cron
journalctl -u crond
grep CRON /var/log/syslog
```
Check:
- cron service
- cron syntax
- executable permission
- absolute paths
- user permissions
- PATH/environment
- script output/error logs

---

# Final Industry Challenge

Configure:

```cron
*/5 * * * * /home/YOUR_USERNAME/cron-lab/health-check.sh
0 2 * * * /home/YOUR_USERNAME/cron-lab/backup.sh
0 3 * * 0 /home/YOUR_USERNAME/cron-lab/cleanup.sh
0 * * * * /home/YOUR_USERNAME/cron-lab/resource-report.sh
```

Prove each job is scheduled and explain how you would troubleshoot a job that does not execute.

## Submission
- Process/PID evidence
- `top` evidence
- Background/foreground demonstration
- `kill` and priority evidence
- `nohup` log
- `crontab -l`
- Health-check script/log
- Backup script/archive/log
- Cleanup script
- Resource report
- Cron troubleshooting evidence
- Answers to 20 interview questions

## Core Mental Model

```text
PROGRAM → PROCESS → PID/PPID → MONITOR → SIGNAL/CONTROL
                         ↓
                 FOREGROUND/BACKGROUND
                         ↓
                       CRON
                         ↓
             SCHEDULE → SCRIPT → LOG
                         ↓
                     VERIFY
                         ↓
                  TROUBLESHOOT
```
