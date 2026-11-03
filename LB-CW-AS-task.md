# Real AWS production-style lab

The lab will follow this flow:

```text
                         USER
                           |
                           v
                    ┌─────────────┐
                    │     ALB     │
                    │Load Balancer│
                    └──────┬──────┘
                           |
              ┌────────────┴────────────┐
              |                         |
              v                         v
        ┌───────────┐             ┌───────────┐
        │   EC2 #1  │             │   EC2 #2  │
        │ Web Server │             │ Web Server │
        └─────┬─────┘             └─────┬─────┘
              |                         |
              └────────────┬────────────┘
                           |
                    ┌──────v──────┐
                    │  CloudWatch │
                    │ CPU/Metrics │
                    └──────┬──────┘
                           |
                           v
                    ┌─────────────┐
                    │ Auto Scaling │
                    │    Group     │
                    └──────┬──────┘
                           |
                  CPU > 60% → ADD EC2
                  CPU < 20% → REMOVE EC2
```

### What you will learn

| Component | Why we use it |
|---|---|
| EC2 | Runs our web application |
| Security Group | Controls network access |
| User Data | Automatically installs/configures the server |
| AMI | Template/image used to create identical EC2 servers |
| Target Group | Group of EC2 instances receiving traffic |
| ALB | Distributes traffic across EC2 instances |
| CloudWatch | Monitors CPU and other metrics |
| Auto Scaling Group | Automatically adds/removes EC2 instances |
| Scaling Policy | Defines when scaling should happen |
| SNS | Optional notification when scaling occurs |

---

# AWS LAB: HIGHLY AVAILABLE AUTO-SCALING WEB APPLICATION

**Level:** Beginner → Intermediate  
**AWS Services:** EC2, ALB, Target Groups, CloudWatch, Auto Scaling  
**Architecture:** 2 Availability Zones  
**Application:** Simple HTML web server  
**Goal:** Build everything from scratch.

---

## 1. First Understand the Problem

Imagine you have a website running on one EC2 instance.

```text
User
  |
  v
EC2
Website
```

Initially this looks fine.

But suppose 10 users visit.

Still fine.

Then 1,000 users visit.

Then 10,000 users visit.

Your EC2 server may become:

```text
CPU: 95%
Memory: High
Response: Slow
Users: Angry 😭
```

And there is another problem.

If the EC2 server crashes:

```text
User
  |
  v
EC2 ❌
```

Website is completely unavailable.

So we need a better architecture.

---

# 2. Solution

Instead of using one server:

```text
              User
                |
                v
              EC2
```

we use multiple servers:

```text
                User
                  |
                  v
             Load Balancer
              /         \
             /           \
            v             v
         EC2 #1        EC2 #2
```

Now traffic is distributed.

If EC2 #1 becomes unhealthy:

```text
                User
                  |
                  v
             Load Balancer
                  |
                  v
               EC2 #2
```

The Load Balancer can stop sending traffic to the unhealthy instance.

But there is still another problem.

What if traffic increases beyond what two servers can handle?

That's where **Auto Scaling** comes in.

---

# 3. Auto Scaling

Auto Scaling can automatically create additional EC2 instances.

For example:

```text
Normal traffic

ALB
 |
 +---- EC2 #1
 |
 +---- EC2 #2
```

Traffic increases:

```text
High traffic

ALB
 |
 +---- EC2 #1
 |
 +---- EC2 #2
 |
 +---- EC2 #3
```

Traffic increases further:

```text
Very high traffic

ALB
 |
 +---- EC2 #1
 |
 +---- EC2 #2
 |
 +---- EC2 #3
 |
 +---- EC2 #4
```

When traffic decreases:

```text
Low traffic

ALB
 |
 +---- EC2 #1
 |
 +---- EC2 #2
```

AWS removes unnecessary instances.

This is called:

> **Elasticity**

---

# 4. What is Load Balancing?

A Load Balancer receives requests from users and distributes those requests across multiple servers.

Without Load Balancer:

```text
User
 |
 +--> EC2
```

With Load Balancer:

```text
             User
               |
               v
             ALB
           /     \
          v       v
       EC2-1    EC2-2
```

### Why?

Because we don't want every user hitting one server.

---

# 5. What is ALB?

ALB means:

> **Application Load Balancer**

It works at the application layer and is commonly used for HTTP/HTTPS applications.

For this lab:

```text
Browser
   |
   | HTTP
   v
ALB
   |
   +--------> EC2
   |
   +--------> EC2
```

The user doesn't directly access the EC2 instances.

The user accesses the ALB.

---

# 6. What is a Target Group?

The ALB needs to know:

> "Which servers should I send traffic to?"

That's the job of a **Target Group**.

Example:

```text
Target Group

EC2-1
EC2-2
EC2-3
```

The ALB sends traffic to these targets.

The Target Group also performs **health checks**.

For example:

```text
ALB
 |
 +--> EC2-1 → Healthy
 |
 +--> EC2-2 → Healthy
 |
 +--> EC2-3 → Unhealthy
```

ALB stops sending traffic to EC2-3.

---

# 7. What is CloudWatch?

CloudWatch is AWS's monitoring service.

It collects metrics such as:

```text
CPU Utilization
Network Traffic
Disk activity
Request metrics
Load Balancer metrics
```

For example:

```text
EC2 CPU

10%
20%
35%
48%
65%
82%
95%
```

CloudWatch can detect this.

Then we can tell Auto Scaling:

> If CPU becomes too high, create another EC2.

---

# 8. What is Auto Scaling Group?

An **Auto Scaling Group**, or ASG, manages a group of EC2 instances.

We define:

```text
Minimum = 2
Desired = 2
Maximum = 4
```

Meaning:

### Minimum

AWS should never normally go below:

```text
2 instances
```

### Desired

Normally we want:

```text
2 instances
```

### Maximum

AWS shouldn't automatically exceed:

```text
4 instances
```

So:

```text
Minimum
   ↓
   2

Desired
   ↓
   2

Maximum
   ↓
   4
```

---

# 9. Complete Lab Architecture

At the end, your architecture should look like this:

```text
                         INTERNET
                             |
                             v
                   ┌──────────────────┐
                   │       ALB        │
                   │ Application LB   │
                   └────────┬─────────┘
                            |
                    Target Group
                            |
              ┌─────────────┴─────────────┐
              |                           |
              v                           v
        ┌────────────┐              ┌────────────┐
        │   EC2 #1   │              │   EC2 #2   │
        │ Web Server │              │ Web Server │
        └────────────┘              └────────────┘
              ^                           ^
              |                           |
              └───────────┬───────────────┘
                          |
                    Auto Scaling
                          |
              ┌───────────┴───────────┐
              |                       |
         Scale Out                Scale In
         CPU > 60%                CPU < 20%
              |                       |
              v                       v
          ADD EC2                  REMOVE EC2

                          ^
                          |
                    CloudWatch
                    CPU Metrics
```

---

# PART 1 — AWS ACCOUNT PREPARATION

## Step 1 — Login

Open the AWS Management Console.

Go to:

**EC2**

Before creating anything, select a region.

For example:

```text
Asia Pacific (Mumbai)
ap-south-1
```

Use one region consistently throughout the lab.

---

# PART 2 — CREATE SECURITY GROUP

A Security Group acts like a virtual firewall.

We need:

```text
HTTP  → 80
SSH   → 22
```

### Important

For SSH, don't blindly use:

```text
0.0.0.0/0
```

in production.

Ideally:

```text
My IP
```

should be used.

For this beginner lab, HTTP can be:

```text
0.0.0.0/0
```

because the website needs to be publicly accessible.

---

# PART 3 — CREATE WEB SERVER

Go to:

**EC2 → Instances → Launch Instance**

Choose:

```text
Name:
AutoScaling-WebServer
```

Choose an Amazon Linux AMI.

Choose an appropriate free-tier-eligible instance type if available to your account.

For example:

```text
t3.micro
```

The exact free-tier eligibility can vary by AWS account and current AWS pricing.

---

# PART 4 — CREATE THE WEB SERVER USING USER DATA

This is an important concept.

Instead of manually installing Apache every time, we can automatically configure the server when it boots.

This is called:

> **User Data**

Put this into the User Data section:

```bash
#!/bin/bash

dnf update -y

dnf install -y httpd

systemctl enable httpd
systemctl start httpd

cat <<EOF > /var/www/html/index.html
<!DOCTYPE html>
<html>
<head>
    <title>AWS Auto Scaling Lab</title>
</head>
<body>
    <h1>AWS Auto Scaling Lab</h1>
    <h2>Welcome to my Web Server</h2>
    <p>Server: $(hostname)</p>
    <p>Auto Scaling + Load Balancing + CloudWatch</p>
</body>
</html>
EOF
```

### What happens?

When EC2 starts:

```text
EC2 starts
   |
   v
User Data executes
   |
   v
Install Apache
   |
   v
Start Apache
   |
   v
Create website
```

---

# PART 5 — TEST THE EC2 SERVER

After launching the instance:

Wait until:

```text
Instance state = Running
```

and:

```text
Status checks = 2/2 checks passed
```

Copy:

```text
Public IPv4 address
```

Open:

```text
http://PUBLIC-IP
```

You should see:

```text
AWS Auto Scaling Lab

Welcome to my Web Server

Server: ip-xxx-xxx-xxx-xxx
```

Congratulations.

You created your first web server.

---

# PART 6 — WHY DO WE NEED AN AMI?

Now imagine we need 5 identical servers.

We don't want to manually configure:

```text
EC2 #1
EC2 #2
EC2 #3
EC2 #4
EC2 #5
```

Instead, we create a template.

That template is called an:

> **AMI — Amazon Machine Image**

Think of AMI as:

```text
MASTER SERVER
      |
      v
     AMI
   /  |  \
  /   |   \
EC2  EC2  EC2
```

The AMI contains the configuration required to launch identical instances.

---

# PART 7 — CREATE AMI

Select your configured EC2 instance.

Go to:

```text
Actions
→ Image and templates
→ Create image
```

Give it:

```text
Name:
auto-scaling-webserver-ami
```

Create the image.

Wait until the AMI becomes:

```text
Available
```

---

# PART 8 — CREATE TARGET GROUP

Go to:

```text
EC2
→ Target Groups
→ Create target group
```

Choose:

```text
Target type:
Instances
```

Protocol:

```text
HTTP
```

Port:

```text
80
```

Health check:

```text
Protocol: HTTP
Path: /
```

Give it a name:

```text
webserver-target-group
```

---

# PART 9 — CREATE APPLICATION LOAD BALANCER

Go to:

```text
EC2
→ Load Balancers
→ Create Load Balancer
```

Choose:

```text
Application Load Balancer
```

Name:

```text
webserver-alb
```

Scheme:

```text
Internet-facing
```

IP address type:

```text
IPv4
```

Select your VPC.

Select **at least two Availability Zones/subnets** where available.

Security Group:

Allow:

```text
HTTP 80
```

Listener:

```text
HTTP : 80
```

Forward traffic to:

```text
webserver-target-group
```

Create the ALB.

---

# PART 10 — REGISTER YOUR EC2

For the first test, register your EC2 instance with the target group.

Go to:

```text
Target Groups
→ webserver-target-group
→ Targets
→ Register targets
```

Select your EC2.

Port:

```text
80
```

Register it.

Wait for:

```text
Health status:
Healthy
```

---

# PART 11 — TEST LOAD BALANCER

Go to:

```text
EC2
→ Load Balancers
→ webserver-alb
```

Copy the:

```text
DNS name
```

It will look something like:

```text
webserver-alb-123456.ap-south-1.elb.amazonaws.com
```

Open:

```text
http://ALB-DNS-NAME
```

Your website should appear.

Now the architecture is:

```text
Browser
   |
   v
ALB
   |
   v
EC2
```

---

# PART 12 — CREATE LAUNCH TEMPLATE

Now we prepare for Auto Scaling.

Go to:

```text
EC2
→ Launch Templates
→ Create launch template
```

Name:

```text
webserver-launch-template
```

Select the AMI you created earlier:

```text
auto-scaling-webserver-ami
```

Choose your instance type.

Select your security group.

The launch template tells AWS:

> "Whenever you need to create a new server, use this configuration."

---

# PART 13 — CREATE AUTO SCALING GROUP

Go to:

```text
EC2
→ Auto Scaling Groups
→ Create Auto Scaling group
```

Name:

```text
webserver-asg
```

Select:

```text
webserver-launch-template
```

Select your VPC.

Select multiple subnets/AZs.

For example:

```text
ap-south-1a
ap-south-1b
```

This is important for high availability.

---

# PART 14 — SET CAPACITY

Set:

```text
Desired capacity:
2

Minimum capacity:
2

Maximum capacity:
4
```

Meaning:

```text
             Maximum
                4
                |
       ┌────────┴────────┐
       |                 |
       |   Auto Scaling  |
       |                 |
       └────────┬────────┘
                |
             Desired
                2
                |
             Minimum
                2
```

---

# PART 15 — CONNECT ASG TO LOAD BALANCER

During ASG configuration, choose:

```text
Attach to an existing load balancer
```

Select:

```text
webserver-target-group
```

Now:

```text
ASG
 |
 +---- EC2
 |
 +---- EC2
```

and:

```text
ALB
 |
 +---- Target Group
          |
          +---- EC2
          +---- EC2
```

---

# PART 16 — HEALTH CHECK

Enable:

```text
ELB health checks
```

Now Auto Scaling can understand whether instances are healthy.

Example:

```text
EC2 #1 → Healthy
EC2 #2 → Healthy
EC2 #3 → Unhealthy
```

The system can replace unhealthy instances.

This is one of the major reasons we combine:

> ALB + Target Group + Auto Scaling.

---

# PART 17 — CLOUDWATCH

Now we introduce monitoring.

Go to:

```text
CloudWatch
→ Metrics
→ EC2
```

You can see metrics such as:

```text
CPUUtilization
NetworkIn
NetworkOut
```

For this lab, our main metric is:

```text
CPUUtilization
```

---

# PART 18 — CREATE SCALING POLICY

Go back to:

```text
Auto Scaling Groups
→ webserver-asg
→ Automatic scaling
```

Create a scaling policy.

For a beginner-friendly lab, use:

```text
Target tracking scaling policy
```

Metric:

```text
Average CPU utilization
```

Target:

```text
60%
```

Now AWS's goal is roughly:

```text
Keep average CPU around 60%
```

If CPU remains high, AWS can add instances.

If CPU remains low, AWS can reduce instances, subject to the minimum capacity.

---

# PART 19 — UNDERSTAND SCALE OUT

Suppose:

```text
EC2 #1 CPU = 75%
EC2 #2 CPU = 80%
```

Average:

```text
77.5%
```

Our target is:

```text
60%
```

AWS may decide:

> We need more capacity.

Then:

```text
EC2 #1
EC2 #2
EC2 #3
```

Now traffic can be distributed across three instances.

This is:

> **Scale Out**

---

# PART 20 — UNDERSTAND SCALE IN

Now suppose traffic decreases.

```text
EC2 #1 = 10%
EC2 #2 = 15%
EC2 #3 = 12%
```

AWS has more capacity than necessary.

It may reduce instances:

```text
EC2 #1
EC2 #2
```

The ASG will respect:

```text
Minimum = 2
```

This is:

> **Scale In**

---

# PART 21 — GENERATE CPU LOAD

Now comes the fun part.

We need to make the CPU busy.

Connect to one of your Linux EC2 instances using SSH.

Run:

```bash
top
```

You can monitor CPU usage.

For a temporary lab test, you can generate CPU load with:

```bash
yes > /dev/null &
```

Check:

```bash
top
```

You should see CPU utilization increase.

Stop the process afterward:

```bash
pkill yes
```

### Important

Don't leave CPU stress running indefinitely.

It's a lab experiment, not a new lifestyle choice. 😄

---

# PART 22 — WATCH CLOUDWATCH

Go to:

```text
CloudWatch
→ Metrics
→ EC2
```

Watch:

```text
CPUUtilization
```

You should see something similar to:

```text
CPU %
100 |              ███
 80 |           ██████
 60 |       █████████
 40 |    █████
 20 | ███
    +-------------------
       Time
```

---

# PART 23 — WATCH AUTO SCALING

Go to:

```text
EC2
→ Auto Scaling Groups
→ webserver-asg
```

Look at:

```text
Instances
```

Initially:

```text
2
```

After scaling:

```text
3
```

Potentially:

```text
4
```

depending on the policy and observed metrics.

---

# PART 24 — WATCH TARGET GROUP

Go to:

```text
Target Groups
→ webserver-target-group
→ Targets
```

You should eventually see:

```text
EC2-1    Healthy
EC2-2    Healthy
EC2-3    Healthy
```

Now the architecture is truly working.

```text
                         USER
                           |
                           v
                          ALB
                           |
                  ┌────────┴────────┐
                  |                 |
                  v                 v
               Target Group
                  |
        ┌─────────┼─────────┐
        v         v         v
      EC2-1     EC2-2     EC2-3
        ^         ^         ^
        |         |         |
        └─────────┼─────────┘
                  |
             Auto Scaling
                  ^
                  |
             CloudWatch
```

---

# PART 25 — TEST LOAD BALANCING

Modify the HTML page so each server displays its hostname.

Our page already has:

```html
<p>Server: HOSTNAME</p>
```

Refresh the ALB URL multiple times.

You may see:

```text
Server: ip-10-0-1-25
```

and later:

```text
Server: ip-10-0-2-31
```

This demonstrates that requests can reach different backend instances.

---

# PART 26 — FAILURE TEST

Now let's test self-healing.

Take one instance and stop it:

```text
EC2
→ Instance
→ Stop instance
```

Watch the Target Group.

You should eventually see the instance become:

```text
Unhealthy
```

The Load Balancer should stop routing traffic to it.

The Auto Scaling Group can launch a replacement instance to maintain desired capacity.

Expected:

```text
Before

ALB
 |
 +--- EC2 #1 ✓
 |
 +--- EC2 #2 ✓


Stop EC2 #1

ALB
 |
 +--- EC2 #1 ✗
 |
 +--- EC2 #2 ✓


Auto Scaling

ALB
 |
 +--- EC2 #2 ✓
 |
 +--- EC2 #3 ✓
```

This is the real power of the architecture.

---

# PART 27 — WHAT HAPPENS INTERNALLY?

Let's simplify the entire chain.

### User sends request

```text
Browser
   |
   v
ALB
```

### ALB checks targets

```text
Target Group
 |
 +--- EC2 #1 ✓
 |
 +--- EC2 #2 ✓
```

### ALB forwards request

```text
ALB
 |
 +----> EC2 #1
```

### CloudWatch monitors

```text
EC2
 |
CPU = 78%
 |
v
CloudWatch
```

### Scaling policy reacts

```text
CPU > target
     |
     v
Auto Scaling
     |
     v
Launch EC2
```

### New instance joins target group

```text
ALB
 |
 +---- EC2 #1
 |
 +---- EC2 #2
 |
 +---- EC2 #3
```

That's the entire concept.

---

# PART 28 — WHY DO WE USE EACH SERVICE?

## EC2

**What?**

Virtual server.

**Why?**

To run our application.

```text
Application
     |
     v
    EC2
```

---

## ALB

**What?**

Application Load Balancer.

**Why?**

Distributes incoming HTTP/HTTPS traffic.

```text
Users
 |
 v
ALB
 |
 +--- EC2
 +--- EC2
```

---

## Target Group

**What?**

Collection of backend targets.

**Why?**

ALB needs to know where to send traffic.

```text
Target Group

EC2
EC2
EC2
```

---

## CloudWatch

**What?**

AWS monitoring service.

**Why?**

To observe system metrics and trigger actions through alarms/scaling mechanisms.

```text
EC2
 |
 v
CloudWatch
 |
 CPU
 Memory-related metrics where available
 Network
```

---

## Auto Scaling

**What?**

Automatic management of EC2 capacity.

**Why?**

To automatically increase/decrease capacity.

```text
High demand
    ↓
Add servers

Low demand
    ↓
Remove servers
```

---

## AMI

**What?**

Machine image/template.

**Why?**

To launch consistently configured EC2 instances.

```text
AMI
 |
 +---- EC2
 +---- EC2
 +---- EC2
```

---

## Launch Template

**What?**

Configuration used to launch EC2 instances.

**Why?**

Auto Scaling needs a standard configuration for new instances.

---

# PART 29 — FINAL ARCHITECTURE

Your final architecture should be:

```text
                         INTERNET
                            |
                            |
                            v
                  ┌─────────────────┐
                  │       ALB       │
                  │ Application LB  │
                  └────────┬────────┘
                           |
                           v
                  ┌─────────────────┐
                  │  Target Group   │
                  └────────┬────────┘
                           |
            ┌──────────────┼──────────────┐
            |              |              |
            v              v              v
       ┌─────────┐    ┌─────────┐    ┌─────────┐
       │  EC2 #1 │    │  EC2 #2 │    │  EC2 #3 │
       │   AZ-A  │    │   AZ-B  │    │   AZ-A  │
       └─────────┘    └─────────┘    └─────────┘
            ^              ^              ^
            |              |              |
            └──────────────┼──────────────┘
                           |
                    Auto Scaling Group
                           |
                           v
                    CloudWatch Metrics
                           |
                           v
                    Scaling Policy
                     /           \
                    /             \
               Scale Out       Scale In
```

---

# PART 30 — PRACTICAL CHALLENGE

Once you complete the basic lab, perform these tasks yourself.

### Task 1

Change:

```text
Minimum = 2
Desired = 2
Maximum = 5
```

---

### Task 2

Change target CPU:

```text
60%
```

to:

```text
50%
```

Observe what changes.

---

### Task 3

Create a CloudWatch alarm for:

```text
CPU > 70%
```

---

### Task 4

Create another alarm for:

```text
CPU < 20%
```

---

### Task 5

Terminate one EC2 instance.

Observe:

```text
Target Group
Auto Scaling Group
CloudWatch
```

---

### Task 6

Generate CPU load.

Observe:

```text
CPU
   ↓
CloudWatch
   ↓
Scaling
   ↓
New EC2
   ↓
Target Group
   ↓
ALB
```

---

### Task 7

Stop generating load.

Wait for the system to stabilize.

Observe whether capacity scales back toward the desired/minimum level.

---

# PART 31 — TROUBLESHOOTING

## Problem: ALB shows 503

Check:

```text
Target Group
→ Targets
```

If:

```text
Unhealthy
```

check:

```text
Security Group
Port 80
Apache
Health check path
```

---

## Problem: Website doesn't open

Check:

```text
systemctl status httpd
```

If stopped:

```bash
sudo systemctl start httpd
```

Check:

```bash
sudo ss -lntp
```

You should see HTTP listening on port 80.

---

## Problem: Target unhealthy

Check:

```bash
curl http://localhost
```

If this doesn't work, your web server isn't working correctly.

Check:

```bash
sudo systemctl status httpd
```

---

## Problem: Auto Scaling isn't creating instances

Check:

```text
Minimum
Desired
Maximum

Scaling policy

CloudWatch metrics

Launch Template

Subnets

Security Group
```

Also remember:

> Scaling isn't necessarily instantaneous. AWS needs to observe metrics and perform the scaling action.

---

# PART 32 — COST WARNING

**Very important.**

AWS resources can cost money.

At the end of the lab, don't simply close your browser and walk away like the AWS bill doesn't exist. 😄

Clean up resources you created.

Check and remove:

```text
Auto Scaling Group
Load Balancer
Target Group
Launch Template
AMI
EBS snapshots
Unused EC2 instances
Unused security groups
CloudWatch alarms
```

Be especially careful with:

- Load Balancers
- NAT Gateways
- EBS volumes/snapshots
- Public IPv4 addresses
- EC2 instances

Some AWS resources can incur charges even when you're not actively using the website.

---

# FINAL LEARNING CHECKLIST

After completing this lab, you should be able to explain:

- [ ] What is EC2?
- [ ] What is a Security Group?
- [ ] What is HTTP port 80?
- [ ] What is User Data?
- [ ] What is an AMI?
- [ ] What is a Launch Template?
- [ ] What is a Target Group?
- [ ] What is an ALB?
- [ ] What is a health check?
- [ ] What is CloudWatch?
- [ ] What is CPU utilization?
- [ ] What is an Auto Scaling Group?
- [ ] What is minimum capacity?
- [ ] What is desired capacity?
- [ ] What is maximum capacity?
- [ ] What is Scale Out?
- [ ] What is Scale In?
- [ ] What is High Availability?
- [ ] Why use multiple Availability Zones?
- [ ] How does ALB communicate with EC2?
- [ ] How does Auto Scaling work with an ALB?
- [ ] How does CloudWatch participate in scaling?

### The one-line mental model

Remember this:

```text
EC2
= Server

ALB
= Traffic Distributor

Target Group
= Backend Server List

CloudWatch
= Monitoring

Auto Scaling
= Automatically Add/Remove Servers

AMI
= Server Template

Launch Template
= Instructions for Creating Servers
```

And the whole system:

```text
                    USER
                      |
                      v
                    ALB
                      |
             ┌────────┴────────┐
             v                 v
           EC2-1             EC2-2
             ^                 ^
             |                 |
             └────────┬────────┘
                      |
                 Auto Scaling
                      ^
                      |
                 CloudWatch
                      |
              CPU / Application
                  Demand
```
