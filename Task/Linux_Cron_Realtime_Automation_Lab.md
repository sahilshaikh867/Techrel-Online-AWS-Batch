# Linux Cron Job – Real-Time Automation Lab

> **Hands-on Practical:** Run a Bash script automatically every minute and watch timestamped logs appear in real time.

**Difficulty:** Beginner → Intermediate  
**Environment:** Ubuntu / Debian / Linux VM / Linux Server  
**Topics:** Cron, Bash Scripting, File Permissions, Scheduling, Logging, Monitoring, Troubleshooting

---

## 🎯 Learning Objectives

By completing this practical, you will learn how to:

- Create a Linux project directory.
- Create and edit a Bash script.
- Understand the Bash shebang.
- Capture the current date and time.
- Write timestamped output to a log file.
- Understand `>>` append redirection.
- Grant execute permission using `chmod`.
- Test a script manually before scheduling it.
- Create and manage a `crontab`.
- Understand all five cron scheduling fields.
- Run a script automatically every minute.
- Monitor the log in real time using `tail -f`.
- Troubleshoot common cron failures.
- Change cron schedules.
- Safely remove a scheduled cron job.

---

# 🏢 Real-World Industry Scenario

Imagine you are working as a **Linux / DevOps Engineer**.

Your operations team has a small maintenance script that needs to run automatically. The script could perform tasks such as:

- Checking disk usage
- Checking application health
- Creating backups
- Cleaning temporary files
- Generating reports
- Checking certificate expiry
- Rotating or collecting logs
- Performing scheduled maintenance

Instead of asking an engineer to run the script manually, Linux **cron** can execute it automatically.

For this lab, we will create a simple script that records:

```text
[CRON SUCCESS] This task automatically executed at: 2026-10-26 13:22:00
```

We will schedule it **every minute** so that we can observe the automation immediately.

---

# 🔄 How the System Works

```text
                 Linux System
                     |
                     v
               Cron Daemon
               (crond/cron)
                     |
                     | Every minute
                     v
              User's Crontab
                     |
                     v
                tracker.sh
                     |
                     | date + timestamp
                     v
                 output.log
                     |
                     v
                 tail -f
                     |
                     v
             Live Terminal Output
```

### Execution Flow

```text
1. Cron checks the schedule
        ↓
2. A minute matches "* * * * *"
        ↓
3. Cron launches tracker.sh
        ↓
4. Bash executes the script
        ↓
5. Script generates current timestamp
        ↓
6. Timestamp is appended to output.log
        ↓
7. tail -f displays the new line
```

---

# Step 1 – Check Your Linux Environment

Before creating anything, confirm that you are working inside a Linux shell.

## 1. Check your username

```bash
whoami
```

Example:

```text
sahil
```

Your username is important because cron jobs should use the correct absolute path.

---

## 2. Check your current directory

```bash
pwd
```

Example:

```text
/home/sahil
```

---

## 3. Check your Linux system

```bash
uname -a
```

---

## 4. Check whether cron is available

```bash
command -v cron || command -v crond || true
```

Depending on your Linux distribution, the cron service may be called:

- `cron`
- `crond`

> **Important:** Commands and service names can vary between Linux distributions.

---

# Step 2 – Create a Dedicated Project Directory

We will keep our script and log file inside one dedicated directory.

Run:

```bash
mkdir -p ~/cron-debug-task
```

Move into the directory:

```bash
cd ~/cron-debug-task
```

Check your location:

```bash
pwd
```

Check the directory contents:

```bash
ls -la
```

At this point, the directory should be empty or contain only default entries.

---

# Step 3 – Create the Bash Script

We will create a script called:

```text
tracker.sh
```

Open Nano:

```bash
nano tracker.sh
```

Paste the following script:

```bash
#!/bin/bash

# Capture the exact current system date and time
CURRENT_TIME=$(date "+%Y-%m-%d %H:%M:%S")

# Append one timestamped record to the log file
echo "[CRON SUCCESS] This task automatically executed at: $CURRENT_TIME" >> ~/cron-debug-task/output.log
```

---

## Understanding the Script

### Line 1 – Shebang

```bash
#!/bin/bash
```

This tells Linux:

> Run this script using the Bash shell.

---

### Line 2 – Comment

```bash
# Capture the exact current system date and time
```

Anything beginning with `#` is a comment.

Comments help developers understand the code.

---

### Line 3 – Capture the timestamp

```bash
CURRENT_TIME=$(date "+%Y-%m-%d %H:%M:%S")
```

Here:

```text
CURRENT_TIME
```

is a variable.

The command:

```bash
date
```

returns the current system date and time.

The format:

```text
%Y-%m-%d %H:%M:%S
```

means:

| Format | Meaning | Example |
|---|---|---|
| `%Y` | Year | 2026 |
| `%m` | Month | 10 |
| `%d` | Day | 26 |
| `%H` | Hour | 13 |
| `%M` | Minute | 25 |
| `%S` | Second | 00 |

Therefore:

```text
2026-10-26 13:25:00
```

---

### Final command

```bash
echo "[CRON SUCCESS] This task automatically executed at: $CURRENT_TIME" >> ~/cron-debug-task/output.log
```

`echo` prints text.

The variable:

```bash
$CURRENT_TIME
```

is replaced with the actual timestamp.

---

## What Does `>>` Mean?

The operator:

```bash
>>
```

means:

> Append the output to the end of a file.

For example:

```bash
echo "Hello" >> output.log
```

adds a new line.

It does **not** erase previous content.

### Difference

```bash
>
```

Overwrites the file.

```bash
>>
```

Appends to the file.

This distinction is extremely important when working with logs.

---

# Step 4 – Save and Exit Nano

Inside Nano:

```text
Ctrl + O
```

Press:

```text
Enter
```

Then exit:

```text
Ctrl + X
```

You should now be back at your terminal.

Verify the file:

```bash
ls -l
```

You should see:

```text
tracker.sh
```

---

# Step 5 – Grant Execute Permission

Linux uses file permissions to control whether files can be executed.

Run:

```bash
chmod +x tracker.sh
```

Now check the permissions:

```bash
ls -l tracker.sh
```

You may see something similar to:

```text
-rwxr-xr-x 1 sahil sahil 280 Oct 26 13:20 tracker.sh
```

The important part is the presence of:

```text
x
```

`x` means **execute permission**.

---

# Step 6 – Test the Script Manually

Before involving cron, always test the script manually.

This is a very important DevOps troubleshooting practice.

Run:

```bash
./tracker.sh
```

Now check the log:

```bash
cat output.log
```

Expected output:

```text
[CRON SUCCESS] This task automatically executed at: 2026-10-26 13:21:30
```

Run it again:

```bash
./tracker.sh
```

Check again:

```bash
cat output.log
```

You should now have two lines.

Example:

```text
[CRON SUCCESS] This task automatically executed at: 2026-10-26 13:21:30
[CRON SUCCESS] This task automatically executed at: 2026-10-26 13:21:42
```

This proves that the script itself works.

---

# Step 7 – Verify File Permissions

Run:

```bash
ls -l tracker.sh
```

You can also inspect the permissions numerically:

```bash
stat tracker.sh
```

The exact output varies by Linux distribution.

The important point is that the script must be executable if you plan to execute it directly.

> In this lab, cron will explicitly invoke `/bin/bash`, so the script's execute bit is not strictly required for that particular cron line. We still set it because understanding executable permissions is an important Linux administration skill.

---

# Step 8 – Find the Absolute Path

Cron jobs should preferably use absolute paths.

Run:

```bash
pwd
```

You may get:

```text
/home/sahil/cron-debug-task
```

Then:

```bash
realpath tracker.sh
```

Example:

```text
/home/sahil/cron-debug-task/tracker.sh
```

Copy your actual path.

Do **not** blindly copy:

```text
/home/YOUR_USERNAME/...
```

Replace it with your real Linux username/path.

---

# Step 9 – Open the Crontab

Run:

```bash
crontab -e
```

This opens your personal cron configuration.

### Important

For this beginner lab, use:

```bash
crontab -e
```

rather than:

```bash
sudo crontab -e
```

because we want the job to belong to your normal user.

---

# Step 10 – Add the Cron Schedule

At the bottom of the crontab, add:

```text
* * * * * /bin/bash /home/YOUR_USERNAME/cron-debug-task/tracker.sh
```

For example, if:

```bash
whoami
```

returns:

```text
sahil
```

then:

```text
* * * * * /bin/bash /home/sahil/cron-debug-task/tracker.sh
```

---

# Step 11 – Understand the Cron Expression

The cron expression is:

```text
* * * * *
```

Cron has five scheduling fields.

```text
*    *    *    *    *
|    |    |    |    |
|    |    |    |    +-- Day of week
|    |    |    +------- Month
|    |    +------------ Day of month
|    +----------------- Hour
+---------------------- Minute
```

---

## Field 1 – Minute

```text
*
```

Means every minute.

Valid values:

```text
0-59
```

---

## Field 2 – Hour

```text
*
```

Means every hour.

Valid values:

```text
0-23
```

---

## Field 3 – Day of Month

```text
*
```

Means every day of the month.

Valid values:

```text
1-31
```

---

## Field 4 – Month

```text
*
```

Means every month.

Valid values:

```text
1-12
```

---

## Field 5 – Day of Week

```text
*
```

Means every day of the week.

Typically:

```text
0-7
```

where Sunday may be represented by `0` or `7`.

---

# Therefore

```text
* * * * *
```

means:

> Run the command every minute of every hour, every day, every month, regardless of the day of the week.

---

# Step 12 – Save the Crontab

If you are using Nano:

```text
Ctrl + O
```

Press:

```text
Enter
```

Then:

```text
Ctrl + X
```

You may see:

```text
crontab: installing new crontab
```

This means your crontab has been installed.

---

# Step 13 – Verify the Cron Job

Run:

```bash
crontab -l
```

You should see your entry:

```text
* * * * * /bin/bash /home/sahil/cron-debug-task/tracker.sh
```

If you don't see it, the job is not installed in your current user's crontab.

---

# Step 14 – Watch the Automation in Real Time

This is the fun part.

Open another terminal window.

Run:

```bash
tail -f ~/cron-debug-task/output.log
```

The command:

```bash
tail -f
```

continuously watches the file for new content.

Wait for the next minute boundary.

You should see:

```text
[CRON SUCCESS] This task automatically executed at: 2026-10-26 13:30:00
```

After another minute:

```text
[CRON SUCCESS] This task automatically executed at: 2026-10-26 13:31:00
```

Then:

```text
[CRON SUCCESS] This task automatically executed at: 2026-10-26 13:32:00
```

🔥 You are now watching a real automated Linux process execute.

---

# Step 15 – Stop the Live Monitor

To stop:

```text
Ctrl + C
```

Important:

`Ctrl+C` stops `tail -f`.

It does **not** stop the cron job.

The cron job will continue running every minute.

---

# Step 16 – Check the Complete Log

Run:

```bash
cat ~/cron-debug-task/output.log
```

This displays the complete history.

---

## Show Only the Last 10 Lines

```bash
tail -n 10 ~/cron-debug-task/output.log
```

---

## Count the Number of Executions

```bash
wc -l ~/cron-debug-task/output.log
```

Example:

```text
10 /home/sahil/cron-debug-task/output.log
```

This means the log contains 10 lines.

---

# Step 17 – Test the Automation for Three Minutes

Your task:

1. Start the live monitor.

```bash
tail -f ~/cron-debug-task/output.log
```

2. Wait for three cron executions.

3. Capture three timestamps.

Example:

```text
[CRON SUCCESS] This task automatically executed at: 2026-10-26 13:40:00
[CRON SUCCESS] This task automatically executed at: 2026-10-26 13:41:00
[CRON SUCCESS] This task automatically executed at: 2026-10-26 13:42:00
```

4. Stop `tail`:

```text
Ctrl + C
```

5. Verify:

```bash
tail -n 3 ~/cron-debug-task/output.log
```

---

# Step 18 – Troubleshooting

## Problem 1 – No new log entries

Check:

```bash
crontab -l
```

Make sure your cron line exists.

---

## Problem 2 – Script works manually but cron does not execute it

Test:

```bash
/bin/bash /home/YOUR_USERNAME/cron-debug-task/tracker.sh
```

If this works, inspect the cron entry.

Use an absolute path.

---

## Problem 3 – Wrong username

Run:

```bash
whoami
```

Then:

```bash
realpath ~/cron-debug-task/tracker.sh
```

Use the exact path returned by the command.

---

## Problem 4 – Permission denied

Check:

```bash
ls -l tracker.sh
```

Grant execute permission:

```bash
chmod +x tracker.sh
```

---

## Problem 5 – `output.log` does not exist

Run the script manually:

```bash
./tracker.sh
```

Then:

```bash
ls -l
```

You should see:

```text
tracker.sh
output.log
```

---

## Problem 6 – Check cron service

On Ubuntu/Debian, try:

```bash
systemctl status cron
```

On systems using `crond`, try:

```bash
systemctl status crond
```

The exact service name depends on the distribution.

---

# Step 19 – Cron Schedule Examples

## Every minute

```text
* * * * *
```

---

## Every 5 minutes

```text
*/5 * * * *
```

Meaning:

```text
00
05
10
15
20
25
30
35
40
45
50
55
```

---

## Every 10 minutes

```text
*/10 * * * *
```

---

## Every hour

```text
0 * * * *
```

Runs at:

```text
01:00
02:00
03:00
04:00
...
```

---

## Every day at 12:00 PM

```text
0 12 * * *
```

---

## Every day at 9:00 AM

```text
0 9 * * *
```

---

## Monday to Friday at 9:00 AM

```text
0 9 * * 1-5
```

Useful for weekday business operations.

---

## Every Sunday at midnight

```text
0 0 * * 0
```

---

# Step 20 – Real-World DevOps Exercise

## Scenario

You are a Junior DevOps Engineer.

Your application operations team wants an automated task that records when a maintenance check executes.

You need to implement the following:

### Requirements

1. Create:

```text
~/cron-debug-task
```

2. Create:

```text
tracker.sh
```

3. Add timestamp functionality.

4. Create:

```text
output.log
```

automatically through the script.

5. Grant execute permission.

6. Test the script manually.

7. Configure cron.

8. Run it every minute.

9. Capture at least three successful executions.

10. Monitor the log using:

```bash
tail -f
```

11. Verify the crontab using:

```bash
crontab -l
```

12. Document the output.

13. Remove the cron job after completing the lab.

---

# 🧪 Practical Challenge

Modify the script so that the log contains:

```text
========================================
CRON JOB EXECUTION
Server: <hostname>
User: <username>
Time: <current timestamp>
Status: SUCCESS
========================================
```

### Helpful commands

Get hostname:

```bash
hostname
```

Get username:

```bash
whoami
```

Get timestamp:

```bash
date "+%Y-%m-%d %H:%M:%S"
```

Your goal is to combine these into one Bash script.

---

# Step 21 – Cleanup

A cron job running every minute should not be left running forever.

Open the crontab:

```bash
crontab -e
```

Remove this line:

```text
* * * * * /bin/bash /home/YOUR_USERNAME/cron-debug-task/tracker.sh
```

Or comment it:

```text
# * * * * * /bin/bash /home/YOUR_USERNAME/cron-debug-task/tracker.sh
```

Save and exit.

---

## Verify Cleanup

Run:

```bash
crontab -l
```

Confirm that the active cron line has been removed or commented.

---

# Step 22 – Optional: Remove the Lab Directory

If you no longer need the files:

```bash
rm -rf ~/cron-debug-task
```

⚠️ **WARNING**

`rm -rf` permanently removes files.

Always verify the path before executing:

```bash
pwd
ls -la ~/cron-debug-task
```

For a learning environment, delete only the directory you intentionally created for this lab.

---

# ✅ Final Validation Checklist

Before marking the practical complete, verify:

- [ ] Linux username identified
- [ ] Project directory created
- [ ] `tracker.sh` created
- [ ] Bash shebang understood
- [ ] Timestamp functionality tested
- [ ] `>>` append operator understood
- [ ] Script tested manually
- [ ] Execute permission verified
- [ ] Absolute path identified
- [ ] `crontab -e` used
- [ ] Cron entry added
- [ ] Five cron fields understood
- [ ] `crontab -l` verified
- [ ] `tail -f` used
- [ ] Three automatic timestamps observed
- [ ] Log history verified
- [ ] Troubleshooting commands understood
- [ ] Cron schedule removed after testing

---

# 📚 Quick Command Reference

## Identity

```bash
whoami
pwd
hostname
uname -a
```

## Directory

```bash
mkdir -p ~/cron-debug-task
cd ~/cron-debug-task
ls -la
```

## Script

```bash
nano tracker.sh
chmod +x tracker.sh
./tracker.sh
```

## File Inspection

```bash
ls -l tracker.sh
cat output.log
tail -n 10 output.log
tail -f output.log
wc -l output.log
```

## Path

```bash
realpath tracker.sh
```

## Cron

```bash
crontab -e
crontab -l
```

## Service Check

```bash
systemctl status cron
systemctl status crond
```

## Cleanup

```bash
crontab -e
rm -rf ~/cron-debug-task
```

---

# 🧠 Key Concepts Learned

| Concept | Purpose |
|---|---|
| Bash Script | Automates Linux commands |
| `#!/bin/bash` | Selects Bash as interpreter |
| Variable | Stores reusable data |
| `date` | Gets current date/time |
| `echo` | Writes text |
| `>` | Overwrites a file |
| `>>` | Appends to a file |
| `chmod +x` | Adds execute permission |
| `crontab` | Defines scheduled jobs |
| `* * * * *` | Runs every minute |
| `tail -f` | Monitors a file live |
| `crontab -l` | Lists scheduled jobs |
| `systemctl` | Checks/manages services |

---

# 🎓 What You Learned

This practical connects several important Linux and DevOps concepts:

```text
Linux
  ↓
Bash
  ↓
Permissions
  ↓
Automation
  ↓
Cron
  ↓
Logging
  ↓
Monitoring
  ↓
Troubleshooting
```

These are simple building blocks, but they appear constantly in real production environments.

A DevOps engineer may use the same ideas for:

- Automated backups
- Database maintenance
- Log cleanup
- Health checks
- Monitoring scripts
- Report generation
- Certificate checks
- Temporary file cleanup
- Scheduled deployments
- Operational maintenance

> **Remember:** In production, don't blindly schedule a script every minute just because cron can do it. Choose a schedule based on the business requirement, system load, failure behavior, and observability requirements.
