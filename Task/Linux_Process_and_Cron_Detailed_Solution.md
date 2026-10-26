# Linux Process & Cron — Detailed Solution

## 1. User and Identity

```bash
whoami
id
```

`whoami` gives the current username. `id` shows UID, GID and supplementary groups.

---

## 2. Process Inspection

```bash
ps
ps -f
ps aux
ps -ef
```

- `ps` = processes associated with the current terminal
- `ps aux` = detailed list of running processes
- `ps -ef` = detailed process list including PPID
- PID = Process ID
- PPID = Parent Process ID

Process hierarchy can look like:

```text
systemd
 └── sshd
      └── bash
           └── application
```

Find processes:

```bash
pgrep ssh
pidof sshd
ps aux | grep ssh
```

---

## 3. Monitoring

```bash
top
```

Look at PID, USER, `%CPU`, `%MEM`, STAT and COMMAND.

Continuous report:

```bash
watch -n 2 "ps -eo pid,ppid,user,%cpu,%mem,stat,cmd --sort=-%cpu | head -15"
```

High CPU does not automatically mean a process should be killed. Identify the application first.

---

## 4. Foreground and Background

Foreground:

```bash
sleep 300
```

The shell waits.

Background:

```bash
sleep 300 &
```

Example:

```text
[1] 2456
```

`1` is the shell job ID; `2456` is the PID.

Check:

```bash
jobs
ps -p 2456
```

Suspend/resume:

```bash
sleep 300
# Ctrl+Z
jobs
bg %1
fg %1
```

---

## 5. Process Termination

```bash
sleep 500 &
pgrep sleep
kill <PID>
```

Normal `kill` sends SIGTERM (15):

```bash
kill -15 <PID>
```

SIGTERM requests graceful shutdown.

SIGKILL:

```bash
kill -9 <PID>
```

SIGKILL forces termination and cannot be handled by the target. Prefer SIGTERM first because applications may need cleanup.

---

## 6. Priority

Check:

```bash
ps -eo pid,ni,pri,cmd | head
```

Start lower priority:

```bash
nice -n 10 sleep 300 &
```

Change existing process:

```bash
renice 15 -p <PID>
```

Higher nice value generally means lower CPU scheduling priority.

---

## 7. nohup

```bash
mkdir -p ~/process-lab
nohup bash -c 'while true; do date; sleep 10; done' > ~/process-lab/app-monitor.log 2>&1 &
```

- `nohup` helps survive terminal logout
- `>` redirects output
- `2>&1` sends errors to the same log
- `&` backgrounds the process

Check:

```bash
pgrep -af "while true"
tail -f ~/process-lab/app-monitor.log
```

Stop the process using its PID:

```bash
kill <PID>
```

---

# 8. Cron Service

Ubuntu/Debian:

```bash
systemctl status cron
```

RHEL-family:

```bash
systemctl status crond
```

Start if needed:

```bash
sudo systemctl enable --now cron
```

or:

```bash
sudo systemctl enable --now crond
```

---

# 9. Cron Syntax

```text
minute hour day-of-month month day-of-week command
```

Examples:

```cron
* * * * * command
```
Every minute.

```cron
*/5 * * * * command
```
Every 5 minutes.

```cron
0 * * * * command
```
Every hour.

```cron
0 2 * * * command
```
Every day at 02:00.

```cron
0 0 * * 0 command
```
Every Sunday at midnight.

---

# 10. Test Cron

Create:

```bash
mkdir -p ~/cron-lab
```

Find username:

```bash
whoami
echo "$HOME"
```

Edit:

```bash
crontab -e
```

Example:

```cron
* * * * * date >> /home/sahil/cron-lab/cron-test.log 2>&1
```

Use your actual home path.

After a few minutes:

```bash
cat ~/cron-lab/cron-test.log
```

Remove the test entry after verification.

---

# 11. Health Check Solution

Create:

```bash
nano ~/cron-lab/health-check.sh
```

Example:

```bash
#!/bin/bash

LOG="$HOME/cron-lab/health.log"
TIMESTAMP=$(date '+%Y-%m-%d %H:%M:%S')

if pgrep -x "sshd" > /dev/null; then
    echo "$TIMESTAMP - Application UP" >> "$LOG"
else
    echo "$TIMESTAMP - Application DOWN" >> "$LOG"
fi
```

For a real application, replace `sshd` with the real process name such as `nginx`, `java`, `node`, etc.

Make executable:

```bash
chmod +x ~/cron-lab/health-check.sh
```

Test:

```bash
~/cron-lab/health-check.sh
cat ~/cron-lab/health.log
```

Schedule:

```cron
*/5 * * * * /home/YOUR_USERNAME/cron-lab/health-check.sh
```

---

# 12. Backup Solution

Prepare:

```bash
mkdir -p ~/cron-lab/application-data ~/cron-lab/backups

echo "Application configuration" > ~/cron-lab/application-data/config.txt
echo "Customer data sample" > ~/cron-lab/application-data/customers.txt
echo "Application log sample" > ~/cron-lab/application-data/app.log
```

Create `backup.sh`:

```bash
#!/bin/bash

SOURCE="$HOME/cron-lab/application-data"
BACKUP_DIR="$HOME/cron-lab/backups"
LOG="$HOME/cron-lab/backup.log"

TIMESTAMP=$(date '+%Y-%m-%d-%H%M%S')
BACKUP_FILE="$BACKUP_DIR/application-backup-$TIMESTAMP.tar.gz"

mkdir -p "$BACKUP_DIR"

if tar -czf "$BACKUP_FILE" -C "$SOURCE" .; then
    echo "$(date '+%Y-%m-%d %H:%M:%S') - Backup SUCCESS: $BACKUP_FILE" >> "$LOG"
else
    echo "$(date '+%Y-%m-%d %H:%M:%S') - Backup FAILED" >> "$LOG"
fi
```

Then:

```bash
chmod +x ~/cron-lab/backup.sh
~/cron-lab/backup.sh
ls -lh ~/cron-lab/backups
cat ~/cron-lab/backup.log
```

Schedule:

```cron
0 2 * * * /home/YOUR_USERNAME/cron-lab/backup.sh
```

---

# 13. Cleanup Solution

Create:

```bash
mkdir -p ~/cron-lab/test-logs
touch ~/cron-lab/test-logs/app1.log
touch ~/cron-lab/test-logs/app2.log
```

Create `cleanup.sh`:

```bash
#!/bin/bash

LOG_DIR="$HOME/cron-lab/test-logs"
LOG="$HOME/cron-lab/cleanup.log"

find "$LOG_DIR" -type f -name "*.log" -mtime +7 -print -delete >> "$LOG" 2>&1

echo "$(date '+%Y-%m-%d %H:%M:%S') - Cleanup completed" >> "$LOG"
```

Make executable:

```bash
chmod +x ~/cron-lab/cleanup.sh
```

Test:

```bash
~/cron-lab/cleanup.sh
cat ~/cron-lab/cleanup.log
```

Schedule Sunday 03:00:

```cron
0 3 * * 0 /home/YOUR_USERNAME/cron-lab/cleanup.sh
```

Use only the test directory during the lab.

---

# 14. Resource Report Solution

Create:

```bash
mkdir -p ~/cron-lab/reports
```

Create `resource-report.sh`:

```bash
#!/bin/bash

REPORT_DIR="$HOME/cron-lab/reports"
TIMESTAMP=$(date '+%Y-%m-%d-%H%M%S')
REPORT="$REPORT_DIR/resource-report-$TIMESTAMP.txt"

{
    echo "===== SYSTEM RESOURCE REPORT ====="
    echo "Generated: $(date)"

    echo
    echo "===== MEMORY ====="
    free -h

    echo
    echo "===== DISK ====="
    df -h

    echo
    echo "===== UPTIME ====="
    uptime

    echo
    echo "===== TOP CPU PROCESSES ====="
    ps -eo pid,user,%cpu,%mem,cmd --sort=-%cpu | head -6

    echo
    echo "===== TOP MEMORY PROCESSES ====="
    ps -eo pid,user,%cpu,%mem,cmd --sort=-%mem | head -6
} > "$REPORT"
```

Make executable:

```bash
chmod +x ~/cron-lab/resource-report.sh
```

Test:

```bash
~/cron-lab/resource-report.sh
ls -lh ~/cron-lab/reports
```

Schedule hourly:

```cron
0 * * * * /home/YOUR_USERNAME/cron-lab/resource-report.sh
```

---

# 15. Final Crontab

```cron
*/5 * * * * /home/YOUR_USERNAME/cron-lab/health-check.sh
0 2 * * * /home/YOUR_USERNAME/cron-lab/backup.sh
0 3 * * 0 /home/YOUR_USERNAME/cron-lab/cleanup.sh
0 * * * * /home/YOUR_USERNAME/cron-lab/resource-report.sh
```

Verify:

```bash
crontab -l
```

---

# 16. Troubleshooting Cron

Use this flow:

```text
Cron job not running
       |
       v
cron/crond active?
       |
       v
crontab syntax correct?
       |
       v
script runs manually?
       |
       v
executable?
       |
       v
absolute paths?
       |
       v
correct permissions?
       |
       v
PATH/environment?
       |
       v
cron logs?
       |
       v
script output/error log?
```

Commands:

```bash
crontab -l
systemctl status cron
systemctl status crond
journalctl -u cron
journalctl -u crond
grep CRON /var/log/syslog
```

A very common production mistake is using relative paths. Prefer:

```cron
/home/user/cron-lab/backup.sh
```

instead of:

```cron
./backup.sh
```

Cron also has a limited environment, so scripts should generally use absolute paths and explicit variables.

---

# Interview Answers

1. **Process:** running instance of a program.
2. **PID:** unique process ID.
3. **PPID:** parent process ID.
4. **Job vs process:** job is a shell management concept; process is an OS execution entity.
5. **Foreground:** owns the terminal. **Background:** shell remains usable.
6. `&` starts a command in the background.
7. `Ctrl+Z` suspends a foreground job.
8. `bg` resumes a stopped job in background.
9. `fg` brings a job to foreground.
10. SIGTERM requests graceful termination; SIGKILL forces termination.
11. `nice` starts a process with a chosen nice value.
12. `renice` changes an existing process's nice value.
13. `nohup` helps a process continue after terminal/session disconnect.
14. `cron` is a recurring time-based scheduler.
15. Cron fields are minute, hour, day-of-month, month, day-of-week.
16. `*/5 * * * *` = every 5 minutes.
17. Absolute paths avoid cron working-directory/PATH problems.
18. Manual success but cron failure often means PATH, permissions, environment, working directory, interpreter or path problems.
19. `crontab -l` lists the user's cron jobs.
20. Troubleshoot service → syntax → manual execution → permissions → paths → environment → logs → output.

## Final Principle

```text
PROGRAM
  ↓
PROCESS
  ↓
PID / PPID
  ↓
MONITOR
  ↓
CONTROL / SIGNAL
  ↓
PRIORITY
  ↓
FOREGROUND / BACKGROUND
  ↓
CRON
  ↓
SCHEDULE
  ↓
SCRIPT
  ↓
LOG
  ↓
VERIFY
  ↓
TROUBLESHOOT
```

The real industry skill is not merely creating a cron entry. Students should be able to **schedule it, prove it executed, capture its output, and troubleshoot it when it fails.**
