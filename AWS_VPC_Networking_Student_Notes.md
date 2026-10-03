# AWS VPC & Networking Fundamentals

## Student Learning Notes

**Level:** Beginner\
**Topic:** AWS VPC and Networking\
**Language:** English\
**Goal:** Understand how AWS networking works and build a basic custom
VPC with public/private networking concepts.

------------------------------------------------------------------------

# 1. Learning Objectives

After completing this topic, you should be able to:

-   Explain what a VPC is.
-   Understand AWS Regions and Availability Zones.
-   Understand IP addresses and CIDR notation.
-   Explain subnets.
-   Differentiate between public and private subnets.
-   Understand Route Tables.
-   Explain Internet Gateway and NAT Gateway.
-   Understand Public IP, Private IP, and Elastic IP.
-   Explain Security Groups and Network ACLs.
-   Understand Route 53.
-   Understand VPC Peering and VPC Endpoints.
-   Understand basic Load Balancer networking.
-   Design a basic 3-tier AWS architecture.
-   Create a basic custom VPC and launch an EC2 instance inside it.
-   Troubleshoot common networking problems.

------------------------------------------------------------------------

# 2. Why Do We Need Networking?

Before learning VPC, understand why networking is required.

Imagine that we have an online application.

``` text
Users
  |
Internet
  |
Web Application
  |
Application Server
  |
Database
```

These components need to communicate with each other.

We need to decide:

-   Which resources can access the internet?
-   Which resources should remain private?
-   How should traffic move between resources?
-   Which ports should be allowed?
-   Which resources can communicate with each other?
-   Where should the database be placed?
-   How can we protect the infrastructure?

AWS networking helps us answer these questions.

------------------------------------------------------------------------

# 3. What is a VPC?

## VPC = Virtual Private Cloud

A VPC is a logically isolated virtual network that you create in AWS.

It allows you to control the networking environment for AWS resources
such as:

-   EC2
-   RDS
-   Load Balancers
-   Applications
-   Other supported AWS resources

### Simple Example

``` text
AWS
 |
 +-------------------------+
 |          VPC            |
 |                         |
 |   EC2     EC2     RDS   |
 |                         |
 +-------------------------+
```

Think of a VPC as your own virtual network inside AWS.

------------------------------------------------------------------------

# 4. Why Do We Need a VPC?

A VPC gives you control over:

-   IP address ranges
-   Subnets
-   Routing
-   Internet connectivity
-   Private connectivity
-   Network security
-   Traffic flow

Without networking, cloud resources would not have a controlled
communication structure.

------------------------------------------------------------------------

# 5. Default VPC

AWS accounts can have a default VPC available in a Region.

The default VPC provides basic networking configuration so that users
can launch resources quickly.

A default setup can include:

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

### Why is Default VPC useful?

It is convenient for:

-   Beginners
-   Simple testing
-   Basic experiments
-   Quickly launching EC2 instances

For more controlled environments, organizations may create custom VPCs.

------------------------------------------------------------------------

# 6. Custom VPC

A Custom VPC is a VPC created according to specific requirements.

Example:

``` text
Custom VPC
10.0.0.0/16
 |
 +-- Public Subnet
 |
 +-- Private Application Subnet
 |
 +-- Private Database Subnet
```

A custom VPC gives you more control over the network architecture.

------------------------------------------------------------------------

# 7. AWS Region

A Region is a geographic area where AWS has infrastructure.

Examples:

-   Mumbai
-   Singapore
-   Tokyo
-   London
-   Frankfurt
-   N. Virginia

Example:

``` text
AWS
 |
 +-- Mumbai Region
 +-- Singapore Region
 +-- London Region
 +-- Tokyo Region
```

### Region Selection Factors

When selecting a Region, consider:

-   User location
-   Latency
-   AWS service availability
-   Compliance requirements
-   Cost

------------------------------------------------------------------------

# 8. Availability Zone

An Availability Zone, or AZ, is an isolated infrastructure location
within an AWS Region.

A Region contains multiple Availability Zones.

Example:

``` text
Mumbai Region
 |
 +-- Availability Zone A
 +-- Availability Zone B
 +-- Availability Zone C
```

Using multiple Availability Zones can help build highly available
architectures.

------------------------------------------------------------------------

# 9. Region vs Availability Zone

  -----------------------------------------------------------------------
  Region                              Availability Zone
  ----------------------------------- -----------------------------------
  Geographic AWS location             Isolated infrastructure location
                                      inside a Region

  Contains multiple AZs               Belongs to one Region

  Example: Mumbai                     Example: an AZ within Mumbai
  -----------------------------------------------------------------------

### Remember

**Region = Geographic area**

**Availability Zone = Infrastructure location inside a Region**

------------------------------------------------------------------------

# 10. IP Address

An IP address identifies a resource on a network.

Example:

``` text
10.0.1.10
```

In AWS networking, you will commonly work with:

-   Private IPv4 addresses
-   Public IPv4 addresses
-   Elastic IP addresses
-   IPv6 addresses

------------------------------------------------------------------------

# 11. Private IP Address

A private IP address is used for communication within private
networking.

Example:

``` text
EC2-1
Private IP: 10.0.1.10

EC2-2
Private IP: 10.0.1.20
```

Resources inside a VPC can communicate using private addressing, subject
to routing and security rules.

------------------------------------------------------------------------

# 12. Public IP Address

A public IP address can be used for internet-facing connectivity.

Example:

``` text
Internet
   |
Public IP
   |
EC2
```

A public IP alone does not guarantee connectivity. Correct routing and
security configuration are also required.

------------------------------------------------------------------------

# 13. Private IP vs Public IP

  Private IP                      Public IP
  ------------------------------- ---------------------------------------
  Used for internal networking    Used for internet-facing connectivity
  Used inside VPC/networking      Publicly routable
  Example: 10.0.1.10              Public IPv4 address
  Common for internal resources   Used when public access is required

------------------------------------------------------------------------

# 14. What is CIDR?

CIDR stands for:

**Classless Inter-Domain Routing**

CIDR notation is used to define an IP address range.

Example:

``` text
10.0.0.0/16
```

A VPC can use a CIDR range such as:

``` text
10.0.0.0/16
```

Subnets can use smaller ranges:

``` text
10.0.1.0/24
10.0.2.0/24
10.0.3.0/24
```

------------------------------------------------------------------------

# 15. Example CIDR Structure

``` text
                 VPC
            10.0.0.0/16
                  |
        +---------+---------+
        |         |         |
        v         v         v
     Subnet    Subnet    Subnet
   10.0.1.0   10.0.2.0  10.0.3.0
      /24        /24        /24
```

The VPC has a larger network range, and subnets use smaller ranges
inside it.

------------------------------------------------------------------------

# 16. What is a Subnet?

A subnet is a logical subdivision of a VPC IP range.

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

Subnets help us separate resources logically.

------------------------------------------------------------------------

# 17. Why Do We Need Subnets?

Subnets can be used to separate different types of resources.

Example:

``` text
VPC
 |
 +-- Public Subnet
 |      |
 |      +-- Load Balancer
 |
 +-- Private Application Subnet
 |      |
 |      +-- Application Servers
 |
 +-- Private Database Subnet
        |
        +-- Database
```

This separation helps with:

-   Security
-   Organization
-   Routing
-   Architecture design

------------------------------------------------------------------------

# 18. Public Subnet

A public subnet is a subnet whose routing configuration provides a path
toward an Internet Gateway.

Typical architecture:

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

A subnet is not public simply because it has the word "public" in its
name.

A public subnet requires appropriate:

-   Route table configuration
-   Internet Gateway connectivity
-   Public IP addressing where required
-   Security rules

------------------------------------------------------------------------

# 19. Private Subnet

A private subnet does not have a direct route to an Internet Gateway for
inbound internet connectivity.

Example:

``` text
Private Subnet
 |
 +-- Application Server
 |
 +-- Database
```

Private resources can still communicate with other resources inside the
VPC according to routing and security rules.

------------------------------------------------------------------------

# 20. Public Subnet vs Private Subnet

  -----------------------------------------------------------------------
  Public Subnet                       Private Subnet
  ----------------------------------- -----------------------------------
  Has a route toward an Internet      No direct route to Internet Gateway
  Gateway                             

  Used for public-facing architecture Used for internal resources

  May contain public-facing Load      Commonly used for
  Balancers                           application/database resources

  Public IP may be required           Public IP is generally avoided
  -----------------------------------------------------------------------

------------------------------------------------------------------------

# 21. Internet Gateway

## IGW = Internet Gateway

An Internet Gateway is a VPC component that provides connectivity
between a VPC and the internet for appropriately configured resources.

Basic flow:

``` text
Internet
   |
   v
Internet Gateway
   |
   v
VPC
```

------------------------------------------------------------------------

# 22. Internet Gateway Does Not Automatically Make EC2 Public

Creating or attaching an Internet Gateway is only one part of the
configuration.

For public connectivity, you may need:

``` text
VPC
 |
Subnet
 |
Route Table
 |
0.0.0.0/0 -> Internet Gateway
 |
EC2
 |
Public IPv4
 |
Security Group
```

All relevant configurations must be correct.

------------------------------------------------------------------------

# 23. Route Table

A Route Table contains routing rules that determine where network
traffic should go.

Example:

``` text
Destination        Target

10.0.0.0/16        local
0.0.0.0/0          Internet Gateway
```

This means:

-   Traffic within the VPC CIDR uses the local route.
-   Other IPv4 destinations can be sent to the Internet Gateway
    according to the route.

------------------------------------------------------------------------

# 24. What is 0.0.0.0/0?

`0.0.0.0/0` represents all IPv4 destinations.

Example:

``` text
Destination: 0.0.0.0/0
Target: Internet Gateway
```

This is commonly used as the default route for a public subnet.

------------------------------------------------------------------------

# 25. Local Route

A VPC has a local route for its own CIDR.

Example:

``` text
VPC CIDR:
10.0.0.0/16

Route:
10.0.0.0/16 -> local
```

This allows communication within the VPC according to the network
architecture and security controls.

------------------------------------------------------------------------

# 26. NAT Gateway

## NAT = Network Address Translation

A NAT Gateway can provide outbound internet access for resources in a
private subnet.

Typical architecture:

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

------------------------------------------------------------------------

# 27. Why Do We Need NAT Gateway?

Suppose an application server is in a private subnet.

It needs to:

-   Download software updates
-   Download packages
-   Access an external API

But you do not want to give the server direct public internet exposure.

A NAT Gateway can provide outbound connectivity.

``` text
Private Server
      |
      v
NAT Gateway
      |
      v
Internet
```

------------------------------------------------------------------------

# 28. NAT Gateway vs Internet Gateway

  -----------------------------------------------------------------------
  Internet Gateway                    NAT Gateway
  ----------------------------------- -----------------------------------
  Provides VPC internet connectivity  Provides outbound internet access
                                      for private resources

  Used for public networking          Commonly used by private subnets

  Attached to VPC                     Used as a route target

  Supports public internet            Helps keep private resources
  architecture                        without direct inbound internet
                                      access
  -----------------------------------------------------------------------

------------------------------------------------------------------------

# 29. Elastic IP

An Elastic IP is a static public IPv4 address that can be allocated to
your AWS account and associated with supported resources.

Example:

``` text
EC2
 |
Elastic IP
 |
Internet
```

### Use Cases

-   Stable public endpoint
-   Specific infrastructure requirements
-   Applications that require a fixed public IPv4 address

### Important

Avoid allocating public IPv4 addresses unnecessarily because AWS charges
may apply.

------------------------------------------------------------------------

# 30. Security Group

A Security Group is a stateful virtual firewall associated with
resources such as EC2 network interfaces.

It controls:

-   Inbound traffic
-   Outbound traffic

Example:

``` text
Security Group
 |
 +-- HTTP 80
 +-- HTTPS 443
 +-- SSH 22
```

------------------------------------------------------------------------

# 31. Example Security Group

For a web server:

``` text
HTTP
Port: 80
Source: 0.0.0.0/0
```

For HTTPS:

``` text
HTTPS
Port: 443
Source: 0.0.0.0/0
```

For SSH:

``` text
SSH
Port: 22
Source: Your Trusted IP
```

### Best Practice

Do not unnecessarily expose SSH to:

``` text
0.0.0.0/0
```

Restrict administrative access whenever possible.

------------------------------------------------------------------------

# 32. Security Group Characteristics

Security Groups are:

-   Stateful
-   Associated with network interfaces/resources
-   Based on allow rules
-   Used for inbound and outbound traffic control

Because they are stateful, return traffic for an allowed connection is
generally automatically permitted.

------------------------------------------------------------------------

# 33. Network ACL

## NACL = Network Access Control List

A Network ACL provides network-level access control at the subnet level.

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

NACLs are:

-   Subnet-level
-   Stateless
-   Based on numbered rules
-   Able to contain Allow and Deny rules

------------------------------------------------------------------------

# 34. Security Group vs NACL

  -----------------------------------------------------------------------
  Security Group                      NACL
  ----------------------------------- -----------------------------------
  Resource/network-interface level    Subnet level

  Stateful                            Stateless

  Allow rules                         Allow and Deny rules

  Associated with resources/network   Associated with subnets
  interfaces                          

  Commonly used for EC2 access        Additional subnet-level control
  control                             
  -----------------------------------------------------------------------

### Remember

**Security Group = Resource-level firewall**

**NACL = Subnet-level network control**

------------------------------------------------------------------------

# 35. DNS in AWS

DNS stands for:

**Domain Name System**

DNS translates domain names into IP addresses.

Example:

``` text
example.com
     |
     v
IP Address
```

DNS is important because users normally access applications using domain
names rather than IP addresses.

------------------------------------------------------------------------

# 36. Route 53

Amazon Route 53 is AWS's DNS service.

Common uses:

-   DNS management
-   Domain routing
-   Health checks
-   Routing policies
-   Domain registration features

Basic flow:

``` text
User
 |
example.com
 |
Route 53
 |
AWS Application
```

------------------------------------------------------------------------

# 37. VPC Peering

VPC Peering provides private network connectivity between two VPCs.

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

Resources in the VPCs can communicate using private addressing when
routes and security rules allow it.

### Important

The VPC CIDR ranges should generally not overlap.

------------------------------------------------------------------------

# 38. VPC Endpoint

A VPC Endpoint provides private connectivity from a VPC to supported AWS
services.

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

A common use case is private access from a VPC to S3.

VPC Endpoints can reduce the need for internet/NAT paths for supported
AWS services.

------------------------------------------------------------------------

# 39. Load Balancer Basics

A Load Balancer distributes incoming traffic across multiple targets.

Example:

``` text
Users
  |
  v
Load Balancer
  |
  +-------+-------+
  |               |
  v               v
 EC2             EC2
```

Benefits include:

-   Traffic distribution
-   Health checks
-   High availability architecture
-   Integration with Auto Scaling

------------------------------------------------------------------------

# 40. Application Load Balancer

An Application Load Balancer, or ALB, is commonly used for HTTP/HTTPS
applications.

Example:

``` text
Internet
   |
   v
ALB
   |
   +-------+
   |       |
   v       v
 EC2     EC2
```

ALB can route HTTP/HTTPS traffic to application targets.

------------------------------------------------------------------------

# 41. Three-Tier Architecture

A common application architecture is divided into three layers.

## Tier 1: Presentation/Web Tier

Example:

``` text
Load Balancer
```

## Tier 2: Application Tier

Example:

``` text
EC2
ECS
Application Servers
```

## Tier 3: Database Tier

Example:

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

# 42. Why Keep Databases Private?

A database normally does not need direct access from every internet
user.

Preferred architecture:

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

Keeping the database private can reduce unnecessary public exposure.

------------------------------------------------------------------------

# 43. Complete VPC Architecture

``` text
                         INTERNET
                            |
                            v
                    Internet Gateway
                            |
                            v
                 +---------------------+
                 |         VPC         |
                 |    10.0.0.0/16      |
                 +---------------------+
                    /               \
                   /                 \
                  v                   v
          PUBLIC SUBNET        PRIVATE SUBNET
          10.0.1.0/24          10.0.2.0/24
               |                     |
               v                     v
         Load Balancer              EC2
               |                     |
               v                     |
              EC2                    |
               |                     v
               +------------------> RDS
```

------------------------------------------------------------------------

# 44. Real-World Example: E-Commerce

A possible architecture:

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
                 |                   |
                 +---------+---------+
                           |
                           v
                     Private Network
                           |
                    +------+------+
                    |             |
                    v             v
                   RDS          Redis
                    |
                    v
                   S3
```

Possible service roles:

  Service      Purpose
  ------------ ----------------------
  Route 53     DNS
  CloudFront   Content delivery/CDN
  ALB          Traffic distribution
  EC2          Application compute
  RDS          Relational database
  Redis        Caching
  S3           Object storage

The exact architecture depends on application requirements.

------------------------------------------------------------------------

# 45. VPC Components and Services

  Service / Component   Main Purpose
  --------------------- ------------------------------------------
  VPC                   Virtual network
  Subnet                Divide the VPC network
  Route Table           Control routing
  Internet Gateway      VPC internet connectivity
  NAT Gateway           Outbound internet for private resources
  Security Group        Resource-level firewall
  NACL                  Subnet-level access control
  Elastic IP            Static public IPv4
  Route 53              DNS
  VPC Peering           Private VPC-to-VPC connectivity
  VPC Endpoint          Private access to supported AWS services
  Load Balancer         Distribute application traffic
  CloudFront            CDN / edge content delivery

------------------------------------------------------------------------

# 46. Practical Lab: Create a Custom VPC

## Lab Objective

Create:

``` text
VPC
10.0.0.0/16
```

Inside the VPC:

``` text
Public Subnet
10.0.1.0/24

Private Subnet
10.0.2.0/24
```

Configure:

-   Internet Gateway
-   Public Route Table
-   Security Group
-   EC2
-   Nginx
-   Public connectivity

------------------------------------------------------------------------

# 47. Step 1: Create VPC

Go to:

``` text
AWS Console
   |
VPC
   |
Your VPCs
   |
Create VPC
```

Name:

``` text
training-vpc
```

IPv4 CIDR:

``` text
10.0.0.0/16
```

Create the VPC.

### \[SCREENSHOT: Create VPC page\]

------------------------------------------------------------------------

# 48. Step 2: Create Public Subnet

Go to:

``` text
VPC
   |
Subnets
   |
Create subnet
```

Configure:

``` text
Name:
public-subnet

VPC:
training-vpc

CIDR:
10.0.1.0/24
```

Choose an Availability Zone.

Create the subnet.

### \[SCREENSHOT: Public Subnet configuration\]

------------------------------------------------------------------------

# 49. Step 3: Create Private Subnet

Create another subnet:

``` text
Name:
private-subnet

VPC:
training-vpc

CIDR:
10.0.2.0/24
```

Choose an Availability Zone.

### \[SCREENSHOT: Private Subnet configuration\]

------------------------------------------------------------------------

# 50. Step 4: Create Internet Gateway

Go to:

``` text
VPC
   |
Internet Gateways
   |
Create Internet Gateway
```

Name:

``` text
training-igw
```

Create it.

Then:

``` text
Actions
   |
Attach to VPC
```

Select:

``` text
training-vpc
```

### \[SCREENSHOT: Internet Gateway creation\]

### \[SCREENSHOT: Attach Internet Gateway to VPC\]

------------------------------------------------------------------------

# 51. Step 5: Create Public Route Table

Go to:

``` text
VPC
   |
Route Tables
   |
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

# 52. Step 6: Add Internet Route

Open:

``` text
public-route-table
```

Go to:

``` text
Routes
   |
Edit routes
   |
Add route
```

Configure:

``` text
Destination:
0.0.0.0/0

Target:
Internet Gateway

training-igw
```

Save the route.

### \[SCREENSHOT: Public Route Table with Internet Route\]

------------------------------------------------------------------------

# 53. Step 7: Associate Public Subnet

Open:

``` text
public-route-table
```

Go to:

``` text
Subnet associations
   |
Edit subnet associations
```

Select:

``` text
public-subnet
```

Save.

### \[SCREENSHOT: Route Table Subnet Association\]

------------------------------------------------------------------------

# 54. Step 8: Create Security Group

Go to:

``` text
EC2
   |
Security Groups
   |
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

Example inbound rules:

``` text
HTTP
Port: 80
Source: 0.0.0.0/0

HTTPS
Port: 443
Source: 0.0.0.0/0

SSH
Port: 22
Source: Your IP
```

### \[SCREENSHOT: Security Group Rules\]

------------------------------------------------------------------------

# 55. Step 9: Launch EC2 in Public Subnet

Go to:

``` text
EC2
   |
Launch Instance
```

Select:

-   Required AMI
-   Instance type
-   Key pair

Network settings:

``` text
VPC:
training-vpc

Subnet:
public-subnet

Security Group:
web-sg
```

Enable public IPv4 assignment when required for this lab.

### \[SCREENSHOT: EC2 Network Settings\]

------------------------------------------------------------------------

# 56. Step 10: Install Nginx

Connect to the EC2 instance.

For Amazon Linux:

``` bash
sudo dnf update -y
sudo dnf install nginx -y
```

Start Nginx:

``` bash
sudo systemctl start nginx
```

Enable it:

``` bash
sudo systemctl enable nginx
```

Check:

``` bash
sudo systemctl status nginx
```

------------------------------------------------------------------------

# 57. Step 11: Test the Web Server

Find the EC2 public IPv4 address.

Open:

``` text
http://PUBLIC-IP
```

Expected result:

``` text
Nginx Welcome Page
```

### \[SCREENSHOT: EC2 Public IPv4 Address\]

### \[SCREENSHOT: Nginx Welcome Page in Browser\]

------------------------------------------------------------------------

# 58. Practical Architecture

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
                            v
                          Nginx
```

------------------------------------------------------------------------

# 59. Troubleshooting: Website Not Opening

Check the following in order:

``` text
1. Is EC2 running?
        |
        v
2. Does EC2 have a public IPv4?
        |
        v
3. Is EC2 inside the correct VPC?
        |
        v
4. Is EC2 inside the public subnet?
        |
        v
5. Is the route table associated with the subnet?
        |
        v
6. Does the route table have 0.0.0.0/0 -> IGW?
        |
        v
7. Is the Internet Gateway attached to the VPC?
        |
        v
8. Does Security Group allow TCP 80?
        |
        v
9. Is Nginx running?
```

------------------------------------------------------------------------

# 60. Troubleshooting: SSH Not Working

Check:

-   EC2 is running.
-   Correct public IP is being used.
-   Port 22 is allowed.
-   Security Group source IP is correct.
-   Correct private key is being used.
-   Correct username is being used.

Amazon Linux:

``` bash
ssh -i my-key.pem ec2-user@PUBLIC-IP
```

Ubuntu:

``` bash
ssh -i my-key.pem ubuntu@PUBLIC-IP
```

------------------------------------------------------------------------

# 61. Common Mistakes

## Mistake 1: Assuming VPC automatically provides internet

A VPC does not automatically make every resource internet-accessible.

Routing and security must be configured correctly.

------------------------------------------------------------------------

## Mistake 2: Assuming Internet Gateway automatically makes a subnet public

The subnet needs an appropriate route to the Internet Gateway.

------------------------------------------------------------------------

## Mistake 3: Opening SSH to everyone

Avoid unnecessarily using:

``` text
0.0.0.0/0
```

for SSH.

------------------------------------------------------------------------

## Mistake 4: Making the database public

Databases should generally be placed in private networking when public
access is not required.

------------------------------------------------------------------------

## Mistake 5: Confusing Security Groups and NACLs

Remember:

``` text
Security Group = Resource level
NACL = Subnet level
```

------------------------------------------------------------------------

# 62. Security Best Practices

Follow these principles:

### 1. Least Privilege

Allow only required access.

### 2. Restrict SSH

Allow administrative access only from trusted sources where possible.

### 3. Keep Databases Private

Avoid unnecessary public exposure.

### 4. Use HTTPS

Use TLS for public applications.

### 5. Use IAM Roles

Avoid hard-coding AWS access keys on EC2.

### 6. Review Security Groups

Remove unnecessary rules.

### 7. Avoid Unnecessary Public IPs

Use private networking when public access is not required.

### 8. Monitor Your Environment

Use appropriate AWS monitoring and logging services.

------------------------------------------------------------------------

# 63. AWS Networking in DevOps

Networking is one of the most important foundations of Cloud and DevOps.

Typical deployment:

``` text
Developer
   |
   v
GitHub
   |
   v
CI/CD Pipeline
   |
   v
AWS
   |
   v
VPC
   |
   v
Load Balancer
   |
   v
Application
   |
   v
Database
```

If networking is incorrectly configured:

-   Application may not be reachable.
-   Database connections may fail.
-   Deployment may fail.
-   APIs may not work.
-   Monitoring agents may not communicate.

------------------------------------------------------------------------

# 64. Real-World Architecture Example

A production application may look conceptually like:

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
             App Server 1        App Server 2
                 |                   |
                 +---------+---------+
                           |
                           v
                    Private Network
                           |
                  +--------+--------+
                  |                 |
                  v                 v
                 RDS              Redis
                  |
                  v
                 S3
```

This is an example architecture. Real production designs vary based on
requirements.

------------------------------------------------------------------------

# 65. Important Services to Remember

  Service            What It Does
  ------------------ --------------------------------------------------
  VPC                Creates a virtual network
  EC2                Provides virtual compute
  Subnet             Divides a VPC network
  Route Table        Controls routing
  Internet Gateway   Provides internet connectivity
  NAT Gateway        Provides outbound internet for private resources
  Security Group     Resource-level firewall
  NACL               Subnet-level access control
  Elastic IP         Static public IPv4
  Route 53           DNS
  VPC Peering        Connects VPCs privately
  VPC Endpoint       Private connectivity to supported AWS services
  ALB                Distributes HTTP/HTTPS traffic
  CloudFront         CDN and edge delivery
  RDS                Managed relational database
  S3                 Object storage
  CloudWatch         Monitoring and observability
  IAM                Identity and access management

------------------------------------------------------------------------

# 66. Interview Questions

## Q1. What is a VPC?

A VPC is a logically isolated virtual network in AWS.

## Q2. Why do we need a VPC?

To control IP addressing, routing, connectivity, and network security.

## Q3. What is a subnet?

A logical subdivision of a VPC IP range.

## Q4. What makes a subnet public?

Its routing configuration provides a path toward an Internet Gateway,
along with the required public IP and security configuration.

## Q5. What is a private subnet?

A subnet without a direct route to an Internet Gateway for inbound
internet connectivity.

## Q6. What is an Internet Gateway?

A VPC component that provides connectivity between appropriately
configured VPC resources and the internet.

## Q7. What is NAT Gateway?

A managed NAT service that can provide outbound internet access to
resources in private subnets.

## Q8. What is a Route Table?

A set of routing rules that determines where traffic is sent.

## Q9. What is a Security Group?

A stateful virtual firewall associated with resources/network
interfaces.

## Q10. What is a Network ACL?

A stateless subnet-level network access control mechanism.

## Q11. What is the difference between Security Group and NACL?

Security Groups are stateful and associated with resources/network
interfaces. NACLs are stateless and associated with subnets.

## Q12. Why use private subnets?

To keep internal resources away from direct public internet exposure.

## Q13. What is CIDR?

CIDR is a notation used to define IP address ranges.

## Q14. What is VPC Peering?

Private network connectivity between two VPCs.

## Q15. What is a VPC Endpoint?

Private connectivity from a VPC to supported AWS services.

## Q16. What is Route 53?

AWS DNS and domain routing service.

## Q17. What is an Elastic IP?

A static public IPv4 address that can be allocated and associated with
supported AWS resources.

## Q18. Why is NAT Gateway used?

To provide outbound internet connectivity for private subnet resources
without giving those resources direct inbound internet connectivity.

------------------------------------------------------------------------

# 67. Quick Revision

``` text
VPC
=
Virtual network in AWS

Subnet
=
Smaller network inside VPC

Public Subnet
=
Subnet with route toward Internet Gateway

Private Subnet
=
Subnet without direct Internet Gateway route

Route Table
=
Controls where traffic goes

Internet Gateway
=
Internet connectivity for VPC

NAT Gateway
=
Outbound internet for private resources

Security Group
=
Resource-level stateful firewall

NACL
=
Subnet-level stateless network control

Route 53
=
DNS

VPC Peering
=
Private VPC-to-VPC connectivity

VPC Endpoint
=
Private access to supported AWS services

Load Balancer
=
Distributes application traffic
```

------------------------------------------------------------------------

# 68. The Most Important Concept

Remember this:

``` text
VPC
 |
 +-- Subnets
 |     |
 |     +-- Public
 |     |
 |     +-- Private
 |
 +-- Route Tables
 |
 +-- Internet Gateway
 |
 +-- NAT Gateway
 |
 +-- Security Groups
 |
 +-- NACLs
 |
 +-- VPC Endpoints
 |
 +-- VPC Peering
```

------------------------------------------------------------------------

# 69. Final Architecture to Remember

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
                 +---------+---------+
                 |                   |
                 v                   v
               EC2                 EC2
                 |                   |
                 +---------+---------+
                           |
                           v
                    PRIVATE SUBNET
                           |
                           v
                          RDS
```

For private outbound internet:

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

------------------------------------------------------------------------

# 70. Practice Assignment

## Task

Create the following:

``` text
VPC:
10.0.0.0/16

Public Subnet:
10.0.1.0/24

Private Subnet:
10.0.2.0/24
```

Configure:

-   Internet Gateway
-   Public Route Table
-   Public Subnet Association
-   Security Group
-   EC2
-   Nginx

Then verify that the EC2 web server is reachable through its public IPv4
address.

------------------------------------------------------------------------

# 71. Assignment Questions

Answer the following:

1.  What is a VPC?
2.  Why is a VPC required?
3.  What is a subnet?
4.  What is the difference between public and private subnets?
5.  What is CIDR?
6.  What does `10.0.0.0/16` represent?
7.  What does `0.0.0.0/0` represent?
8.  What is an Internet Gateway?
9.  Why is an Internet Gateway alone not enough to make an EC2 publicly
    accessible?
10. What is a Route Table?
11. What is a NAT Gateway?
12. Why is NAT Gateway useful for private subnets?
13. What is a Security Group?
14. What is a NACL?
15. What is the difference between Security Group and NACL?
16. Why should databases generally be private?
17. What is VPC Peering?
18. What is a VPC Endpoint?
19. What is Route 53?
20. Explain the complete network flow when a user opens a web
    application hosted on AWS.

------------------------------------------------------------------------

# 72. Final Takeaway

AWS networking is not only about creating a VPC.

It is about understanding:

-   How resources communicate
-   How traffic is routed
-   How public and private resources are separated
-   How internet access is provided
-   How network traffic is secured
-   How applications are designed for availability and scalability

The basic flow is:

``` text
VPC
  |
  v
Subnet
  |
  v
Route Table
  |
  +----> Internet Gateway
  |
  +----> NAT Gateway
  |
  v
Security Controls
  |
  v
EC2 / Load Balancer / RDS
```

### Key Rule

> **Public resources handle public-facing traffic. Private resources
> handle internal workloads. Route Tables control traffic paths, while
> Security Groups and NACLs control network access.**

------------------------------------------------------------------------

# 73. Recommended Next Topics

After completing VPC and Networking Fundamentals, continue with:

``` text
VPC & Networking
       |
       v
Security Groups + NACL
       |
       v
NAT Gateway + Internet Gateway
       |
       v
Application Load Balancer
       |
       v
Auto Scaling
       |
       v
S3
       |
       v
IAM
       |
       v
RDS
       |
       v
CloudWatch
       |
       v
Docker on AWS
       |
       v
CI/CD on AWS
       |
       v
Terraform on AWS
```

------------------------------------------------------------------------

# Screenshot Checklist

Add your own AWS Console screenshots at these points:

-   \[SCREENSHOT: AWS VPC dashboard\]
-   \[SCREENSHOT: Create VPC\]
-   \[SCREENSHOT: VPC details\]
-   \[SCREENSHOT: Create public subnet\]
-   \[SCREENSHOT: Create private subnet\]
-   \[SCREENSHOT: Internet Gateway\]
-   \[SCREENSHOT: Attach Internet Gateway\]
-   \[SCREENSHOT: Route Table\]
-   \[SCREENSHOT: Add 0.0.0.0/0 route\]
-   \[SCREENSHOT: Subnet route table association\]
-   \[SCREENSHOT: Security Group\]
-   \[SCREENSHOT: EC2 network configuration\]
-   \[SCREENSHOT: EC2 public IPv4\]
-   \[SCREENSHOT: Nginx browser page\]
-   \[SCREENSHOT: Final VPC architecture / resource map\]

------------------------------------------------------------------------

# End of Lecture

**Core concepts to remember:**

``` text
VPC
Subnet
CIDR
Route Table
Internet Gateway
NAT Gateway
Public IP
Private IP
Elastic IP
Security Group
NACL
Route 53
VPC Peering
VPC Endpoint
Load Balancer
```

> **Cloud infrastructure starts with compute, but reliable cloud
> architecture starts with networking.**
