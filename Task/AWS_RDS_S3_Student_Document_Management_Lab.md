# AWS Practical Task: Build an Application Using Amazon RDS and Amazon S3

## 1. Task Overview

In this practical task, you will build a simple application architecture using:

- **Amazon RDS** — to store structured application data.
- **Amazon S3** — to store files/images/documents.
- **EC2** — optional application server from which you will connect to RDS and S3.
- **IAM** — to securely control access to S3.
- **AWS CLI** — to test S3 operations from a Linux server.

### Real-world scenario

Imagine you are building a **Student Document Management System**.

A student has:

- Name
- Email
- Course
- Profile/document file

We will store:

```text
Student information  ---> Amazon RDS
Student documents    ---> Amazon S3
Application          ---> EC2
Access permissions   ---> IAM
```

The important idea is:

> **RDS stores data. S3 stores files. They solve different problems.**

---

# 2. Learning Objectives

After completing this lab, you should understand:

1. What Amazon RDS is.
2. What Amazon S3 is.
3. Why RDS should not be used as a normal file-storage system.
4. How to create an RDS database.
5. How to create an S3 bucket.
6. How to upload files to S3.
7. How to create a database table.
8. How an application can connect RDS data with an S3 object.
9. Basic AWS security considerations.
10. How to clean up AWS resources after the lab.

---

# 3. Architecture

```text
                         AWS CLOUD
┌──────────────────────────────────────────────────────────┐
│                                                          │
│   ┌───────────────┐          ┌──────────────────────┐   │
│   │     User      │          │      Amazon S3       │   │
│   │ / Application │─────────▶│ Student Documents    │   │
│   └───────┬───────┘          └──────────────────────┘   │
│           │                                              │
│           │                                              │
│           ▼                                              │
│   ┌─────────────────┐                                    │
│   │       EC2       │                                    │
│   │ Application     │                                    │
│   │ Server          │                                    │
│   └────────┬────────┘                                    │
│            │                                             │
│            │ SQL                                        │
│            ▼                                             │
│   ┌─────────────────┐                                    │
│   │   Amazon RDS    │                                    │
│   │    MySQL        │                                    │
│   │                 │                                    │
│   │ students table  │                                    │
│   └─────────────────┘                                    │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

---

# 4. Important Concept Before Starting

## RDS vs S3

### Amazon RDS

RDS is a **managed relational database service**.

Example data:

```text
ID | Name       | Email              | Course
1  | Rahul      | rahul@example.com  | AWS
2  | Priya      | priya@example.com  | DevOps
```

This data needs:

- Tables
- Rows
- Columns
- SQL queries
- Relationships
- Transactions

Therefore, RDS is appropriate.

### Amazon S3

S3 is **object storage**.

It is suitable for:

- Images
- Videos
- PDFs
- ZIP files
- Backups
- Logs
- Documents
- Static website files

Example:

```text
s3://student-document-bucket/
    students/
        101/
            resume.pdf
            profile.jpg
        102/
            certificate.pdf
```

### Golden Rule

```text
Structured data  → RDS
Files/objects     → S3
```

Do not put large PDFs, images, or videos directly inside an RDS database just because you can. In most application architectures, the file goes to S3 and the database stores information such as its S3 key/path.

---

# 5. Prerequisites

You need:

- AWS account
- IAM permissions to create RDS, S3, EC2, and IAM resources
- Basic Linux knowledge
- Basic SQL knowledge
- Optional: an EC2 Linux instance
- AWS CLI installed on the EC2 instance

## Cost Warning

AWS resources can cost money.

For a learning lab:

- Use the smallest suitable RDS configuration.
- Do not enable unnecessary Multi-AZ or large storage.
- Do not leave resources running after the lab.
- Check the AWS pricing page for your region before creating resources.

---

# 6. Part A — Create an S3 Bucket

## Step 1: Open S3

AWS Console:

```text
AWS Console
→ S3
→ General purpose buckets
→ Create bucket
```

Choose a globally unique bucket name, for example:

```text
student-document-lab-<unique-number>
```

Do not copy this exact name if it is already taken.

---

## Step 2: Bucket Settings

For a beginner lab:

- Keep **Object Ownership** as recommended/default.
- Keep **Block Public Access** enabled.
- Keep versioning disabled unless you specifically want to practice it.
- Leave other settings at their safe defaults.

Click:

```text
Create bucket
```

---

# 7. Step 3 — Create an S3 Folder

Open your bucket.

Create:

```text
students/
```

Then create:

```text
students/101/
```

Your structure becomes:

```text
student-document-lab/
└── students/
    └── 101/
```

Remember:

> S3 does not have traditional directories in the same way Linux does. What looks like a folder is essentially part of an object's key.

For example:

```text
students/101/resume.pdf
```

is an S3 object key.

---

# 8. Step 4 — Upload a File

Create a test file on your computer:

```text
resume.pdf
```

Upload it into:

```text
students/101/
```

You should see:

```text
students/101/resume.pdf
```

---

# 9. S3 CLI Practice

If AWS CLI is configured on your Linux/EC2 machine:

## Check AWS CLI

```bash
aws --version
```

## Check identity

```bash
aws sts get-caller-identity
```

This helps confirm which AWS identity is being used.

## List buckets

```bash
aws s3 ls
```

## List objects

Replace the bucket name:

```bash
aws s3 ls s3://student-document-lab-<unique-number>/
```

## Upload a file

```bash
aws s3 cp resume.pdf \
s3://student-document-lab-<unique-number>/students/101/resume.pdf
```

## Download the file

```bash
aws s3 cp \
s3://student-document-lab-<unique-number>/students/101/resume.pdf \
downloaded-resume.pdf
```

## List the student folder

```bash
aws s3 ls \
s3://student-document-lab-<unique-number>/students/101/
```

---

# 10. Understanding the S3 Object Key

Suppose the object is:

```text
s3://student-document-lab-123456/students/101/resume.pdf
```

Break it down:

```text
s3://
│
└── student-document-lab-123456
    │
    └── students/101/resume.pdf
```

Where:

```text
Bucket = student-document-lab-123456

Object Key = students/101/resume.pdf
```

An application normally stores the object key in the database rather than storing the entire file inside the database.

---

# 11. Part B — Create Amazon RDS MySQL

## Step 1: Open RDS

Go to:

```text
AWS Console
→ RDS
→ Databases
→ Create database
```

Choose:

```text
Standard create
```

For learning, select:

```text
Engine:
MySQL
```

---

# 12. Database Configuration

Use a simple lab configuration.

Example:

```text
Engine: MySQL
Template: Dev/Test or Free Tier, if available
DB identifier: student-db
Master username: admin
```

Create a strong password.

### IMPORTANT

Do not put the real password inside:

- GitHub
- README files
- source code
- screenshots
- public chat messages

---

# 13. Instance Configuration

Choose a small instance appropriate for your AWS account and current pricing/free-tier eligibility.

For example, use a small burstable instance class if available.

Storage:

```text
General Purpose SSD
```

Use a small amount for this lab.

---

# 14. Connectivity

For a real production architecture:

```text
Internet
   |
   v
Application / Load Balancer
   |
   v
Private EC2
   |
   v
Private RDS
```

For this beginner lab, you may use a simpler setup if required by your environment.

### Recommended security approach

If connecting from EC2:

```text
RDS Security Group
Inbound:
MySQL/Aurora
Port: 3306
Source: EC2 Security Group
```

Avoid:

```text
0.0.0.0/0
```

for database access.

### Why?

Because this:

```text
0.0.0.0/0 → Port 3306
```

can expose your database to the internet.

A better design is:

```text
EC2 Security Group
       |
       | TCP 3306
       ▼
RDS Security Group
```

---

# 15. Create the RDS Database

Review the configuration.

Then click:

```text
Create database
```

Wait until the status becomes:

```text
Available
```

---

# 16. Find the RDS Endpoint

Open:

```text
RDS
→ Databases
→ student-db
```

Find:

```text
Endpoint
```

It will look similar to:

```text
student-db.xxxxxxxxxxxx.ap-south-1.rds.amazonaws.com
```

Do not copy the example above. Use the endpoint shown for your database.

---

# 17. Connect to MySQL

If you are connecting from an EC2 Linux server, install the MySQL client if needed.

For Amazon Linux:

```bash
sudo dnf install mariadb105 -y
```

Depending on the Amazon Linux version, the package name may differ.

Check:

```bash
mysql --version
```

Connect:

```bash
mysql -h YOUR-RDS-ENDPOINT \
-u admin \
-p
```

Example structure:

```bash
mysql -h student-db.xxxxxxxxxxxx.ap-south-1.rds.amazonaws.com \
-u admin \
-p
```

Enter the password when prompted.

---

# 18. Create the Database

Inside MySQL:

```sql
CREATE DATABASE student_portal;
```

Check:

```sql
SHOW DATABASES;
```

Use the database:

```sql
USE student_portal;
```

---

# 19. Create the Students Table

Run:

```sql
CREATE TABLE students (
    student_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(150) NOT NULL UNIQUE,
    course VARCHAR(100),
    document_key VARCHAR(500),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

Check:

```sql
SHOW TABLES;
```

---

# 20. Understand the Table

The table contains:

```text
student_id
name
email
course
document_key
created_at
```

The important field is:

```text
document_key
```

It can contain:

```text
students/101/resume.pdf
```

The actual PDF is stored in S3.

So:

```text
RDS
┌────────────────────────────────────────────┐
│ student_id │ name  │ document_key          │
├────────────┼───────┼───────────────────────┤
│ 101        │ Rahul │ students/101/resume.pdf│
└────────────────────────────────────────────┘
```

And S3 contains:

```text
Bucket
└── students/
    └── 101/
        └── resume.pdf
```

This is the key architecture you are learning.

---

# 21. Insert Student Data

Run:

```sql
INSERT INTO students
(name, email, course, document_key)
VALUES
(
    'Rahul Sharma',
    'rahul@example.com',
    'AWS DevOps',
    'students/101/resume.pdf'
);
```

Check:

```sql
SELECT * FROM students;
```

Expected conceptually:

```text
student_id | name          | email              | course     | document_key
-----------+---------------+--------------------+------------+----------------------------
1          | Rahul Sharma  | rahul@example.com   | AWS DevOps | students/101/resume.pdf
```

---

# 22. Add More Students

```sql
INSERT INTO students
(name, email, course, document_key)
VALUES
(
    'Priya Patil',
    'priya@example.com',
    'Cloud Computing',
    'students/102/certificate.pdf'
);

INSERT INTO students
(name, email, course, document_key)
VALUES
(
    'Amit Joshi',
    'amit@example.com',
    'DevOps',
    'students/103/resume.pdf'
);
```

Check:

```sql
SELECT * FROM students;
```

---

# 23. Test Queries

## Display all students

```sql
SELECT * FROM students;
```

## Find one student

```sql
SELECT *
FROM students
WHERE student_id = 1;
```

## Find students studying DevOps

```sql
SELECT *
FROM students
WHERE course = 'DevOps';
```

## Display only names and emails

```sql
SELECT name, email
FROM students;
```

---

# 24. The Complete Data Flow

Imagine Rahul uploads his resume.

### Step 1

Application receives:

```text
resume.pdf
```

### Step 2

Application uploads it to:

```text
S3
students/101/resume.pdf
```

### Step 3

Application stores metadata in RDS:

```text
student_id = 101
name = Rahul Sharma
email = rahul@example.com
course = AWS DevOps
document_key = students/101/resume.pdf
```

### Step 4

When Rahul wants to download his resume:

```text
Application
    |
    | Query RDS
    v
document_key
    |
    | Request S3 object
    v
S3
    |
    v
resume.pdf
```

---

# 25. Practical Challenge

Now perform this task without copying the solution.

## Requirement

Create a **Student Document Management System** using:

```text
RDS + S3
```

### Create 3 students:

```text
Student 1:
Name: Rahul Sharma
Course: AWS DevOps

Student 2:
Name: Priya Patil
Course: Cloud Computing

Student 3:
Name: Amit Joshi
Course: DevOps
```

### S3 objects

Create:

```text
students/101/resume.pdf
students/102/certificate.pdf
students/103/resume.pdf
```

### RDS records

Each student must have:

```text
student_id
name
email
course
document_key
created_at
```

The `document_key` must match the S3 object.

---

# 26. Validation Checklist

## S3

Run:

```bash
aws s3 ls s3://YOUR-BUCKET/students/101/
```

You should see:

```text
resume.pdf
```

For student 102:

```bash
aws s3 ls s3://YOUR-BUCKET/students/102/
```

For student 103:

```bash
aws s3 ls s3://YOUR-BUCKET/students/103/
```

---

## RDS

Run:

```sql
SELECT * FROM students;
```

You should have 3 records.

Then:

```sql
SELECT name, course, document_key
FROM students;
```

Verify that every `document_key` corresponds to an object in S3.

---

# 27. Bonus Task — Add File Metadata

Improve the table.

Create another table:

```sql
CREATE TABLE student_documents (
    document_id INT AUTO_INCREMENT PRIMARY KEY,
    student_id INT NOT NULL,
    document_name VARCHAR(255) NOT NULL,
    s3_bucket VARCHAR(255) NOT NULL,
    s3_key VARCHAR(500) NOT NULL,
    uploaded_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (student_id) REFERENCES students(student_id)
);
```

Insert:

```sql
INSERT INTO student_documents
(student_id, document_name, s3_bucket, s3_key)
VALUES
(
    1,
    'resume.pdf',
    'student-document-lab-123456',
    'students/101/resume.pdf'
);
```

Check:

```sql
SELECT * FROM student_documents;
```

---

# 28. Why Store the Bucket and Key?

Instead of storing:

```text
PDF FILE → RDS
```

store:

```text
Bucket + Object Key → RDS
```

Example:

```text
Bucket:
student-document-lab-123456

Key:
students/101/resume.pdf
```

Together they identify the object.

Conceptually:

```text
S3 Location
=
Bucket + Key
```

---

# 29. Security Notes

## Never make the database public unnecessarily

Avoid:

```text
RDS Port 3306
Source: 0.0.0.0/0
```

Prefer:

```text
EC2 Security Group
        ↓
RDS Security Group
```

---

## Never expose AWS credentials

Never put:

```text
AWS_ACCESS_KEY_ID=...
AWS_SECRET_ACCESS_KEY=...
```

inside GitHub repositories.

For EC2 applications, prefer an **IAM role attached to the EC2 instance**.

The application can then access AWS services without hardcoding long-term credentials.

---

# 30. S3 Public Access

For this lab, keep:

```text
Block Public Access = ON
```

Do not make your student documents publicly readable.

In a real application, the backend can generate controlled access such as a **pre-signed URL** when a user needs to download a private object.

---

# 31. Important Difference: Security Groups vs IAM

This is a common beginner confusion.

### Security Group

Controls network traffic.

Example:

```text
Who can connect to RDS port 3306?
```

### IAM

Controls AWS API permissions.

Example:

```text
Can this EC2 application upload objects to S3?
```

Think:

```text
Security Group → Network access

IAM            → AWS resource/API permissions
```

---

# 32. Troubleshooting

## Problem 1 — Cannot connect to RDS

Check:

```text
RDS status = Available
```

Check:

```text
Endpoint is correct
Username is correct
Password is correct
Port = 3306
```

Check the RDS Security Group.

---

## Problem 2 — Connection times out

Usually check:

```text
Security Group
Subnet routing
Network ACL
RDS accessibility
```

For an EC2-to-RDS setup, the RDS security group should allow MySQL traffic from the EC2 security group.

---

## Problem 3 — AccessDenied from S3

Check IAM permissions.

The role/user may need appropriate permissions such as:

```text
s3:ListBucket
s3:GetObject
s3:PutObject
```

Do not blindly attach `AdministratorAccess`.

Follow least privilege.

---

## Problem 4 — Bucket name already exists

S3 bucket names are globally unique.

Change:

```text
student-document-lab
```

to something unique:

```text
student-document-lab-2026-<unique-value>
```

---

# 33. Interview Questions

## Q1. What is Amazon RDS?

Amazon RDS is a managed relational database service that makes it easier to set up, operate, and scale relational databases.

---

## Q2. What is Amazon S3?

Amazon S3 is an object storage service designed to store and retrieve objects such as files, images, videos, backups, and documents.

---

## Q3. Why use S3 instead of RDS for PDFs?

S3 is designed for object/file storage, while RDS is designed for relational structured data.

---

## Q4. What should RDS store about an S3 file?

Typically metadata and the S3 location/key, for example:

```text
s3_bucket = student-document-lab
s3_key = students/101/resume.pdf
```

---

## Q5. What is an S3 object key?

It is the name/path-like identifier of an object within an S3 bucket.

Example:

```text
students/101/resume.pdf
```

---

## Q6. What is the difference between IAM and Security Groups?

```text
IAM:
Controls AWS API/resource permissions.

Security Group:
Controls network traffic to resources.
```

---

## Q7. Should RDS normally be publicly accessible?

No. In a production architecture, RDS is generally kept private and accessed by application resources through controlled networking.

---

# 34. Production-Level Architecture

After learning the basic version, think about the architecture like this:

```text
                     Internet
                         |
                         v
                  ┌─────────────┐
                  │     ALB     │
                  └──────┬──────┘
                         |
                         v
                ┌─────────────────┐
                │   EC2 / ECS     │
                │   Application   │
                └───────┬─────────┘
                        |
             ┌──────────┴───────────┐
             |                      |
             v                      v
      ┌──────────────┐       ┌──────────────┐
      │    RDS       │       │      S3      │
      │ MySQL/Postgres│       │ Files/Images │
      └──────────────┘       └──────────────┘
```

Security:

```text
Internet
   |
   v
ALB
   |
   v
Application
   |
   +------→ RDS
   |
   +------→ S3
```

RDS should not need to be directly exposed to the public internet.

---

# 35. Final Architecture Summary

The main lesson is:

```text
                 Student Application
                         |
             ┌───────────┴───────────┐
             |                       |
             v                       v
          Amazon RDS              Amazon S3
             |                       |
             |                       |
      Structured data             Files
             |                       |
             v                       v
       Student record          resume.pdf
       course                  certificate.pdf
       email                   profile.jpg
       S3 object key
```

### Remember this sentence

> **RDS remembers the data about the file; S3 remembers the file itself.**

That distinction is one of the most important concepts in beginner AWS application architecture.

---

# 36. Cleanup — VERY IMPORTANT

When the lab is finished, delete resources you no longer need.

## S3

First delete the objects:

```bash
aws s3 rm s3://YOUR-BUCKET --recursive
```

Then delete the bucket from the AWS Console.

## RDS

Delete:

```text
RDS
→ Databases
→ student-db
→ Actions
→ Delete
```

For a temporary lab, avoid keeping unnecessary automated backups if you do not need them.

## EC2

If you created an EC2 instance only for this lab, terminate it when finished.

## Other resources

Check for:

```text
Elastic IPs
NAT Gateways
Load Balancers
Snapshots
CloudWatch resources
```

These can potentially generate charges.

---

# 37. Final Student Assignment

## Task

Build the following architecture:

```text
Student Management System

RDS:
    Store student information.

S3:
    Store student documents.

IAM:
    Provide controlled S3 access.

EC2:
    Optional application server.
```

### Deliverables

Submit:

1. Screenshot of S3 bucket.
2. Screenshot of uploaded objects.
3. Screenshot of RDS database.
4. Screenshot of `students` table.
5. Output of:

```sql
SELECT * FROM students;
```

6. Output of:

```bash
aws s3 ls s3://YOUR-BUCKET/students/ --recursive
```

7. Architecture diagram.
8. Short explanation of why RDS and S3 are used together.

---

# 38. Knowledge Check

Answer these without looking back:

1. What is the difference between RDS and S3?
2. Where would you store a PDF?
3. Where would you store a student's email?
4. What is an S3 object key?
5. Why should RDS usually remain private?
6. What does a Security Group control?
7. What does IAM control?
8. Why should AWS credentials not be hardcoded?
9. What information can be stored in RDS to identify an S3 object?
10. What happens if you delete the S3 object but keep its database record?

---

# 39. Expected Final Result

Your final system should conceptually look like:

```text
RDS
┌──────────────────────────────────────────────┐
│ students                                     │
├────────┬──────────────┬────────────┬─────────┤
│ ID     │ Name         │ Course     │ S3 Key  │
├────────┼──────────────┼────────────┼─────────┤
│ 1      │ Rahul        │ AWS DevOps │ students│
│        │              │            │ /101/...│
├────────┼──────────────┼────────────┼─────────┤
│ 2      │ Priya        │ Cloud      │ students│
│        │              │ Computing  │ /102/...│
├────────┼──────────────┼────────────┼─────────┤
│ 3      │ Amit         │ DevOps     │ students│
│        │              │            │ /103/...│
└────────┴──────────────┴────────────┴─────────┘

                    |
                    | S3 Object Key
                    v

S3
┌───────────────────────────────────┐
│ student-document-lab              │
├───────────────────────────────────┤
│ students/101/resume.pdf           │
│ students/102/certificate.pdf      │
│ students/103/resume.pdf           │
└───────────────────────────────────┘
```

**Lab complete.**
