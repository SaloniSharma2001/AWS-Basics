# AWS Basics

 > A personal collection of AWS fundamentals, architecture concepts, commands, interview questions, and practical notes.

 This repository contains my notes while learning **Amazon Web Services (AWS)**, covering compute, storage, databases, networking, security, load balancing, Docker, and other fundamental cloud concepts.

---

 ## Table of Contents

 \<details\> \<summary\>\<strong\>Click to expand\</strong\>\</summary\> - 🔤 AWS Acronyms
- 🌎AWS Regions and Availability Zones
- 💻 EC2
  - AMI
  - EC2 Instance Types
  - Key Pairs
  - SSH
  - EC2 Instance Connect
- 🔐 Linux Permissions
- 🆔 ARN
- 👤 IAM
- 🔢 TOTP
- 🔑 STS
- 🔐 RSA
- 🪣 S3
  - Object Storage
  - S3 Encryption
  - Pre-Signed URLs
- 🗄️ RDS
  - Supported Databases
  - Multi-AZ
  - Read Replicas
  - Snapshots
  - IOPS
- ⚖️ Load Balancing
  - ALB
  - Target Groups
  - Health Checks
  - Path-Based Routing
  - ALB vs NLB vs GWLB
- 🔒 Security Groups
- 🐳 Docker Port Mapping
- 🌐 Common Ports
- 🏗️ Example AWS Architecture
- ❓ Questions to Remember
- 🧠 Quick Revision
- 🚀 Learning Roadmap

 \</details\>
---

 # 🔤 AWS Acronyms

 | Acronym | Full Form | What it means |
| --- | --- | --- |
| **AWS** | Amazon Web Services | Cloud computing platform |
| **EC2** | Elastic Compute Cloud | Virtual servers in AWS |
| **VM** | Virtual Machine | Software-based computer |
| **AMI** | Amazon Machine Image | Template used to launch EC2 instances |
| **SSH** | Secure Shell | Secure protocol for remotely accessing machines |
| **ARN** | Amazon Resource Name | Unique identifier for AWS resources |
| **S3** | Amazon Simple Storage Service | Object storage service |
| **IAM** | Identity and Access Management | AWS identity and access-control service |
| **RDS** | Relational Database Service | Managed relational database service |
| **TOTP** | Time-Based One-Time Password | Time-based authentication code |
| **STS** | Security Token Service | Provides temporary AWS credentials |
| **ALB** | Application Load Balancer | Layer 7 HTTP/HTTPS load balancer |
| **NLB** | Network Load Balancer | Layer 4 load balancer |
| **GWLB** | Gateway Load Balancer | Used primarily for network/security appliances |
| **AZ** | Availability Zone | Isolated location inside an AWS Region |
| **VPC** | Virtual Private Cloud | Logically isolated virtual network |
| **HTTP** | Hypertext Transfer Protocol | Protocol used for web traffic |
| **HTTPS** | HTTP Secure | Encrypted HTTP traffic |
| **IOPS** | Input/Output Operations Per Second | Measure of storage I/O performance |

---

 # 🌎 AWS Regions and Availability Zones

 AWS infrastructure is divided into **Regions** and **Availability Zones (AZs)**.

```
                         AWS
                          │
             ┌────────────┴────────────┐
             │                         │
          Region A                  Region B
             │                         │
       ┌─────┼─────┐             ┌─────┼─────┐
       │     │     │             │     │     │
      AZ-A  AZ-B  AZ-C          AZ-A  AZ-B  AZ-C
```

 ## AWS Region

 An AWS Region is a geographic area containing multiple Availability Zones.

 Examples include:

 - Mumbai
- Singapore
- Frankfurt
- Northern Virginia
- Oregon

 A Region is independent of another Region.

 For example:

```
Mumbai Region
     │
     ├── AZ-A
     ├── AZ-B
     └── AZ-C

Singapore Region
     │
     ├── AZ-A
     ├── AZ-B
     └── AZ-C
```

 ## Availability Zone

 An Availability Zone is an isolated infrastructure location within an AWS Region.

 For example:

```
Region
│
├── Availability Zone A
├── Availability Zone B
└── Availability Zone C
```

 Deploying application components across multiple AZs helps protect against failures affecting a single AZ.

 > **Important:** Multi-AZ and Multi-Region are different concepts.
>
>  - **Multi-AZ** → multiple Availability Zones within a Region
> - **Multi-Region** → multiple AWS Regions

---

 # 💻 EC2 — Elastic Compute Cloud

 Amazon EC2 provides virtual computing capacity in AWS.

 In simple terms:

 > **EC2 = Virtual Machine / Server running in the AWS Cloud**

 An EC2 instance can have:

 - CPU
- Memory
- Operating system
- Storage
- Network interface
- Security groups
- IAM role
- Public/private IP addresses

 A simplified architecture:

```
                         AWS
                          │
                         EC2
                          │
          ┌───────────────┼───────────────┐
          │               │               │
         CPU            Memory          Storage
          │               │               │
          └───────────────┼───────────────┘
                          │
                    Operating System
```

---

 ## AMI — Amazon Machine Image

 An **AMI** is a template used to launch an EC2 instance.

 An AMI can contain:

 - Operating system
- Software
- Configuration
- Required packages
- Application setup

 Example:

```
              Ubuntu AMI
                  │
          ┌───────┼───────┐
          ▼       ▼       ▼
        EC2-1   EC2-2   EC2-3
```

 Instead of manually configuring every server, we can create an AMI containing the desired configuration and use it to launch multiple instances.

---

 ## EC2 Instance Types

 An EC2 instance type determines the resources available to the virtual machine.

 Examples of resources include:

 - vCPUs
- Memory
- Network performance
- EBS bandwidth
- Specialized hardware in certain instance families

 Different workloads require different instance types.

 For example:

```
General purpose
       │
       ▼
Web applications

Compute optimized
       │
       ▼
CPU-intensive applications

Memory optimized
       │
       ▼
Memory-intensive workloads

Storage optimized
       │
       ▼
Storage-intensive workloads
```

---

 # 🔑 Key Pairs

 EC2 key pairs are commonly used for SSH authentication.

 A key pair consists of:

```
Public Key
     +
Private Key
```

 AWS stores/associates the public key with the EC2 instance, while the private key is downloaded by you when creating/downloading the key pair.

 Common file formats include:

```
.pem
.ppk
```

 ### `.pem`

 Commonly used with OpenSSH.

 Example:

```
ssh -i my-key.pem ec2-user@<PUBLIC-IP>
```

 ### `.ppk`

 Commonly associated with PuTTY.

 > **Important:** `.pem` does not mean "Mac" and `.ppk` does not strictly mean "Windows". The format depends primarily on the SSH client/tool being used.

---

 # 🔐 SSH — Secure Shell

 SSH allows secure remote access to a machine.

 Default SSH port:

```
22
```

 Example:

```
ssh -i vins.pem ec2-user@<EC2_PUBLIC_IP>
```

 The username depends on the operating system/AMI.

 Examples:

 | AMI | Typical User |
| --- | --- |
| Amazon Linux | `ec2-user` |
| Ubuntu | `ubuntu` |

The general flow:

```
Your Computer
      │
      │ SSH
      │ Port 22
      ▼
EC2 Instance
```

---

 # 🖥️ EC2 Instance Connect

 AWS provides **EC2 Instance Connect** as another method for connecting to supported EC2 instances.

 Instead of manually running:

```
ssh -i key.pem user@ip
```

 you can use the AWS Console's EC2 Instance Connect feature where supported.

 Conceptually:

```
AWS Console
     │
     │ EC2 Instance Connect
     ▼
EC2 Instance
```

 This can reduce the need to manage local SSH private keys for interactive console access.

 > EC2 Instance Connect does not mean SSH itself has disappeared. Under the hood, it still uses SSH-based connectivity for supported instances.

---

 # 🔒 Linux File Permissions

 Linux has permissions for:

```
Owner
Group
Others
```

 Example:

```
ls -l vins.pem
```

 You may see:

```
-r-------- 1 user user 1674 vins.pem
```

---

 ## `chmod`

 `chmod` is used to modify file and directory permissions.

 For an SSH private key:

```
chmod 400 vins.pem
```

 This means the owner has read permission, while group and others have no permissions.

 Conceptually:

```
Owner  → Read
Group  → None
Others → None
```

 This is commonly used because SSH clients may reject private-key files that are accessible by other users.

---

 # 🆔 ARN — Amazon Resource Name

 ARN stands for:

 > **Amazon Resource Name**

 ARNs uniquely identify many AWS resources.

 A generalized ARN looks like:

```
arn:partition:service:region:account-id:resource
```

 Example:

```
arn:aws:s3:::my-bucket
```

 Another example conceptually:

```
arn:aws:ec2:region:account-id:instance/instance-id
```

 ARNs are especially important when writing IAM policies.

 Example:

```
IAM Policy
    │
    ├── Action
    │
    └── Resource → ARN
```

---

 # 👤 IAM — Identity and Access Management

 IAM controls who can access AWS resources and what they are allowed to do.

 The major IAM concepts are:

```
                     IAM
                      │
       ┌──────────────┼──────────────┐
       │              │              │
     Users          Groups          Roles
       │              │              │
       └──────────────┼──────────────┘
                      │
                   Policies
```

 ## IAM Users

 An IAM user represents an identity that can have AWS permissions.

 ## IAM Groups

 Groups allow permissions to be assigned to multiple users.

 Example:

```
Developers Group
       │
       ├── Alice
       ├── Bob
       └── Charlie
```

 ## IAM Roles

 A role is an identity that can be assumed by trusted entities.

 Roles are commonly used by:

 - EC2
- Lambda
- ECS
- Other AWS services
- Federated users
- Cross-account access

 For example:

```
EC2
 │
 │ Assume IAM Role
 ▼
IAM Role
 │
 ▼
Permissions
 │
 ▼
S3
```

 This is generally preferable to putting long-term AWS access keys directly on an EC2 instance.

---

 # 📜 IAM Policies

 IAM policies define permissions.

 Conceptually:

```
WHO
 │
 ▼
CAN DO WHAT
 │
 ▼
ON WHICH RESOURCE
```

 A policy can define:

 - `Effect`
- `Action`
- `Resource`
- Conditions

 Example structure:

```
{
  "Effect": "Allow",
  "Action": "s3:GetObject",
  "Resource": "arn:aws:s3:::example-bucket/*"
}
```

 The principle of **least privilege** means granting only the permissions required for a particular task.

---

 # 🔢 TOTP — Time-Based One-Time Password

 TOTP stands for:

 > **Time-Based One-Time Password**

 A TOTP code is generated based on a shared secret and the current time.

 Conceptually:

```
Password
   +
TOTP Code
   ↓
Authentication
```

 The code changes periodically.

 TOTP is commonly used as a second authentication factor.

---

 # 🔑 STS — Security Token Service

 AWS STS provides **temporary security credentials**.

 Temporary credentials typically include:

```
Access Key ID
Secret Access Key
Session Token
```

 Conceptually:

```
Identity / Role
      │
      ▼
     STS
      │
      ▼
Temporary Credentials
      │
      ▼
AWS Resources
```

 Temporary credentials are useful for:

 - IAM roles
- Federated access
- Cross-account access
- Temporary permissions

---

 # 🔐 RSA

 RSA stands for:

 > **Rivest–Shamir–Adleman**

 RSA is an asymmetric cryptographic algorithm.

 It uses:

```
Public Key
     +
Private Key
```

 Unlike symmetric cryptography, asymmetric cryptography uses different keys for related cryptographic operations.

 RSA can be used for:

 - Encryption/decryption
- Digital signatures

 For digital signatures:

```
Private Key
     │
     ▼
   Sign
     │
     ▼
Message + Signature
     │
     ▼
Public Key verifies
```

 > The simplified statement "public key encrypts and private key decrypts" is only one way of describing asymmetric encryption. RSA is also widely used for signatures.

---

 # 🪣 S3 — Simple Storage Service

 Amazon S3 is AWS's **object storage service**.

 The basic structure is:

```
S3
│
└── Bucket
     │
     ├── Object
     ├── Object
     ├── Object
     └── Object
```

 An S3 object can be thought of as:

```
Object
├── Data
├── Key
└── Metadata
```

---

 # 📦 Why is S3 Called Object Storage?

 This was one of the questions in my original notes:

 > **How does S3 store files, and why is it called object storage and not file storage?**

 Traditional file storage generally looks like:

```
File System
│
├── directory
│    ├── file.txt
│    └── image.png
│
└── another-directory
     └── data.json
```

 S3 instead uses:

```
Bucket
│
├── object
├── object
└── object
```

 Each object has a **key**.

 For example:

```
Bucket:
my-application-data

Object Key:
users/vinod/profile.json
```

 The `/` makes the key look like a directory structure:

```
users/
   └── vinod/
        └── profile.json
```

 But S3 fundamentally uses an **object/key model**, not a traditional hierarchical filesystem model.

 Therefore:

```
S3 = Object Storage
```

---

 # 🔐 S3 Encryption

 S3 supports encryption for objects at rest.

 Common server-side encryption approaches include:

 - SSE-S3
- SSE-KMS
- SSE-C

 Conceptually:

```
Original File
     │
     ▼
Encryption
     │
     ▼
Encrypted Object
     │
     ▼
     S3
```

---

 ## Client-Side Encryption

 Your original notes also mention:

 > "We can also encrypt the file and upload it if we don't trust AWS."

 More precisely, this is called **client-side encryption**.

 The application encrypts the data **before** sending it to S3.

```
Application
     │
     │ Encrypt
     ▼
Encrypted Data
     │
     │ Upload
     ▼
S3
```

 This provides an additional layer of control because the plaintext data is not uploaded to S3.

---

 # 🔗 S3 Pre-Signed URLs

 An S3 pre-signed URL provides temporary access to an S3 object.

 For example, imagine an application where users need to download private documents.

 Instead of making the bucket public:

```
User
 │
 ▼
Application
 │
 │ Generate pre-signed URL
 ▼
Temporary URL
 │
 ▼
S3
```

 The URL can be configured to expire after a certain period.

 Typical use cases:

 - Temporary downloads
- Temporary uploads
- Private files
- User-generated content
- Large file transfers

---

 # 🗄️ RDS — Relational Database Service

 Amazon RDS is a managed relational database service.

 Instead of manually managing the database server, AWS manages many infrastructure and operational tasks for you.

```
Application
     │
     ▼
    RDS
     │
     ▼
Relational Database
```

---

 # 🗃️ Supported Database Engines

 RDS supports several relational database engines, including:

 - PostgreSQL
- MySQL
- MariaDB
- Oracle
- Microsoft SQL Server
- Amazon Aurora

 So instead of:

```
EC2
 │
 └── Install PostgreSQL
       │
       ├── Manage OS
       ├── Manage DB
       ├── Manage backups
       └── Manage patches
```

 you can use:

```
RDS
 │
 └── PostgreSQL
```

 and AWS handles many of the underlying management tasks.

---

 # 🛡️ RDS Multi-AZ

 One of the original notes said:

 > "Each RDS has another replica in another region known as a standby instance."

 This needs an important correction.

 ### Multi-AZ is generally within the same AWS Region.

 A simplified architecture is:

```
                    AWS Region
                        │
             ┌──────────┴──────────┐
             │                     │
          AZ-A                  AZ-B
             │                     │
       ┌─────▼─────┐        ┌──────▼─────┐
       │  Primary  │        │  Standby   │
       │    RDS    │◄──────►│    RDS     │
       └───────────┘        └────────────┘
```

 The standby exists primarily for:

 - High availability
- Automatic failover
- Infrastructure failure protection

 The standby is not normally used to serve application read traffic.

---

 # 🔄 RDS Failover

 If the primary database becomes unavailable:

```
Before:

Application
     │
     ▼
Primary DB
     │
     ▼
Standby

Failure:

Application
     │
     ▼
Primary ❌

After Failover:

Application
     │
     ▼
Standby
     │
     ▼
New Primary
```

 The exact failover behavior and timing depend on the RDS configuration and failure scenario.

---

 # 📖 RDS Read Replicas

 Read replicas are primarily used for **read scaling**.

 Imagine:

```
Application
     │
     ├─────────────── Write ──────────────► Primary
     │
     ├────────────── Read ────────────────► Replica 1
     │
     └────────────── Read ────────────────► Replica 2
```

 This is useful when an application is **read-heavy**.

 For example:

```
100,000 requests

90,000 → READ
10,000 → WRITE
```

 Instead of making one database handle all read traffic:

```
             Primary DB
                 ▲
                 │
          100,000 requests
```

 Read replicas can help distribute read workloads.

---

 # ⚖️ Multi-AZ vs Read Replica

 This distinction is extremely important.

 | Feature | Multi-AZ | Read Replica |
| --- | --- | --- |
| Primary purpose | High availability | Read scaling |
| Main concern | Failover | Read workload |
| Standby receives normal application reads? | No | Yes |
| Can have multiple replicas? | Architecture-dependent | Yes |
| Can be cross-region? | Typically same Region for standard Multi-AZ | Supported for certain engines/configurations |
| Application normally connects to standby? | No | Yes |

Think:

```
Multi-AZ
   ↓
"What happens if my DB fails?"
```

 versus:

```
Read Replica
   ↓
"How can I handle more reads?"
```

---

 # 📸 RDS Snapshots

 An RDS snapshot is a point-in-time backup of a database instance.

 Conceptually:

```
RDS
 │
 │ Create Snapshot
 ▼
Snapshot
 │
 │ Restore
 ▼
New RDS Database
```

 Snapshots can be used for:

 - Backup
- Restoration
- Creating test databases
- Creating copies
- Disaster recovery workflows

---

 # ⚡ What is IOPS?

 IOPS means:

 > **Input/Output Operations Per Second**

 It measures how many input/output operations storage can perform per second.

 A database constantly performs operations such as:

```
READ
WRITE
READ
READ
WRITE
READ
...
```

 If a workload performs a large number of storage operations, storage I/O performance becomes important.

 Conceptually:

```
Low IOPS
   ↓
Fewer I/O operations per second

High IOPS
   ↓
More I/O operations per second
```

 The achievable IOPS depends on the selected RDS storage type, configuration, and instance capabilities.

---

 # ❓ How does RDS provide higher IOPS?

 RDS storage performance depends on the storage technology and configuration.

 For workloads requiring high I/O performance, AWS provides storage options with configurable/provisioned performance characteristics.

 The general idea is:

```
Application
     │
     ▼
    RDS
     │
     ▼
High-performance Storage
     │
     ▼
More I/O Operations
```

 Therefore, **IOPS is primarily a storage-performance concept**, not something achieved simply because RDS is a managed service.

---

 # ⚖️ Load Balancing

 A load balancer distributes incoming traffic across multiple targets.

 Without a load balancer:

```
User
 │
 ▼
EC2 Instance
```

 Problem:

```
              ┌── EC2 ❌
              │
Users ────────┤
              │
              └── One server
```

 With a load balancer:

```
                  ┌── EC2 #1
                  │
Users → ALB ──────┼── EC2 #2
                  │
                  └── EC2 #3
```

 Advantages include:

 - Traffic distribution
- Health checks
- High availability
- Horizontal scaling
- Fault isolation

---

 # 🚦 Application Load Balancer — ALB

 An **Application Load Balancer** operates at the application layer and is designed primarily for HTTP/HTTPS traffic.

 Example:

```
                         Internet
                            │
                            ▼
                     ┌────────────┐
                     │    ALB     │
                     └─────┬──────┘
                           │
             ┌─────────────┼─────────────┐
             │             │             │
             ▼             ▼             ▼
           EC2-1         EC2-2         EC2-3
```

 The ALB receives the request and decides which target should handle it.

---

 # 🎯 Target Groups

 A **Target Group** is a logical grouping of targets behind a load balancer.

 This corresponds directly to the original note:

 > "There is something called Target Groups."

 For example:

```
                       ALB
                        │
             ┌──────────┴──────────┐
             │                     │
             ▼                     ▼
       Order Target Group    Customer Target Group
             │                     │
        ┌────┴────┐           ┌────┴────┐
        ▼         ▼           ▼         ▼
      EC2-1     EC2-2       EC2-3     EC2-4
```

 A target group can contain targets that provide the same application/service.

 For example:

```
Order Target Group
├── Order Service Instance 1
└── Order Service Instance 2
```

---

 # ❤️ Health Checks

 One of the most important features of a target group is the **health check**.

 Suppose our application exposes:

```
GET /health
```

 The endpoint might return:

```
HTTP 200 OK
```

 The ALB periodically checks the target.

```
ALB
 │
 ├── GET /health → EC2-1 → 200 → Healthy
 │
 ├── GET /health → EC2-2 → 200 → Healthy
 │
 └── GET /health → EC2-3 → Failure → Unhealthy
```

 If a target becomes unhealthy, the load balancer can stop routing new requests to it.

---

 ## Health Check Configuration

 A target group's health check can contain settings such as:

```
Protocol
Port
Path
Interval
Timeout
Healthy Threshold
Unhealthy Threshold
Success Codes
```

 For example:

```
Path:
 /health

Interval:
 10 seconds

Success:
 HTTP 200

Healthy Threshold:
 3 consecutive successes
```

 The exact values are configurable.

---

 # 🛣️ Path-Based Routing

 ALB supports path-based routing.

 Suppose we have:

```
/orders
/customers
/payments
```

 We can create:

```
                       ALB
                        │
             ┌──────────┼──────────┐
             │          │          │
             ▼          ▼          ▼
          /orders   /customers  /payments
             │          │          │
             ▼          ▼          ▼
           Orders    Customers   Payments
          Target       Target      Target
          Group        Group       Group
```

 Example:

```
GET /orders/123
```

 goes to:

```
Order Target Group
```

 while:

```
GET /customers/123
```

 goes to:

```
Customer Target Group
```

 This is particularly useful for microservice architectures.

---

 # 🔀 Host-Based Routing

 ALB can also route based on hostnames.

 For example:

```
orders.example.com
        │
        ▼
Order Target Group
```

 and:

```
customers.example.com
        │
        ▼
Customer Target Group
```

 So routing can be based on:

```
Path
  +
Host
```

 depending on the listener rules.

---

 # 🔐 Security Groups with ALB

 Suppose the architecture is:

```
Internet
   │
   │ HTTP/HTTPS
   ▼
 ALB
   │
   │ Application Traffic
   ▼
 EC2
```

 A good security-group design is:

```
Internet
   │
   ▼
ALB Security Group
   │
   ▼
EC2 Security Group
```

 The ALB security group can allow inbound:

```
80
443
```

 from the appropriate clients.

 The EC2 security group can allow the application port **from the ALB security group** rather than allowing the whole internet.

 Conceptually:

```
ALB SG
  │
  │ allowed
  ▼
EC2 SG
```

 This creates a better security boundary.

---
# Listener Rules

 # 🚦 ALB vs NLB vs GWLB

 AWS provides multiple load-balancing types.

 | Type | Typical Layer | Main Purpose |
| --- | --- | --- |
| **ALB** | Layer 7 | HTTP/HTTPS applications |
| **NLB** | Layer 4 | TCP/UDP/TLS and high-performance network traffic |
| **GWLB** | Network appliance integration | Firewalls and security/network appliances |

---

 ## Application Load Balancer

 ALB is useful when you need application-aware routing.

 Examples:

```
/orders
/customers
/payments
```

 or:

```
orders.example.com
customers.example.com
```

---

 ## Network Load Balancer

 NLB operates at the network/transport level and is designed for workloads such as:

```
TCP
UDP
TLS
```

 It is useful when very high network performance and low latency are important.

---

 ## Gateway Load Balancer

 GWLB is designed primarily for deploying, scaling, and managing virtual network appliances such as:

```
Firewalls
Intrusion detection systems
Security appliances
Network inspection appliances
```

 Conceptually:

```
Traffic
   │
   ▼
GWLB
   │
   ▼
Security Appliance
   │
   ▼
Application
```

---

 # 🐳 Docker Port Mapping

 Docker containers have their own networking environment.

 Suppose Nginx is listening inside a container on:

```
Port 80
```

 We can expose that port on the host using:

```
sudo docker run -p 80:80 nginx
```

 The format is:

```
-p HOST_PORT:CONTAINER_PORT
```

 Therefore:

```
-p 80:80
```

 means:

```
Host Port 80
      │
      ▼
Container Port 80
```

---

 ## Example

```
docker run -p 8080:80 nginx
```

 means:

```
Browser
   │
   │ localhost:8080
   ▼
Host Port 8080
   │
   ▼
Container Port 80
   │
   ▼
Nginx
```

 Therefore:

```
http://localhost:8080
```

 can reach Nginx listening on port `80` inside the container.

---

 # 🌐 Common Ports

 | Port | Protocol | Common Use |
| --- | --- | --- |
| **22** | SSH | Secure shell |
| **80** | HTTP | Web traffic |
| **443** | HTTPS | Secure web traffic |
| **3306** | MySQL | MySQL |
| **5432** | PostgreSQL | PostgreSQL |
| **6379** | Redis | Redis |
| **8080** | HTTP alternative | Common application/dev port |

---

 # 🏗️ Example AWS Architecture

 Putting the concepts together:

```
flowchart TB

    User[User / Client]

    User --> ALB[Application Load Balancer]

    ALB --> OrderTG[Order Target Group]
    ALB --> CustomerTG[Customer Target Group]

    subgraph Region[AWS Region]
        subgraph AZ1[Availability Zone A]
            Order1[Order EC2]
            Customer1[Customer EC2]
        end

        subgraph AZ2[Availability Zone B]
            Order2[Order EC2]
            Customer2[Customer EC2]
        end

        OrderTG --> Order1
        OrderTG --> Order2

        CustomerTG --> Customer1
        CustomerTG --> Customer2

        Order1 --> RDSPrimary[(RDS Primary)]
        Order2 --> RDSPrimary
        Customer1 --> RDSPrimary
        Customer2 --> RDSPrimary

        RDSPrimary -. Multi-AZ .-> RDSStandby[(RDS Standby)]
    end
```

 This architecture demonstrates:

 - AWS Region
- Multiple Availability Zones
- ALB
- Target Groups
- EC2
- Health checks
- RDS
- Multi-AZ
- High availability

---

 # 🔄 Complete Request Flow

 Suppose a user requests:

```
https://example.com/orders/123
```

 The request could conceptually flow like this:

```
User
 │
 ▼
DNS
 │
 ▼
ALB
 │
 │ Path = /orders/*
 ▼
Order Target Group
 │
 ├── EC2 Instance A
 │
 └── EC2 Instance B
        │
        ▼
      RDS
```

 The ALB checks the health of the EC2 instances.

```
Healthy → Receive traffic
Unhealthy → Do not receive new traffic
```

---

 # 🔐 Security Architecture

 A simple secure architecture could look like:

```
                  Internet
                     │
                     ▼
              ┌────────────┐
              │    ALB     │
              └─────┬──────┘
                    │
              ALB Security Group
                    │
                    ▼
              ┌────────────┐
              │    EC2     │
              └─────┬──────┘
                    │
              EC2 Security Group
                    │
                    ▼
              ┌────────────┐
              │    RDS     │
              └────────────┘
```

 The basic principle is:

```
Internet
   │
   ▼
ALB
   │
   ▼
Application
   │
   ▼
Database
```

 rather than exposing every component directly to the internet.

---

 # 🧠 Important Questions

 ## 1\. Why is S3 object storage?

 Because S3 stores data as objects identified by keys inside buckets rather than using a traditional hierarchical filesystem model.

```
Bucket
  │
  ├── Object
  ├── Object
  └── Object
```

---

 ## 2\. Can I encrypt a file before uploading it to S3?

 Yes.

 This is commonly referred to as **client-side encryption**.

```
File
 │
 ▼
Encrypt locally
 │
 ▼
Encrypted file
 │
 ▼
S3
```

---

 ## 3\. What is an S3 pre-signed URL?

 A temporary URL that grants access to a specific S3 operation/object according to the permissions and expiration configured when the URL is generated.

 Useful for:

 - Temporary downloads
- Temporary uploads
- Private files

---

 ## 4\. What is the difference between Multi-AZ and Read Replica?

```
Multi-AZ
    ↓
High Availability
    ↓
Failover
```

 versus:

```
Read Replica
    ↓
Read Scaling
    ↓
More Read Capacity
```

---

 ## 5\. Does the RDS Multi-AZ standby handle read traffic?

 Generally, no.

 The standby exists primarily for high availability and failover.

 Read replicas are the mechanism designed for read scaling.

---

 ## 6\. How does an ALB know whether an EC2 instance is healthy?

 The ALB uses the target group's configured health check.

 For example:

```
GET /health
```

 If the response matches the configured success criteria, the target can be considered healthy.

---

 ## 7\. Why do we need Target Groups?

 Because the ALB needs a logical group of targets to route traffic to.

 For example:

```
ALB
 │
 ├── /orders → Order Target Group
 │
 └── /customers → Customer Target Group
```

---

 ## 8\. Why do we use health-check endpoints?

 An endpoint such as:

```
/health
```

 provides a simple way for infrastructure to determine whether the application is able to receive traffic.

 A health endpoint might return:

```
HTTP/1.1 200 OK
```

 when the service is healthy.

---

 ## 9\. What is port mapping in Docker?

 The syntax:

```
-p HOST_PORT:CONTAINER_PORT
```

 maps a host port to a container port.

 Example:

```
docker run -p 8080:80 nginx
```

 means:

```
localhost:8080
      ↓
container:80
```

---

 ## 10\. Why is SSH port 22?

 SSH conventionally listens on TCP port `22`.

```
SSH → 22
```

 However, a server administrator can configure SSH to listen on another port.

 The important concept is that **port numbers are conventions/configuration, not intrinsic properties of the protocol**.

---

 ## 11\. Why is HTTP port 80?

 HTTP conventionally uses:

```
HTTP → 80
```

 HTTPS conventionally uses:

```
HTTPS → 443
```

---

 # ❓ Why can't resource names start or end with `-`?

 This depends on the AWS service.

 There is no single naming rule shared by every AWS service.

 Many AWS resource names follow DNS-style or identifier restrictions where:

```
my-application
```

 is valid, while:

```
-my-application
my-application-
```

 may not be.

 The reason is generally related to the syntax/constraints of the particular identifier being used.

 Therefore, don't assume:

```
AWS rule = no leading/trailing hyphen everywhere
```

 Instead:

 > **Always check the naming requirements of the specific AWS service.**

---

 # 🧩 Important AWS Concepts to Connect

 The individual services become much easier to understand when connected together.

 ## Compute

```
EC2
 ↓
Runs application
```

 ## Storage

```
S3
 ↓
Stores objects/files
```

 ## Database

```
RDS
 ↓
Stores relational application data
```

 ## Identity

```
IAM
 ↓
Controls access
```

 ## Load Balancing

```
ALB
 ↓
Distributes application traffic
```

 ## Networking

```
VPC
 ↓
Provides network isolation
```

---

 # 🏢 Simple Production Architecture

 A more realistic architecture might look like:

 Mermaid flowchart: Users, DNS, Application Load Balancer, EC2 - AZ A, EC2 - AZ B, (RDS), (S3), IAM Roles

Here:

```
Route 53
    ↓
ALB
    ↓
EC2
    ↓
┌───────────────┐
│               │
RDS             S3
│               │
Database        Objects
```

---

 # 🧱 High Availability Concept

 A single server creates a potential **Single Point of Failure (SPOF)**.

```
User
 │
 ▼
EC2 ❌
```

 If the EC2 instance fails, the application may become unavailable.

 Instead:

```
                 ALB
                  │
          ┌───────┴───────┐
          │               │
        EC2-A           EC2-B
       AZ-A             AZ-B
```

 If one instance fails:

```
EC2-A ❌

       ALB
        │
        ▼
      EC2-B
```

 The application can continue serving traffic through the healthy target, assuming the application is designed for this architecture.

---

 # 🛡️ Single Point of Failure

 A **Single Point of Failure (SPOF)** is a component whose failure can cause the system to fail.

 Bad:

```
             Application
                  │
                  ▼
             One EC2
                  │
                  ▼
             One Database
```

 Better:

```
              ALB
             /   \
           EC2   EC2
            \     /
             \   /
              RDS
             /   \
        Primary  Standby
```

 The exact architecture required depends on the application's availability requirements.

---

 # 📝 Important Corrections from My Original Notes

 This section is intentionally included so that incorrect assumptions don't get carried forward.

 ### ❌ "RDS has another replica in another Region by default"

 Not correct.

 ### ✅ Correct concept

 RDS Multi-AZ generally places the standby in another Availability Zone **within the same Region**.

 Cross-region replication is a separate architecture.

---

 ### ❌ "`.ppk` for Windows and `.pem` for Mac"

 Oversimplified.

 ### ✅ Correct concept

 `.pem` is commonly used with OpenSSH, while `.ppk` is commonly associated with PuTTY.

 The choice is based on the tool/client being used rather than simply the operating system.

---

 ### ❌ "Multi-AZ standby is a read replica"

 Not correct.

 ### ✅ Correct concept

```
Multi-AZ → High Availability / Failover

Read Replica → Read Scaling
```

---

 ### ❌ "ALB simply sends traffic to instances"

 Incomplete.

 ### ✅ Correct concept

 ALB can use:

 - Listener rules
- Target groups
- Health checks
- Path-based routing
- Host-based routing
- Other routing conditions

 to determine where traffic should go.

---

 # 🧠 AWS CloudFront (CDN - Content Delivery Network)

 We have Region >> AZ >> LocalZone >> Edge

 Now, the edge location (Data centers) is the nearest possible location where the CDN exists to avoid network latency.

 Edge locations cache the content closer to the end users so the round trip becomes extremely short.

 # CloudFront vs Edge Locations
 > Edge Locations: They are physical site/ Infras
 > CloudFront: A service that uses these edge locations under the hood to cache and deliver our content. Using these, we ensure what to cache and how long it should be cached.

CloudFront has something called a CloudFront distribution, a configuration, and we can create an Origin Access Control policy (a bucket policy that can be configured in a way such that the bucket remains private and only this CDN can access S3) for secure S3 access.

> CloudFront is not a region resource but a global resource.
> We need to provide the origin type, and it is where our content, such as a website or an app, lives. CloudFront works with AWS-based origins and origins hosted on other cloud providers.
> Let us say a file got cached in one CDN that was serving us, but some new changes have been pushed to that file, and if our earlier-used CDN serves the file, then it will serve it without the latest changes until the TTL expires for the file. When we send the request, it does not go to the closest edge location based on distance; it goes to the nearest edge location/CDN based on the lowest network latency. This is what we mean by the nearest edge location.

CloudFront provides Cache Invalidation, where we provide the path of the object that we want to clear from the CDN, and once it is done, then the CDN will have to bring the files again from the declared origin in the CDN.
 
---
 # 🧠 AWS Route 53 (DNS - Domain Name System)
 
 We purchase a domain here, and once it is done, go to Certificate Manager and get your registered domain's certificate.
 
---

 # 🧠 Quick Revision

 **EC2 = Elastic Compute Cloud**

 A virtual server in AWS.

```
EC2 → Compute
```
An AMI is a template used to launch EC2 instances.

```
AMI → EC2 Template
```

 Secure remote access protocol.

```
SSH → Port 22
```

Object storage.

```
S3 → Bucket → Object
```

Controls identity and access.

```
IAM → Users / Groups / Roles / Policies
```

Managed relational database service.

```
RDS → Relational Database
```

Primarily for high availability and failover.

```
Multi-AZ → HA / Failover
```

Primarily for read scaling.

```
Read Replica → Read Scaling
```

Application-layer load balancer for HTTP/HTTPS workloads.

```
ALB → Listener → Target Group → Targets
```
Logical collection of targets behind a load balancer.

```
Target Group
├── EC2
├── EC2
└── EC2
```

Used by the load balancer to determine whether a target is healthy.

```
GET /health
     ↓
   200 OK
     ↓
 Healthy
```

Docker Port Mapping
```
-p HOST_PORT:CONTAINER_PORT
```

 Example:

```
docker run -p 8080:80 nginx
```

 \</details\>
---

 # 📊 AWS Cheat Sheet

 | Concept | Remember |
| --- | --- |
| EC2 | Virtual server |
| AMI | EC2 template |
| SSH | Remote secure access |
| SSH Port | 22 |
| HTTP Port | 80 |
| HTTPS Port | 443 |
| ARN | AWS resource identifier |
| IAM | Identity & access management |
| TOTP | Time-based one-time password |
| STS | Temporary credentials |
| S3 | Object storage |
| S3 Bucket | Container for objects |
| S3 Object | Data + key + metadata |
| S3 Pre-signed URL | Temporary object access |
| RDS | Managed relational DB |
| Multi-AZ | High availability/failover |
| Read Replica | Read scaling |
| RDS Snapshot | Backup/restore |
| IOPS | Input/output operations per second |
| ALB | HTTP/HTTPS load balancer |
| Target Group | Group of load-balancer targets |
| Health Check | Determines target health |
| Security Group | Virtual firewall |
| Docker `-p` | Port mapping |

---

 # 🚀 Learning Roadmap

 A useful order for understanding these concepts is:

 Mermaid flowchart: AWS Fundamentals, Regions & Availability Zones, VPC & Networking, EC2, SSH & Security Groups, AMI, Load Balancing, Target Groups, Health Checks, S3, IAM, RDS, Multi-AZ, Read Replicas, Docker, Monitoring & Security

---

 # 📚 Further Reading

 - [AWS Documentation](<https://docs.aws.amazon.com/>)
- [Amazon EC2 Documentation](<https://docs.aws.amazon.com/ec2/>)
- [Amazon S3 Documentation](<https://docs.aws.amazon.com/s3/>)
- [Amazon RDS Documentation](<https://docs.aws.amazon.com/rds/>)
- [Elastic Load Balancing Documentation](<https://docs.aws.amazon.com/elasticloadbalancing/>)
- [AWS IAM Documentation](<https://docs.aws.amazon.com/iam/>)
- [AWS Security Documentation](<https://docs.aws.amazon.com/security/>)

---

 # 🔗 Reference

 One of the resources used while studying RDS concepts:

 aws-cloud-architect-essentials — RDS

---

 # ⭐ Final Mental Model

 If I have to remember the entire AWS architecture at a high level:

```
                         AWS
                          │
                   ┌──────┴──────┐
                   │   Region    │
                   └──────┬──────┘
                          │
              ┌───────────┴───────────┐
              │                       │
             AZ-A                   AZ-B
              │                       │
           ┌──┴──┐                 ┌──┴──┐
           │ EC2 │                 │ EC2 │
           └──┬──┘                 └──┬──┘
              │                       │
              └──────────┬────────────┘
                         │
                        RDS
                         │
                   Multi-AZ
                         │
                      Standby

Internet
   │
   ▼
 ALB
   │
   ▼
Target Groups
   │
   ▼
 EC2
   │
   ├──────────► S3
   │
   └──────────► RDS

IAM
 │
 └── Controls access to AWS resources
```

 ### The core idea:

```
EC2  → Compute
S3   → Object Storage
RDS  → Database
IAM  → Access Control
ALB  → Traffic Distribution
VPC  → Networking
AZ   → Isolation / Availability
AMI  → EC2 Template
ARN  → Resource Identifier
STS  → Temporary Credentials
SSH  → Remote Access
```

 > **AWS is easier to understand when the services are not studied independently. Think about how they work together to build a highly available, secure, and scalable application.**

---

 \<div align="center"\> ### ☁️ Learn → Build → Break → Fix → Repeat

 \</div\>
