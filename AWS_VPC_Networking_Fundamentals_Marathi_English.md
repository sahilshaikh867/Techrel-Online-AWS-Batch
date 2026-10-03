# AWS VPC & Networking Fundamentals

## Complete Beginner-Friendly Lecture Notes

### Marathi + English Mix

> **Level:** Beginner\
> **Topic:** AWS VPC, Networking, Subnets, Internet Gateway, NAT
> Gateway, Route Tables, Security Groups, NACL, Public/Private
> Networking and related services\
> **Teaching Style:** Marathi + English\
> **Practical Goal:** Create a custom VPC, public/private subnets,
> routing, security controls and launch EC2.

------------------------------------------------------------------------

# 1. Lecture Overview

आजच्या lecture मध्ये आपण AWS networking अगदी **zero पासून** समजून घेणार आहोत.

आपण खालील concepts cover करणार आहोत:

1.  What is Networking?
2.  Why Networking is required in AWS
3.  What is VPC?
4.  Default VPC
5.  Custom VPC
6.  AWS Region
7.  Availability Zone
8.  IP Address
9.  Private IP
10. Public IP
11. CIDR
12. Subnet
13. Public Subnet
14. Private Subnet
15. Route Table
16. Internet Gateway
17. NAT Gateway
18. Elastic IP
19. Security Group
20. Network ACL
21. DNS and Route 53
22. VPC Peering
23. VPC Endpoints
24. Load Balancer basics
25. 3-Tier Architecture
26. Real Industry Architecture
27. Complete VPC Practical
28. Troubleshooting
29. Interview Questions
30. Revision

------------------------------------------------------------------------

# 2. Start With a Question

Class सुरू करण्यापूर्वी students ना विचार:

> **"आपण EC2 launch केला. पण तो internet शी communicate कसा करतो?"**

Another question:

> **"दोन EC2 instances एकमेकांशी communicate कसे करतात?"**

Another:

> **"आपला database internet वर public असायला हवा का?"**

याच प्रश्नांची answers समजण्यासाठी AWS Networking समजणे आवश्यक आहे.

------------------------------------------------------------------------

# 3. What is Networking?

Networking म्हणजे different computers, servers, applications किंवा devices
यांच्यामध्ये communication establish करणे.

Simple example:

``` text
Computer A
     |
     | Network
     |
Computer B
```

AWS मध्ये:

``` text
User
 |
Internet
 |
AWS
 |
EC2
 |
Database
```

म्हणजे data एका system मधून दुसऱ्या system कडे कसा जाईल हे networking ठरवते.

------------------------------------------------------------------------

# 4. Why Do We Need Networking in AWS?

समजा आपण एक e-commerce application तयार केली.

आपल्याकडे:

``` text
Users
Web Server
Application Server
Database
Storage
```

आपल्याला decide करावे लागेल:

-   कोणता server public असेल?
-   कोणता server private असेल?
-   कोणत्या server ला internet access असेल?
-   कोणता traffic allow करायचा?
-   कोणता traffic block करायचा?
-   database कुठे ठेवायचा?
-   traffic कोणत्या route ने जाईल?

हे सगळे networking concepts वापरून control केले जाते.

------------------------------------------------------------------------

# 5. What is VPC?

## VPC = Virtual Private Cloud

Simple definition:

> **VPC is a logically isolated virtual network in AWS where you can
> launch and control AWS resources.**

Marathi:

> **VPC म्हणजे AWS मध्ये आपल्यासाठी तयार केलेले logical/private network.**

VPC मध्ये आपण:

-   EC2
-   RDS
-   Load Balancer
-   Application servers
-   Internal services

इत्यादी resources organize करू शकतो.

------------------------------------------------------------------------

# 6. Real-Life Example of VPC

Company office imagine करा.

``` text
Company
 |
 +-- HR
 |
 +-- Finance
 |
 +-- Development
 |
 +-- Server Room
 |
 +-- Security
```

सगळे company च्या network मध्ये आहेत, पण प्रत्येकाला वेगवेगळा access असू शकतो.

AWS मध्ये:

``` text
AWS
 |
 +-- VPC
      |
      +-- Public Network
      |
      +-- Private Network
      |
      +-- Application Servers
      |
      +-- Database
```

VPC म्हणजे आपल्या cloud infrastructure साठी network boundary.

------------------------------------------------------------------------

# 7. Default VPC

AWS account मध्ये default VPC उपलब्ध असू शकते.

Default VPC मुळे beginners ना basic networking manually configure न करता
EC2 launch करणे सोपे होते.

Concept:

``` text
Default VPC
 |
 +-- Subnets
 |
 +-- Route Tables
 |
 +-- Internet Gateway
 |
 +-- Security Groups
```

### Important

Default VPC learning साठी useful आहे.

Production मध्ये organization च्या requirements नुसार custom networking
design केले जाऊ शकते.

------------------------------------------------------------------------

# 8. Custom VPC

Custom VPC म्हणजे आपण स्वतः design केलेली VPC.

Example:

``` text
VPC
10.0.0.0/16
 |
 +-- Public Subnet
 |
 +-- Private Subnet
 |
 +-- Database Subnet
```

Custom VPC मध्ये आपण networking requirements नुसार:

-   CIDR
-   Subnets
-   Route Tables
-   Internet Gateway
-   NAT Gateway
-   Security Controls

configure करू शकतो.

------------------------------------------------------------------------

# 9. AWS Region

Region म्हणजे AWS infrastructure चा geographic area.

Examples:

-   Mumbai
-   Singapore
-   Tokyo
-   London
-   Frankfurt
-   N. Virginia

Concept:

``` text
AWS
 |
 +-- Mumbai Region
 |
 +-- Singapore Region
 |
 +-- London Region
```

Region निवडताना विचारात घेतले जाणारे factors:

-   User location
-   Latency
-   Service availability
-   Compliance requirements
-   Cost

------------------------------------------------------------------------

# 10. Availability Zone

एका Region मध्ये multiple Availability Zones असतात.

Example:

``` text
Mumbai Region
 |
 +-- Availability Zone A
 |
 +-- Availability Zone B
 |
 +-- Availability Zone C
```

Availability Zones independently operated infrastructure locations आहेत.

Multiple AZs वापरल्यामुळे applications अधिक resilient architecture मध्ये
design करता येतात.

------------------------------------------------------------------------

# 11. Region vs Availability Zone

  -----------------------------------------------------------------------
  Region                              Availability Zone
  ----------------------------------- -----------------------------------
  Geographic AWS location             Region मधील isolated infrastructure
                                      location

  Example: Mumbai                     Mumbai Region मधील AZ

  Contains multiple AZs               Belongs to one Region
  -----------------------------------------------------------------------

### Memory Trick

> **Region = Geographic Area**

> **AZ = Region मधील infrastructure location**

------------------------------------------------------------------------

# 12. IP Address

Network मध्ये प्रत्येक resource ला identify करण्यासाठी IP addressing वापरले
जाते.

Example:

``` text
10.0.1.10
```

AWS networking मध्ये mainly:

-   Private IPv4
-   Public IPv4
-   Elastic IP
-   IPv6

यांचा वापर होऊ शकतो.

------------------------------------------------------------------------

# 13. Private IP

Private IP VPC/network मध्ये communication साठी वापरला जातो.

Example:

``` text
EC2-1
Private IP: 10.0.1.10

EC2-2
Private IP: 10.0.1.20
```

दोन्ही resources VPC networking वापरून communicate करू शकतात, subject to
routing and security rules.

------------------------------------------------------------------------

# 14. Public IP

Public IP internet-facing connectivity साठी वापरला जाऊ शकतो.

Example:

``` text
Internet
   |
Public IP
   |
EC2
```

Public IP वापरण्यासाठी networking आणि security configuration योग्य असणे
आवश्यक आहे.

------------------------------------------------------------------------

# 15. Private IP vs Public IP

  -----------------------------------------------------------------------
  Private IP                          Public IP
  ----------------------------------- -----------------------------------
  Internal VPC communication          Internet-facing connectivity

  VPC/network scope                   Public internet scope

  Example: 10.0.1.10                  Public IPv4 address

  Commonly used between internal      Used when direct public
  resources                           connectivity is required
  -----------------------------------------------------------------------

------------------------------------------------------------------------

# 16. CIDR

CIDR = Classless Inter-Domain Routing.

CIDR network address range define करण्यासाठी वापरले जाते.

Example:

``` text
10.0.0.0/16
```

हे VPC साठी network range define करू शकते.

Subnets:

``` text
10.0.1.0/24
10.0.2.0/24
10.0.3.0/24
```

------------------------------------------------------------------------

# 17. CIDR Simple Explanation

Suppose:

``` text
VPC
10.0.0.0/16
```

त्याच्या आत आपण smaller networks create करू शकतो:

``` text
10.0.1.0/24
10.0.2.0/24
10.0.3.0/24
```

Concept:

``` text
             VPC
        10.0.0.0/16
              |
       +------+------+
       |      |      |
       v      v      v
    Subnet  Subnet  Subnet
     /24     /24     /24
```

------------------------------------------------------------------------

# 18. What is a Subnet?

Subnet = Sub-network.

> **A subnet is a logical subdivision of a VPC IP range.**

Marathi:

> **VPC हा मोठा network आहे आणि subnet म्हणजे त्या VPC मधील smaller network
> segment.**

Example:

``` text
VPC
10.0.0.0/16
 |
 +-- 10.0.1.0/24
 |
 +-- 10.0.2.0/24
 |
 +-- 10.0.3.0/24
```

------------------------------------------------------------------------

# 19. Why Do We Need Subnets?

Subnets वापरून आपण resources logically separate करू शकतो.

Example:

``` text
VPC
 |
 +-- Public Subnet
 |      |
 |      +-- Load Balancer
 |
 +-- Private App Subnet
 |      |
 |      +-- Application Servers
 |
 +-- Private DB Subnet
        |
        +-- Database
```

यामुळे architecture अधिक organized आणि controlled बनते.

------------------------------------------------------------------------

# 20. Public Subnet

Public subnet हा असा subnet आहे ज्याच्या route table मध्ये internet gateway
कडे route असू शकतो.

Typical flow:

``` text
Internet
   |
   v
Internet Gateway
   |
   v
Public Subnet
   |
   v
Public-facing Resource
```

### Important

फक्त subnet ला "public" नाव दिल्याने तो public होत नाही.

Public connectivity साठी:

-   Route to Internet Gateway
-   Appropriate public IP
-   Security rules
-   Correct network configuration

आवश्यक असते.

------------------------------------------------------------------------

# 21. Private Subnet

Private subnet मध्ये Internet Gateway कडे direct route नसतो.

Example:

``` text
Private Subnet
 |
 +-- Application Server
 |
 +-- Database
```

जर private resource ला outbound internet access आवश्यक असेल तर NAT Gateway
सारखा component वापरता येतो.

------------------------------------------------------------------------

# 22. Public vs Private Subnet

  Public Subnet                     Private Subnet
  --------------------------------- -------------------------------
  Route to IGW possible             No direct route to IGW
  Public-facing architecture साठी   Internal resources साठी
  Load Balancer सारखे resources      Application/DB resources
  Public IP आवश्यक असू शकतो           Public IP सामान्यतः टाळला जातो

------------------------------------------------------------------------

# 23. Internet Gateway

## IGW = Internet Gateway

Internet Gateway VPC ला internet शी connect करण्यासाठी वापरला जातो.

Basic concept:

``` text
Internet
    |
    v
Internet Gateway
    |
    v
VPC
```

Internet Gateway VPC ला attach केला जातो.

------------------------------------------------------------------------

# 24. Internet Gateway Alone Is Not Enough

Important concept:

> **Internet Gateway attach केला म्हणून EC2 automatically public होत
> नाही.**

योग्य configuration आवश्यक:

``` text
VPC
 |
Subnet
 |
Route Table
 |
0.0.0.0/0 -> IGW
 |
EC2
 |
Public IP
 |
Security Group
```

सगळ्या layers योग्य असल्या पाहिजेत.

------------------------------------------------------------------------

# 25. Route Table

Route Table traffic कुठे जायचा हे define करते.

Example:

``` text
Destination        Target

10.0.0.0/16        local
0.0.0.0/0          Internet Gateway
```

Meaning:

``` text
10.0.0.0/16
     |
     +-- Local VPC traffic

0.0.0.0/0
     |
     +-- Internet Gateway
```

------------------------------------------------------------------------

# 26. What is 0.0.0.0/0?

`0.0.0.0/0` म्हणजे IPv4 साठी all destinations चा route.

Example:

``` text
0.0.0.0/0 -> Internet Gateway
```

म्हणजे VPC च्या local range बाहेरील traffic Internet Gateway कडे जाऊ शकतो,
subject to other network and security configuration.

------------------------------------------------------------------------

# 27. Local Route

VPC route table मध्ये VPC CIDR साठी local route असतो.

Example:

``` text
VPC CIDR = 10.0.0.0/16

Destination:
10.0.0.0/16

Target:
local
```

यामुळे VPC मधील subnets/resources मध्ये private routing शक्य होते, subject to
security controls.

------------------------------------------------------------------------

# 28. NAT Gateway

## NAT = Network Address Translation

NAT Gateway private subnet मधील resources ना outbound internet access
देण्यासाठी वापरता येतो.

Architecture:

``` text
Private EC2
     |
     v
NAT Gateway
     |
     v
Internet Gateway
     |
     v
Internet
```

### Important

NAT Gateway चा common use:

> **Private resources → outbound Internet**

It is not a replacement for an Internet Gateway.

------------------------------------------------------------------------

# 29. Why Do We Need NAT Gateway?

Suppose application server private subnet मध्ये आहे.

त्याला:

-   OS updates
-   Package downloads
-   External APIs

access करायचे आहेत.

पण server public internet वर directly exposed ठेवायचा नाही.

Architecture:

``` text
Private EC2
     |
     v
NAT Gateway
     |
     v
Internet
```

यामुळे private resource ला outbound connectivity मिळू शकते.

------------------------------------------------------------------------

# 30. NAT Gateway vs Internet Gateway

  -----------------------------------------------------------------------
  Internet Gateway                    NAT Gateway
  ----------------------------------- -----------------------------------
  VPC ↔ Internet connectivity         Private subnet outbound
                                      connectivity

  Public-facing architecture मध्ये      Private resources साठी
  important                           

  VPC ला attach केला जातो              Subnet route table मध्ये target म्हणून
                                      वापरला जातो

  Public IPv4 connectivity            Private resources ना outbound
  architecture मध्ये वापरला जातो        internet देतो
  -----------------------------------------------------------------------

------------------------------------------------------------------------

# 31. Elastic IP

Elastic IP एक static public IPv4 address आहे जो AWS account मध्ये allocate
करून supported resource ला associate करता येतो.

Use cases:

-   Stable public endpoint
-   Specific infrastructure requirement

### Important

Public IPv4 addresses and Elastic IP usage can incur charges, so
unnecessary allocation टाळा.

------------------------------------------------------------------------

# 32. Security Group

Security Group हा virtual firewall आहे.

तो resources such as EC2 च्या network traffic ला control करण्यासाठी वापरला
जातो.

Controls:

-   Inbound traffic
-   Outbound traffic

Example:

``` text
Security Group
 |
 +-- HTTP 80
 |
 +-- HTTPS 443
 |
 +-- SSH 22
```

------------------------------------------------------------------------

# 33. Security Group Example

Web server:

``` text
HTTP
Port: 80
Source: 0.0.0.0/0
```

HTTPS:

``` text
HTTPS
Port: 443
Source: 0.0.0.0/0
```

SSH:

``` text
SSH
Port: 22
Source: Your Trusted IP
```

### Best Practice

SSH ला unnecessarily:

``` text
0.0.0.0/0
```

open करू नका.

------------------------------------------------------------------------

# 34. Security Group Features

Security Group:

-   Stateful आहे
-   Resource/network interface level वर लागू होतो
-   Allow rules वापरतो
-   Inbound/outbound traffic control करतो

Stateful म्हणजे return traffic साठी separate reverse rule manually define
करण्याची गरज सामान्यतः नसते.

------------------------------------------------------------------------

# 35. Network ACL

## NACL = Network Access Control List

NACL हा subnet level network access control mechanism आहे.

Concept:

``` text
VPC
 |
Subnet
 |
NACL
 |
Resources
```

NACL:

-   Subnet level वर associated असतो
-   Stateless असतो
-   Allow आणि Deny rules support करतो
-   Rule numbers वापरतो

------------------------------------------------------------------------

# 36. Security Group vs NACL

  -----------------------------------------------------------------------
  Security Group                      NACL
  ----------------------------------- -----------------------------------
  Resource/network interface level    Subnet level

  Stateful                            Stateless

  Allow rules                         Allow + Deny

  EC2/network interface सारख्या        Subnet सोबत
  resources सोबत                      

  Common first layer for EC2 access   Additional subnet-level control
  control                             
  -----------------------------------------------------------------------

### Memory Trick

> **Security Group = Resource-level firewall**

> **NACL = Subnet-level network control**

------------------------------------------------------------------------

# 37. VPC DNS

AWS VPC मध्ये DNS support available असते.

DNS चा उपयोग domain name ला IP address शी resolve करण्यासाठी होतो.

Example:

``` text
example.com
     |
     v
IP Address
```

VPC मध्ये DNS settings resources च्या name resolution साठी important असतात.

------------------------------------------------------------------------

# 38. Route 53

Amazon Route 53 is AWS's DNS service.

Use cases:

-   Domain name management
-   DNS records
-   Routing policies
-   Health checks
-   Domain registration features

Simple:

``` text
User
 |
example.com
 |
Route 53
 |
AWS Resource
```

------------------------------------------------------------------------

# 39. VPC Peering

VPC Peering दोन VPCs मध्ये private network connectivity enable करण्यासाठी
वापरले जाऊ शकते.

Example:

``` text
VPC-A
10.0.0.0/16
    |
    |
 VPC Peering
    |
    |
VPC-B
10.1.0.0/16
```

Resources private IPs वापरून communicate करू शकतात, subject to routes and
security rules.

### Important

CIDR ranges overlap नसणे सामान्य requirement आहे.

------------------------------------------------------------------------

# 40. VPC Endpoint

VPC Endpoint च्या मदतीने VPC मधून supported AWS services कडे traffic public
internet route न वापरता private connectivity ने पाठवता येतो.

Example:

``` text
Private EC2
    |
    v
VPC Endpoint
    |
    v
AWS Service
```

Common example:

``` text
Private EC2
    |
    v
S3
```

VPC Endpoint architecture मुळे NAT Gateway dependency काही use cases मध्ये
कमी करता येऊ शकते.

------------------------------------------------------------------------

# 41. Load Balancer Basics

AWS Elastic Load Balancing incoming traffic multiple targets मध्ये
distribute करण्यासाठी वापरले जाते.

Example:

``` text
Users
  |
  v
Load Balancer
  |
  +------+
  |      |
  v      v
 EC2    EC2
```

Benefits:

-   Traffic distribution
-   High availability architecture
-   Scaling support
-   Health checks

------------------------------------------------------------------------

# 42. Application Load Balancer

## ALB

Application Load Balancer HTTP/HTTPS applications साठी commonly वापरला
जातो.

Example:

``` text
Internet
   |
   v
ALB
   |
   +------+
   |      |
   v      v
 EC2    EC2
```

------------------------------------------------------------------------

# 43. Complete AWS VPC Architecture

``` text
                              INTERNET
                                  |
                                  v
                          Internet Gateway
                                  |
                                  v
                       +--------------------+
                       |        VPC         |
                       |   10.0.0.0/16      |
                       +--------------------+
                          /              \
                         /                \
                        v                  v
               PUBLIC SUBNET        PRIVATE SUBNET
               10.0.1.0/24          10.0.2.0/24
                    |                     |
                    v                     v
              Load Balancer             EC2
                    |                     |
                    v                     |
                   EC2                    |
                    |                     v
                    +--------------->  RDS
```

------------------------------------------------------------------------

# 44. 3-Tier Architecture

Real-world applications often separate responsibilities into tiers.

## Tier 1: Presentation / Web

``` text
Load Balancer
```

## Tier 2: Application

``` text
EC2 / Containers
```

## Tier 3: Database

``` text
RDS
```

Architecture:

``` text
                  USERS
                    |
                    v
              Load Balancer
                    |
                    v
             Application Tier
                EC2 / ECS
                    |
                    v
               Database Tier
                    |
                    v
                   RDS
```

------------------------------------------------------------------------

# 45. Why Database Should Usually Be Private?

Question:

> "Does every internet user need direct access to our database?"

Answer:

**No.**

Correct architecture:

``` text
User
 |
 v
Application
 |
 v
Database
```

Not:

``` text
User
 |
 v
Database
```

Database directly exposing to the internet unnecessarily increases
attack surface.

------------------------------------------------------------------------

# 46. Real Industry Architecture

Example: E-commerce Application

``` text
                          USERS
                            |
                            v
                         Route 53
                            |
                            v
                       CloudFront
                            |
                            v
                    Application Load Balancer
                            |
                  +---------+---------+
                  |                   |
                  v                   v
              EC2 App 1           EC2 App 2
              Public/App          Public/App
                  |                   |
                  +---------+---------+
                            |
                            v
                     Private Subnet
                            |
                            v
                           RDS
                            |
                            v
                           S3
```

The exact placement and services vary by application requirements.

------------------------------------------------------------------------

# 47. Example: Online Examination Platform

Suppose we are deploying an online examination platform.

Possible architecture:

``` text
Students
   |
   v
Internet
   |
   v
Route 53
   |
   v
Load Balancer
   |
   +----------------+
   |                |
   v                v
App Server 1     App Server 2
   |                |
   +-------+--------+
           |
           v
      Private Network
           |
     +-----+------+
     |            |
     v            v
    RDS           Redis
     |
     v
    S3
```

Possible use:

-   Route 53 → DNS
-   Load Balancer → Traffic distribution
-   EC2/ECS → Application
-   RDS → Database
-   Redis → Cache
-   S3 → Files/results/assets

------------------------------------------------------------------------

# 48. VPC Services / Components and Their Uses

  Component / Service   Main Use
  --------------------- ------------------------------------------------
  VPC                   Create isolated virtual network
  Subnet                Divide VPC network
  Route Table           Control traffic routing
  Internet Gateway      Connect VPC to internet
  NAT Gateway           Outbound internet for private resources
  Security Group        Resource-level firewall
  NACL                  Subnet-level network access control
  Elastic IP            Static public IPv4
  Route 53              DNS and domain routing
  VPC Peering           Private connectivity between VPCs
  VPC Endpoint          Private connectivity to supported AWS services
  ALB                   Distribute HTTP/HTTPS traffic
  CloudFront            CDN and edge content delivery

------------------------------------------------------------------------

# 49. Public vs Private Architecture

## Public Architecture

``` text
Internet
   |
   v
IGW
   |
   v
Public Subnet
   |
   v
Public-facing Resource
```

## Private Architecture

``` text
Private Subnet
   |
   +-- Application
   |
   +-- Database
```

Outbound access if needed:

``` text
Private Resource
      |
      v
 NAT Gateway
      |
      v
Internet Gateway
      |
      v
Internet
```

------------------------------------------------------------------------

# 50. Practical: Create a Custom VPC

## Goal

Create:

``` text
VPC
10.0.0.0/16
```

Inside it:

``` text
Public Subnet
10.0.1.0/24

Private Subnet
10.0.2.0/24
```

Then configure:

-   Internet Gateway
-   Route Table
-   Security Group
-   EC2

------------------------------------------------------------------------

# 51. Practical Step 1: Create VPC

AWS Console:

``` text
AWS Console
    ↓
VPC
    ↓
Your VPCs
    ↓
Create VPC
```

Name:

``` text
training-vpc
```

CIDR:

``` text
10.0.0.0/16
```

Create VPC.

### \[SCREENSHOT: Create VPC screen\]

------------------------------------------------------------------------

# 52. Practical Step 2: Create Public Subnet

Go to:

``` text
VPC
    ↓
Subnets
    ↓
Create subnet
```

Name:

``` text
public-subnet
```

VPC:

``` text
training-vpc
```

CIDR:

``` text
10.0.1.0/24
```

Select an Availability Zone.

### \[SCREENSHOT: Create Public Subnet\]

------------------------------------------------------------------------

# 53. Practical Step 3: Create Private Subnet

Name:

``` text
private-subnet
```

VPC:

``` text
training-vpc
```

CIDR:

``` text
10.0.2.0/24
```

Select an Availability Zone.

### \[SCREENSHOT: Create Private Subnet\]

------------------------------------------------------------------------

# 54. Practical Step 4: Create Internet Gateway

Go to:

``` text
VPC
    ↓
Internet Gateways
    ↓
Create Internet Gateway
```

Name:

``` text
training-igw
```

Create.

Then:

``` text
Actions
    ↓
Attach to VPC
```

Select:

``` text
training-vpc
```

### \[SCREENSHOT: Internet Gateway creation\]

### \[SCREENSHOT: Attach Internet Gateway to VPC\]

------------------------------------------------------------------------

# 55. Practical Step 5: Create Public Route Table

Go to:

``` text
VPC
    ↓
Route Tables
    ↓
Create route table
```

Name:

``` text
public-route-table
```

VPC:

``` text
training-vpc
```

Create.

### \[SCREENSHOT: Create Route Table\]

------------------------------------------------------------------------

# 56. Practical Step 6: Add Internet Route

Open:

``` text
public-route-table
```

Go to:

``` text
Routes
    ↓
Edit routes
    ↓
Add route
```

Destination:

``` text
0.0.0.0/0
```

Target:

``` text
Internet Gateway
```

Select:

``` text
training-igw
```

Save.

### \[SCREENSHOT: Route Table with 0.0.0.0/0\]

------------------------------------------------------------------------

# 57. Practical Step 7: Associate Public Subnet

Open:

``` text
public-route-table
```

Go to:

``` text
Subnet associations
    ↓
Edit subnet associations
```

Select:

``` text
public-subnet
```

Save.

Now:

``` text
Public Subnet
      |
      v
Public Route Table
      |
      v
0.0.0.0/0
      |
      v
Internet Gateway
      |
      v
Internet
```

### \[SCREENSHOT: Subnet association\]

------------------------------------------------------------------------

# 58. Practical Step 8: Create Security Group

Go to:

``` text
EC2
    ↓
Security Groups
    ↓
Create Security Group
```

Name:

``` text
web-sg
```

VPC:

``` text
training-vpc
```

Inbound rules:

``` text
HTTP    80     0.0.0.0/0
HTTPS   443    0.0.0.0/0
SSH     22     Your IP
```

### \[SCREENSHOT: Security Group inbound rules\]

------------------------------------------------------------------------

# 59. Practical Step 9: Launch EC2

Go to:

``` text
EC2
    ↓
Launch Instance
```

Select:

``` text
AMI
Instance Type
Key Pair
```

Network settings:

``` text
VPC:
training-vpc

Subnet:
public-subnet

Security Group:
web-sg
```

Enable public IP assignment when required for this lab.

### \[SCREENSHOT: EC2 Network Settings\]

------------------------------------------------------------------------

# 60. Practical Step 10: Install Nginx

Connect to EC2.

Amazon Linux example:

``` bash
sudo dnf update -y
sudo dnf install nginx -y
```

Start:

``` bash
sudo systemctl start nginx
```

Enable:

``` bash
sudo systemctl enable nginx
```

Check:

``` bash
sudo systemctl status nginx
```

------------------------------------------------------------------------

# 61. Practical Step 11: Test Website

Find EC2 public IPv4 address.

Open browser:

``` text
http://PUBLIC-IP
```

Expected:

``` text
Nginx Welcome Page
```

### \[SCREENSHOT: EC2 Public IP\]

### \[SCREENSHOT: Nginx Welcome Page\]

------------------------------------------------------------------------

# 62. Practical Architecture

After completing the lab:

``` text
                     INTERNET
                         |
                         v
                 Internet Gateway
                         |
                         v
                    VPC
               10.0.0.0/16
                         |
                Public Subnet
                 10.0.1.0/24
                         |
                         v
                        EC2
                         |
                  Security Group
                         |
                       Nginx
```

------------------------------------------------------------------------

# 63. Troubleshooting: Website Not Opening

If Nginx website is not opening, don't randomly change configurations.

Follow this checklist:

``` text
EC2 Running?
     |
     v
Public IP available?
     |
     v
Correct VPC?
     |
     v
Correct Public Subnet?
     |
     v
Route Table associated?
     |
     v
0.0.0.0/0 -> IGW?
     |
     v
IGW attached?
     |
     v
Security Group allows TCP 80?
     |
     v
Nginx running?
```

------------------------------------------------------------------------

# 64. Troubleshooting: SSH Not Working

Check:

``` text
1. EC2 is running
2. Correct public IP
3. Port 22 allowed
4. Source IP is correct
5. Correct key pair
6. Correct username
```

Amazon Linux:

``` bash
ssh -i my-key.pem ec2-user@PUBLIC-IP
```

Ubuntu:

``` bash
ssh -i my-key.pem ubuntu@PUBLIC-IP
```

------------------------------------------------------------------------

# 65. Common Networking Mistakes

### Mistake 1

Thinking:

> "I created a VPC, so EC2 has internet."

Not necessarily.

You need correct routing and security configuration.

------------------------------------------------------------------------

### Mistake 2

Thinking:

> "Internet Gateway automatically makes subnet public."

No.

The subnet's route table must have the appropriate route.

------------------------------------------------------------------------

### Mistake 3

Opening SSH to everyone.

Avoid:

``` text
0.0.0.0/0
```

for SSH unless there is a specific controlled reason.

------------------------------------------------------------------------

### Mistake 4

Making database public.

Prefer private architecture for databases unless a specific architecture
requires otherwise.

------------------------------------------------------------------------

### Mistake 5

Confusing Security Group and NACL.

Remember:

``` text
Security Group = Resource level
NACL = Subnet level
```

------------------------------------------------------------------------

# 66. Security Best Practices

1.  Follow least privilege.
2.  Open only required ports.
3.  Restrict SSH access.
4.  Keep databases private where appropriate.
5.  Use IAM roles instead of hard-coded AWS credentials.
6.  Use HTTPS for public applications.
7.  Monitor network and application activity.
8.  Review security groups regularly.
9.  Avoid unnecessary public IPv4 exposure.
10. Use private connectivity where possible.

------------------------------------------------------------------------

# 67. AWS Networking and DevOps

DevOps engineers need networking because deployments depend on
connectivity.

Typical flow:

``` text
Developer
   |
GitHub
   |
CI/CD
   |
AWS
   |
VPC
   |
Load Balancer
   |
EC2
   |
Database
```

If networking is wrong:

-   Deployment may fail
-   Application may not be reachable
-   Database connection may fail
-   APIs may fail
-   Monitoring agents may fail

Therefore:

> **Networking is one of the foundations of DevOps and Cloud
> Engineering.**

------------------------------------------------------------------------

# 68. Important Real-World Services

## VPC

Virtual network.

## EC2

Compute/server.

## S3

Object storage.

## RDS

Managed relational database.

## Route 53

DNS.

## Elastic Load Balancing

Traffic distribution.

## CloudFront

CDN.

## IAM

Identity and access management.

## CloudWatch

Monitoring and observability.

These services work together with networking.

------------------------------------------------------------------------

# 69. Example Production Architecture

``` text
                         USERS
                           |
                           v
                       Route 53
                           |
                           v
                       CloudFront
                           |
                           v
                    Load Balancer
                           |
             +-------------+-------------+
             |                           |
             v                           v
        Application 1              Application 2
             |                           |
             +-------------+-------------+
                           |
                           v
                    Private Network
                           |
                +----------+----------+
                |                     |
                v                     v
               RDS                  Redis
                |
                v
               S3
```

This is an example architecture. Exact production design depends on
application requirements, security, scale, availability and cost.

------------------------------------------------------------------------

# 70. Interview Questions

## Q1. What is VPC?

A logically isolated virtual network in AWS.

## Q2. Why do we need VPC?

To control IP addressing, routing, connectivity and network security.

## Q3. What is a subnet?

A logical subdivision of a VPC IP range.

## Q4. What makes a subnet public?

Its routing configuration provides a path to an Internet Gateway, along
with the other required public connectivity configuration.

## Q5. What is a private subnet?

A subnet without a direct route to an Internet Gateway for inbound
internet connectivity.

## Q6. What is an Internet Gateway?

A VPC component that provides internet connectivity for appropriately
configured resources.

## Q7. What is NAT Gateway?

A managed NAT service that can provide outbound internet access to
resources in private subnets.

## Q8. What is a Route Table?

A set of routing rules that determines where network traffic is sent.

## Q9. What is Security Group?

A stateful virtual firewall associated with resources/network
interfaces.

## Q10. What is NACL?

A stateless network access control mechanism associated with a subnet.

## Q11. Difference between Security Group and NACL?

Security Group is resource/network-interface level and stateful. NACL is
subnet level and stateless.

## Q12. Why use private subnet?

To keep internal resources away from direct public internet exposure.

## Q13. What is CIDR?

CIDR is a notation used to represent IP address ranges.

## Q14. What is VPC Peering?

Private connectivity between two VPCs.

## Q15. What is VPC Endpoint?

Private connectivity from a VPC to supported AWS services.

------------------------------------------------------------------------

# 71. Quick Revision

Remember:

``` text
VPC
= AWS Virtual Network

Subnet
= Smaller network inside VPC

Route Table
= Where traffic should go

Internet Gateway
= VPC Internet connectivity

NAT Gateway
= Private resources -> Outbound Internet

Security Group
= Resource-level firewall

NACL
= Subnet-level network control

Route 53
= DNS

VPC Peering
= VPC-to-VPC private connectivity

VPC Endpoint
= Private access to supported AWS services

Load Balancer
= Distribute traffic
```

------------------------------------------------------------------------

# 72. Most Important Memory Trick

``` text
VPC
  |
  +-- Subnet
  |      |
  |      +-- Public
  |      |
  |      +-- Private
  |
  +-- Route Table
  |
  +-- Internet Gateway
  |
  +-- NAT Gateway
  |
  +-- Security Group
  |
  +-- NACL
  |
  +-- VPC Endpoint
  |
  +-- VPC Peering
```

------------------------------------------------------------------------

# 73. Final Practical Assignment

## Task

Create:

``` text
VPC
10.0.0.0/16
```

Create:

``` text
Public Subnet
10.0.1.0/24
```

Create:

``` text
Private Subnet
10.0.2.0/24
```

Configure:

-   Internet Gateway
-   Public Route Table
-   Public subnet association
-   Security Group
-   EC2
-   Nginx

Then verify:

``` text
Browser
   |
   v
EC2 Public IP
   |
   v
Nginx
```

------------------------------------------------------------------------

# 74. Student Assignment Questions

Answer these after the practical:

1.  What is VPC?
2.  Why is VPC required?
3.  What is a subnet?
4.  What is the difference between public and private subnet?
5.  What is CIDR?
6.  What is `10.0.0.0/16`?
7.  What is `0.0.0.0/0`?
8.  What is an Internet Gateway?
9.  Why is an Internet Gateway not enough by itself?
10. What is a Route Table?
11. What is NAT Gateway?
12. Why is NAT Gateway useful for private subnets?
13. What is a Security Group?
14. What is NACL?
15. Difference between Security Group and NACL?
16. Why should databases usually be private?
17. What is VPC Peering?
18. What is VPC Endpoint?
19. What is Route 53?
20. Explain the complete flow of a user accessing an EC2 web
    application.

------------------------------------------------------------------------

# 75. Final Takeaway

> **AWS Networking is not just about creating a VPC. It is about
> designing how resources communicate, how traffic is routed, who can
> access what, and how public and private resources are separated.**

Remember:

``` text
VPC
  ↓
Subnet
  ↓
Route Table
  ↓
Internet Gateway / NAT Gateway
  ↓
Security Group / NACL
  ↓
EC2 / Load Balancer / RDS
```

And the most important architecture concept:

``` text
                    USERS
                      |
                      v
                   INTERNET
                      |
                      v
               Internet Gateway
                      |
                      v
                 PUBLIC SUBNET
                      |
                Load Balancer
                      |
             +--------+--------+
             |                 |
             v                 v
           EC2               EC2
             |                 |
             +--------+--------+
                      |
                      v
                 PRIVATE SUBNET
                      |
                      v
                     RDS
```

> **Public resources handle public-facing traffic. Private resources
> handle internal workloads. Routing controls the path. Security
> controls access.**

------------------------------------------------------------------------

# 76. Suggested Next Lecture

After completing VPC fundamentals, the recommended sequence is:

``` text
VPC & Networking
       ↓
Security Groups + NACL
       ↓
NAT Gateway + Internet Gateway
       ↓
Application Load Balancer
       ↓
Auto Scaling
       ↓
S3
       ↓
IAM
       ↓
RDS
       ↓
CloudWatch
       ↓
Docker on AWS
       ↓
CI/CD on AWS
       ↓
Terraform on AWS
```

This sequence gives students a strong foundation before moving into
advanced AWS DevOps topics.
