# AWS Lecture: AMI and EC2 Launch Template

**Level:** Beginner  
**Language:** Marathi + English  
**Topic:** Amazon Machine Image (AMI) and EC2 Launch Template

---

# 1. Introduction

In the previous lectures, we learned:

- What is Cloud Computing?
- Why do we need AWS?
- What is EC2?
- How to launch an EC2 instance?
- How to connect to an EC2 instance?
- How to install and configure software on EC2?

Now suppose we have configured one EC2 server properly.


For example:
```
EC2 Instance
    |
    ├── Amazon Linux
    ├── Nginx
    ├── Node.js
    ├── Git
    ├── Application
    └── Configuration
````

Now the company wants 5 more servers with exactly the same configuration.

Question:

> Do we need to configure every server manually?

Answer:

**No.**

This is where **AMI** and **Launch Template** are useful.

---

# 2. What is AMI?

## AMI = Amazon Machine Image

AMI is a pre-configured image that can be used to launch EC2 instances.

Simple definition:

> **AMI is a blueprint/image of a server that can be used to create new EC2 instances.**

Marathi:

> **AMI म्हणजे तयार केलेल्या server ची image किंवा blueprint. त्या image च्या आधारावर आपण नवीन EC2 instances तयार करू शकतो.**

---

# 3. Simple Real-Life Example

Imagine that you have a perfect recipe for making a cake.

Instead of remembering all the ingredients and steps every time, you save the recipe.

Now you can make:

```
Cake 1
Cake 2
Cake 3
Cake 4
Cake 5
```

using the same recipe.

Similarly:

```
Configured EC2
       |
       ↓
      AMI
       |
   ┌───┼───┐
   ↓   ↓   ↓
 EC2 EC2 EC2
```

AMI acts like a reusable server blueprint.

---

# 4. Why Do We Need AMI?

Suppose a company needs 20 web servers.

Without AMI:

```
Server 1 → Install OS → Nginx → Node.js → Configuration
Server 2 → Install OS → Nginx → Node.js → Configuration
Server 3 → Install OS → Nginx → Node.js → Configuration
...
Server 20 → Install OS → Nginx → Node.js → Configuration
```

This is:

* Time consuming
* Error prone
* Difficult to maintain
* Difficult to standardize

With AMI:

```
Configured Server
       |
       ↓
      AMI
       |
       ├── EC2
       ├── EC2
       ├── EC2
       ├── EC2
       └── EC2
```

Much faster and more consistent.

---

# 5. What Can an AMI Contain?

An AMI can contain the configuration required to launch an EC2 instance.

For example:

```
AMI
 |
 ├── Operating System
 |
 ├── Installed Software
 |      ├── Nginx
 |      ├── Node.js
 |      ├── Python
 |      └── Docker
 |
 ├── Application Files
 |
 └── System Configuration
```

Depending on how the image is created, application data and configurations can also be included.

---

# 6. Types of AMI

## 6.1 AWS Provided AMI

AWS provides ready-to-use AMIs.

Examples:

* Amazon Linux
* Ubuntu
* Windows Server
* Other supported operating system images

These are useful when creating a new EC2 instance.

---

## 6.2 Custom AMI

A Custom AMI is created by the user or organization.

Example:

```
Ubuntu
   +
Nginx
   +
Node.js
   +
Docker
   +
Application Dependencies
   ↓
Custom AMI
```

This custom AMI can then be reused to launch new EC2 instances.

---

# 7. EC2 vs AMI

This is a very important concept.

## EC2

EC2 is a running virtual server.

## AMI

AMI is an image/blueprint used to launch an EC2 instance.

Simple flow:

```
AMI
 |
 | Launch
 ↓
EC2 Instance
```

Remember:

> **AMI is not the running server.**

---

# 8. How to Create a Custom AMI?

Let's understand the practical workflow.

## Step 1: Launch an EC2 Instance

Go to:

```
AWS Console
   ↓
EC2
   ↓
Instances
   ↓
Launch Instance
```

Choose:

* AMI
* Instance Type
* Key Pair
* Security Group
* Storage

Then launch the instance.

---

# 9. Step 2: Connect to EC2

For Linux EC2:

```bash
ssh -i my-key.pem ec2-user@PUBLIC-IP
```

Or use:

```
EC2 Console → Connect → EC2 Instance Connect
```

depending on the instance configuration.

---

# 10. Step 3: Configure the Server

For example, install Nginx.

Amazon Linux example:

```bash
sudo dnf update -y
sudo dnf install nginx -y
```

Start Nginx:

```bash
sudo systemctl start nginx
```

Enable Nginx:

```bash
sudo systemctl enable nginx
```

Check status:

```bash
sudo systemctl status nginx
```

Now the EC2 server has been configured.

---

# 11. Step 4: Create AMI

Go to:

```
AWS Console
   ↓
EC2
   ↓
Instances
```

Select your configured EC2 instance.

Then:

```
Actions
   ↓
Image and templates
   ↓
Create image
```

Enter:

```
Image Name:
web-server-v1

Description:
Nginx configured web server
```

Then click:

```
Create image
```

AWS will create the custom AMI.

---

# 12. Step 5: Check the AMI

Go to:

```
EC2
   ↓
AMIs
```

You will see your custom AMI.

Example:

```
Name: web-server-v1
Status: Available
```

Once it is available, it can be used to launch new EC2 instances.

---

# 13. Launch EC2 From Custom AMI

Go to:

```
EC2
   ↓
AMIs
   ↓
Select web-server-v1
   ↓
Launch instance from AMI
```

Configure the required settings and launch.

Now:

```
Custom AMI
     |
     ├── EC2-1
     ├── EC2-2
     └── EC2-3
```

Each instance can start from the same base image.

---

# 14. Real Industry Use of AMI

AMI is widely useful in infrastructure automation and standardized server deployments.

Suppose a company has a web application.

They need:

```
100 EC2 instances
```

Instead of manually configuring every server:

```
Create standard configuration
          ↓
Create Custom AMI
          ↓
Use AMI to launch instances
```

This provides:

* Faster deployment
* Standardized configuration
* Reduced manual work
* Repeatability
* Easier scaling

---

# 15. What is a Golden AMI?

A **Golden AMI** is a standardized and approved AMI used by an organization.

Example:

```
Golden AMI
    |
    ├── Approved OS
    ├── Security Updates
    ├── Nginx
    ├── Monitoring Agent
    ├── Required Packages
    └── Organization Configuration
```

Organizations can use this as a standard starting point for EC2 instances.

---

# 16. What is an EC2 Launch Template?

Now we come to the second important topic.

## Launch Template

A Launch Template stores the configuration required to launch EC2 instances.

Simple definition:

> **Launch Template is a reusable configuration blueprint for launching EC2 instances.**

Marathi:

> **Launch Template मध्ये EC2 instance कसा launch करायचा याची configuration save केलेली असते.**

---

# 17. Why Do We Need Launch Templates?

Suppose every time you launch an EC2 you need to configure:

```
AMI
Instance Type
Key Pair
Security Group
Subnet
IAM Role
Storage
User Data
Network configuration
```

Doing this manually again and again is inefficient.

Instead:

```
Launch Template
       |
       ↓
Saved Configuration
       |
       ↓
Launch EC2
```

This makes the process repeatable.

---

# 18. What Does a Launch Template Contain?

A Launch Template can contain settings such as:

```
Launch Template
 |
 ├── AMI ID
 ├── Instance Type
 ├── Key Pair
 ├── Security Group
 ├── Network/Subnet
 ├── IAM Role
 ├── Storage Configuration
 ├── User Data
 └── Other EC2 Launch Settings
```

---

# 19. AMI vs Launch Template

This is one of the most important concepts.

## AMI

Answers:

> **What should be inside the machine?**

Example:

```
Ubuntu
Nginx
Node.js
Docker
Application dependencies
```

---

## Launch Template

Answers:

> **How should the EC2 instance be launched?**

Example:

```
AMI = web-server-v1
Instance Type = t3.micro
Security Group = web-sg
Subnet = subnet-123
Key Pair = my-key
IAM Role = EC2-role
Storage = 20 GB
```

---

# 20. Easy Memory Trick

Remember:

```
AMI = WHAT
Launch Template = HOW
```

### AMI

**WHAT is installed/configured in the machine?**

### Launch Template

**HOW should the machine be launched?**

---

# 21. Example

Suppose our application requires:

```
Ubuntu
Nginx
Node.js
Docker
```

We create:

```
Custom AMI
```

Then we create:

```
Launch Template
```

with:

```
AMI = Custom AMI
Instance Type = t3.micro
Security Group = Web-SG
Key Pair = Dev-Key
Subnet = Private Subnet
IAM Role = EC2-Role
Storage = 20 GB
```

Now the launch configuration is reusable.

---

# 22. How to Create Launch Template?

Go to:

```
AWS Console
   ↓
EC2
   ↓
Launch Templates
   ↓
Create launch template
```

Enter:

```
Launch Template Name:
web-server-template
```

Then configure:

### AMI

Select:

```
web-server-v1
```

### Instance Type

Example:

```
t3.micro
```

### Key Pair

Select required key pair.

### Security Group

Select the required security group.

### Storage

Configure required EBS volume.

### IAM Role

Attach the required IAM role if needed.

Then:

```
Create launch template
```

---

# 23. Launch Template Versions

Launch Templates support versions.

Example:

```
Version 1
    |
    ├── AMI v1
    └── t3.micro

Version 2
    |
    ├── AMI v2
    └── t3.small
```

This is useful when infrastructure configuration changes.

Instead of changing everything manually, we can create a new version.

---

# 24. What is User Data?

Launch Template can also contain **User Data**.

User Data is a script that can run when an EC2 instance starts.

Example:

```bash
#!/bin/bash

dnf update -y
dnf install nginx -y

systemctl start nginx
systemctl enable nginx
```

Concept:

```
EC2 Launch
     ↓
User Data Executes
     ↓
Install/Configure Software
     ↓
Application Starts
```

---

# 25. AMI + Launch Template

Now combine both concepts.

```
             Custom AMI
                  |
                  ↓
          Launch Template
                  |
                  ↓
             EC2 Instance
```

The AMI provides the machine image.

The Launch Template provides the launch configuration.

---

# 26. AMI + Launch Template + Auto Scaling

This is where the concept becomes very important for real-world infrastructure.

Architecture:

```
                    Users
                      |
                      ↓
                Load Balancer
                      |
                      ↓
             Auto Scaling Group
                      |
              Launch Template
                      |
                      ↓
                    AMI
                      |
          ┌───────────┼───────────┐
          ↓           ↓           ↓
        EC2-1       EC2-2       EC2-3
```

Flow:

```
AMI
 ↓
Launch Template
 ↓
Auto Scaling Group
 ↓
EC2 Instances
```

---

# 27. Real-World Example

Suppose an e-commerce website normally needs:

```
2 EC2 instances
```

During a sale, traffic increases.

The system may need:

```
5 EC2 instances
```

or more, depending on the scaling configuration.

Auto Scaling can launch new instances using the configured launch settings.

Conceptually:

```
Normal Traffic
     ↓
2 EC2

High Traffic
     ↓
More EC2 Instances
```

The new instances can use the standardized AMI and Launch Template configuration.

---

# 28. Why Companies Use AMI + Launch Template

Main benefits:

## 1. Standardization

All servers can start from a common approved configuration.

## 2. Automation

Less manual server configuration.

## 3. Faster Deployment

New EC2 instances can be launched quickly.

## 4. Repeatability

The same configuration can be reused.

## 5. Scaling

Useful with Auto Scaling.

## 6. Disaster Recovery

A maintained AMI can help recreate server environments.

## 7. Consistency

Different EC2 instances can use the same base configuration.

---

# 29. AMI and Launch Template in DevOps

In DevOps, automation and repeatability are very important.

Typical flow:

```
Developer
    ↓
Git
    ↓
CI/CD
    ↓
Build
    ↓
Test
    ↓
Create/Update AMI
    ↓
Launch Template Version
    ↓
Auto Scaling
    ↓
EC2
```

Depending on the organization's architecture, AMI creation and deployment can be automated using tools such as:

* Terraform
* Packer
* CI/CD pipelines
* AWS services

---

# 30. AMI vs Snapshot

Students often confuse AMI and EBS Snapshot.

## EBS Snapshot

Backup of an EBS volume.

## AMI

A machine image used to launch EC2 instances.

Simple:

```
EBS Snapshot
    ↓
Volume Backup
```

while:

```
AMI
    ↓
Launch EC2
```

An AMI can reference the necessary EBS snapshots for its block devices.

---

# 31. Important Security Point

When creating a Custom AMI, do not store sensitive information inside it.

Avoid:

```
Passwords
API Keys
Private Keys
Database Passwords
Access Tokens
Secrets
```

Instead use appropriate secret-management and IAM mechanisms.

For example:

```
AWS Secrets Manager
SSM Parameter Store
IAM Roles
```

---

# 32. Important AMI Best Practices

### 1. Keep AMIs updated

Regularly apply security updates.

### 2. Use meaningful names

Example:

```
web-server-ubuntu-v1
web-server-ubuntu-v2
```

### 3. Maintain versioning

Keep track of AMI versions.

### 4. Remove old unused AMIs

Old AMIs and their related snapshots can consume storage and incur costs.

### 5. Do not store secrets

Never bake passwords or API keys into images.

### 6. Test before production

Always test a new AMI before using it in production.

---

# 33. Important Launch Template Best Practices

### 1. Use meaningful names

```
production-web-template
staging-web-template
```

### 2. Use versions

```
Version 1
Version 2
Version 3
```

### 3. Use IAM Roles

Avoid putting AWS access keys directly on EC2 instances.

### 4. Use Security Groups properly

Only allow required traffic.

### 5. Review User Data

Make sure startup scripts are correct and secure.

---

# 34. Practical Lab

## Task 1: Create EC2

Launch an EC2 instance.

---

## Task 2: Install Nginx

```
sudo dnf update -y
sudo dnf install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
```

---

## Task 3: Verify Nginx

Open:

```
http://PUBLIC-IP
```

Make sure the Nginx page appears.

---

## Task 4: Create Custom AMI

Go to:

```
EC2
→ Instances
→ Select Instance
→ Actions
→ Image and templates
→ Create image
```

Name:

```
web-server-v1
```

---

## Task 5: Launch New EC2 From AMI

Go to:

```
EC2
→ AMIs
→ Select web-server-v1
→ Launch instance
```

Verify the new instance.

---

## Task 6: Create Launch Template

Go to:

```
EC2
→ Launch Templates
→ Create launch template
```

Configure:

```
Name:
web-server-template

AMI:
web-server-v1

Instance Type:
t3.micro

Security Group:
web-sg

Key Pair:
your-key
```

Create the template.

---

# 35. Practical Architecture

```
                CONFIGURED EC2
                     |
                     ↓
                CUSTOM AMI
                     |
                     ↓
             LAUNCH TEMPLATE
                     |
                     ↓
            AUTO SCALING GROUP
                     |
        ┌────────────┼────────────┐
        ↓            ↓            ↓
      EC2-1        EC2-2        EC2-3
        │            │            │
        └────────────┼────────────┘
                     ↓
              LOAD BALANCER
                     |
                     ↓
                   USERS
```

---

# 36. Interview Questions

## Q1. What is AMI?

AMI stands for Amazon Machine Image. It is a machine image used to launch EC2 instances.

---

## Q2. Why do we create Custom AMIs?

To create reusable and standardized EC2 configurations and reduce repeated manual setup.

---

## Q3. What is the difference between EC2 and AMI?

EC2 is a running virtual server, while AMI is an image used to launch EC2 instances.

---

## Q4. What is a Launch Template?

A Launch Template stores reusable EC2 launch configuration.

---

## Q5. What is the difference between AMI and Launch Template?

AMI defines the machine image, while Launch Template defines how the EC2 instance should be launched.

---

## Q6. What is a Golden AMI?

A Golden AMI is an organization's standardized and approved machine image.

---

## Q7. Can we launch multiple EC2 instances from one AMI?

Yes.

---

## Q8. What is User Data?

User Data is a startup script that can run when an EC2 instance launches.

---

## Q9. Why are Launch Templates useful with Auto Scaling?

They provide a reusable configuration that can be used when Auto Scaling launches new EC2 instances.

---

## Q10. Should passwords and API keys be stored inside AMIs?

No. Secrets should be managed using appropriate security mechanisms such as Secrets Manager or Parameter Store.

---

# 37. Quick Revision

Remember these four concepts:

```
AMI
 ↓
WHAT is inside the machine?

Launch Template
 ↓
HOW should the machine be launched?

Auto Scaling
 ↓
HOW MANY instances should run?

Load Balancer
 ↓
HOW should traffic be distributed?
```

---

# 38. Final Takeaway

The main purpose of AMI and Launch Templates is:

> **Create infrastructure that is reusable, consistent, automated, and scalable.**

### Remember:

```
AMI = Machine Image
Launch Template = Launch Configuration
Auto Scaling = Automatically manage instance count
Load Balancer = Distribute traffic
```

The complete concept:

```
          AMI
           ↓
    Launch Template
           ↓
    Auto Scaling Group
           ↓
      EC2 Instances
           ↓
     Load Balancer
           ↓
         Users
```

---

# 39. Homework

### Beginner Task

1. Launch an EC2 instance.
2. Install Nginx.
3. Create a Custom AMI.
4. Launch another EC2 using the Custom AMI.
5. Verify Nginx.
6. Create a Launch Template using the Custom AMI.
7. Compare the original EC2 and the new EC2.

### Questions to Answer

1. What is AMI?
2. Why do we need AMI?
3. What is Custom AMI?
4. What is Golden AMI?
5. What is Launch Template?
6. Why do we need Launch Templates?
7. Difference between AMI and Launch Template?
8. What is User Data?
9. How are AMI and Launch Template used with Auto Scaling?
10. Why should secrets not be stored inside an AMI?

---

# 40. One-Line Summary

> **AMI gives us the machine image, Launch Template gives us the launch configuration, Auto Scaling gives us scalability, and Load Balancer distributes the traffic.**

```
AMI
+
Launch Template
+
Auto Scaling
+
Load Balancer
=
Scalable AWS Infrastructure
```

```
```
