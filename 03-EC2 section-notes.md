# EC2 Section Notes —


## 1. What is Amazon EC2?

**EC2 = Elastic Compute Cloud**

EC2 is one of AWS's most popular services. It provides **Infrastructure as a Service (IaaS)**.

Simple meaning:

> **EC2 = renting a virtual computer/server from AWS.**

You can use an EC2 instance to:
- run a website
- run a backend/API
- install applications
- run Linux or Windows
- process workloads

### EC2-related concepts

The EC2 section introduces:

1. **EC2** → virtual machines
2. **EBS** → storage on virtual/network-attached drives
3. **ELB** → distributes load across machines
4. **ASG** → scales the number of EC2 instances

### Real-life example

```text
Users
  |
  v
Load Balancer (ELB)
  |
  +----> EC2-1
  |
  +----> EC2-2
  |
  +----> EC2-3
           ^
           |
          ASG
```

- EC2 = servers
- EBS = storage
- ELB = distributes traffic
- ASG = manages/scales the number of instances

**Exam point:** EC2 is an **IaaS** service.

---

# 2. EC2 Sizing and Configuration

When launching an EC2 instance, you choose its configuration.

Important configuration areas:

### 1. Operating System

Examples:
- Linux
- Windows
- Mac OS

### 2. CPU

CPU determines the processing capability of the instance.

Example:

```text
Simple website      -> lower CPU requirement
Video processing    -> higher CPU requirement
Scientific work    -> very high CPU requirement
```

### 3. RAM

RAM is the working memory of the instance.

Example:

```text
Simple web server   -> moderate RAM
Large database      -> high RAM
```

### 4. Storage

The slides mention:
- EBS / EFS as network-attached storage
- EC2 Instance Store as hardware/local storage

### 5. Network

Network configuration includes:
- network card/performance
- public IP address

### 6. Security Group

Security Groups define firewall rules for the EC2 instance.

### 7. EC2 User Data

User Data is used to automatically run commands during the first startup.

---

# 3. EC2 User Data

**EC2 User Data = a bootstrap script that runs commands when an instance starts.**

Bootstrapping means launching commands when a machine starts.

According to the slides:
- User Data runs only once, at the instance's first start.
- It can install updates.
- It can install software.
- It can download common files from the internet.
- It can automate other boot tasks.
- The User Data script runs with the root user.

### Example

Suppose you want a web server to be installed automatically when an EC2 instance starts:

```text
EC2 Launch
    |
    v
User Data Script
    |
    +--> Update system
    +--> Install software
    +--> Start web server
    |
    v
Web Server Ready
```

**Exam point:** EC2 User Data is a **first-start bootstrap script**.

---

# 4. EC2 Instance Types

AWS provides different EC2 instance types for different workloads.

Example:

```text
m5.2xlarge
```

Break it into three parts:

```text
m       5        2xlarge
|       |           |
class   generation  size
```

- **m** = instance class
- **5** = generation
- **2xlarge** = size within the instance class

---

# 5. General Purpose Instances

General Purpose instances provide a **balance between compute, memory, and networking**.

Use cases include:
- web servers
- code repositories
- general applications

The course uses **t2.micro** as a General Purpose example.

### Easy example

If you need a normal server without a strong CPU-only, RAM-only, or storage-only requirement:

```text
General Purpose
       |
       v
Balanced CPU + RAM + Networking
```

**Exam clue:** **Balanced workload → General Purpose**

---

# 6. Compute Optimized Instances

Compute Optimized instances are designed for **compute-intensive workloads that require high-performance processors**.

Use cases:
- batch processing
- media transcoding
- high-performance web servers
- High Performance Computing (HPC)
- scientific modeling
- machine learning
- dedicated gaming servers

### Easy example

If the main requirement is heavy CPU processing:

```text
Heavy calculations
       |
       v
Compute Optimized
```

**Exam clue:** **CPU-intensive / compute-intensive → Compute Optimized**

---

# 7. Memory Optimized Instances

Memory Optimized instances provide fast performance for workloads that process **large datasets in memory**.

Use cases:
- high-performance relational databases
- high-performance non-relational databases
- distributed web-scale cache stores
- in-memory databases
- Business Intelligence workloads
- real-time processing of large unstructured data

### Easy example

If an application needs to keep a large amount of data in memory for fast processing:

```text
Large dataset
     |
     v
Large memory requirement
     |
     v
Memory Optimized
```

**Exam clue:** **Large data in memory → Memory Optimized**

---

# 8. Storage Optimized Instances

Storage Optimized instances are designed for storage-intensive workloads that require high, sequential read/write access to large datasets on local storage.

Use cases:
- high-frequency OLTP systems
- relational databases
- NoSQL databases
- cache workloads such as Redis
- data warehousing
- distributed file systems

### Easy example

If the workload needs very high storage read/write performance:

```text
Large dataset
     |
     v
High read/write requirement
     |
     v
Storage Optimized
```

**Exam clue:** **High storage read/write → Storage Optimized**

---

# 9. Instance Types — Quick Memory Table

| Workload clue | Instance type |
|---|---|
| Balanced CPU + memory + networking | General Purpose |
| CPU-intensive calculations | Compute Optimized |
| Large datasets in memory | Memory Optimized |
| High storage read/write | Storage Optimized |

### Memory trick

```text
General  = Balanced
Compute  = CPU
Memory   = RAM
Storage  = Read/Write
```

---

# 10. EC2 Instance Size

Different sizes are available within an instance family.

Examples from the slides:

```text
t2.micro
t2.xlarge
```

The slides show:

```text
t2.micro  -> 1 vCPU, 1 GiB memory
t2.xlarge -> 4 vCPU, 16 GiB memory
```

Instance specifications can include:
- vCPU
- memory
- storage
- network performance
- EBS bandwidth

General idea:

```text
Smaller instance
      |
      v
Fewer resources

Larger instance
      |
      v
More resources
```

---

# 11. Security Groups

**Security Group = a firewall for an EC2 instance.**

Security Groups control:
- inbound traffic
- outbound traffic
- access to ports
- authorized IPv4/IPv6 ranges
- rules referencing IP addresses or other Security Groups

### Inbound

Inbound means traffic coming **from outside toward the EC2 instance**.

```text
Internet / User
      |
      | INBOUND
      v
Security Group
      |
      v
    EC2
```

### Outbound

Outbound means traffic going **from the EC2 instance to another destination**.

```text
EC2
 |
 | OUTBOUND
 v
Internet / Other Service
```

---

# 12. Security Group as a Firewall

Suppose an EC2 instance is running a web server.

If HTTP port 80 is allowed:

```text
User
 |
 | HTTP : 80
 v
Security Group
 |
 | ALLOW
 v
EC2 Web Server
```

If port 80 is not allowed:

```text
User
 |
 | HTTP : 80
 v
Security Group
 |
 | BLOCK
 X
EC2
```

The blocked traffic does not reach the EC2 instance.

---

# 13. Important Security Group Rules

According to the slides:

- A Security Group can be attached to multiple EC2 instances.
- A Security Group is locked to a **Region/VPC combination**.
- Security Groups operate outside the EC2 instance; blocked traffic is not seen by the EC2 instance.
- It is good practice to maintain a separate Security Group for SSH access.
- If an application is not accessible and times out, check the Security Group.
- If the application gives **connection refused**, it can indicate an application problem or that the application is not running.
- All inbound traffic is blocked by default.
- All outbound traffic is authorized by default.

### Timeout vs Connection Refused

```text
Timeout
   |
   v
Check Security Group / network rules
```

```text
Connection refused
   |
   v
Check the application/service
```

---

# 14. Security Group Referencing Another Security Group

A Security Group rule can reference another Security Group.

Example:

```text
Load Balancer
Security Group = SG-ALB
       |
       | HTTP
       v
EC2
Security Group = SG-EC2
```

The EC2 Security Group can conceptually say:

```text
Allow HTTP
FROM SG-ALB
```

This is useful because the rule can trust instances associated with the specified Security Group instead of manually maintaining IP addresses.

---

# 15. Important Ports

These ports are important for the exam:

| Port | Protocol | Purpose |
|---:|---|---|
| 22 | SSH | Log into a Linux instance |
| 21 | FTP | File Transfer Protocol |
| 22 | SFTP | Secure file transfer using SSH |
| 80 | HTTP | Unsecured websites |
| 443 | HTTPS | Secured websites |
| 3389 | RDP | Log into a Windows instance |

### Memory trick

```text
22   = SSH
21   = FTP
80   = HTTP
443  = HTTPS
3389 = RDP
```

---

# 16. SSH

**SSH = Secure Shell**

SSH allows you to control a remote EC2 machine using the command line.

Example:

```text
Your Computer
      |
      | SSH : 22
      v
EC2 Linux Instance
```

For SSH access, port **22** must be allowed by the Security Group.

**Exam point:** SSH → **Port 22**

---

# 17. Connecting to EC2 from Windows

The slides discuss:
- PuTTY
- Windows SSH
- EC2 Instance Connect

The main concept is:

```text
Windows Computer
       |
       | SSH
       v
Linux EC2
```

---

# 18. EC2 Instance Connect

**EC2 Instance Connect = connect to an EC2 instance from your browser.**

According to the slides:
- You can connect from the browser.
- You do not need to use the downloaded key file.
- AWS uploads a temporary key to the EC2 instance.
- The slides mention out-of-the-box support with Amazon Linux 2.
- Port 22 must still be open.

### Simple flow

```text
Browser
   |
   v
EC2 Instance Connect
   |
   | Temporary key
   v
EC2 Instance
```

---

# 19. EC2 Purchasing Options

The EC2 purchasing options covered in these pages are:

1. On-Demand Instances
2. Reserved Instances
3. Convertible Reserved Instances
4. Savings Plans
5. Spot Instances
6. Dedicated Hosts
7. Dedicated Instances
8. Capacity Reservations

---

# 20. On-Demand Instances

**On-Demand = use EC2 when needed without a long-term commitment.**

According to the slides:
- Pay for what you use.
- Linux/Windows are billed per second after the first minute.
- Other operating systems are billed per hour.
- It has the highest cost among the compared options.
- There is no upfront payment requirement.
- There is no long-term commitment.
- It is recommended for short-term and unpredictable workloads.

### Example

```text
Need server now
     |
     v
Launch On-Demand
     |
     v
Use it
     |
     v
Pay for usage
```

**Exam clue:** **Short-term + unpredictable + no commitment → On-Demand**

---

# 21. Reserved Instances

Reserved Instances are designed for **long-term, steady workloads**.

According to the slides:
- Reservation period = 1 year or 3 years.
- You reserve specific instance attributes.
- Payment options:
  - No Upfront
  - Partial Upfront
  - All Upfront
- Scope can be Regional or Zonal.
- Recommended for steady-state applications such as databases.
- Reserved Instances can be bought/sold in the Reserved Instance Marketplace.

### Example

If a database will run continuously for a long period:

```text
Database
   |
   | Predictable
   | Long-term
   v
Reserved Instance
```

**Exam clue:** **Steady workload + long-term → Reserved Instance**

---

# 22. Convertible Reserved Instances

Convertible Reserved Instances provide more flexibility than standard Reserved Instances.

According to the slides, you can change:
- instance type
- instance family
- operating system
- scope
- tenancy

Simple idea:

```text
Long-term commitment
        +
More flexibility
        |
        v
Convertible Reserved Instance
```

The exact discount percentage can change over time, so the important exam concept is the **flexibility**, not memorizing the percentage.

---

# 23. Savings Plans

Savings Plans provide a discount based on a long-term usage commitment.

Example from the slides:

```text
Commit to $10/hour
for 1 or 3 years
```

Usage beyond the committed Savings Plan amount is billed at the On-Demand price.

According to the slides, the EC2 Savings Plan is locked to a specific instance family and AWS Region, while allowing flexibility across:
- instance size
- operating system
- tenancy

### Example

```text
Commitment = $10/hour

Actual usage = $8/hour
       |
       v
Covered by commitment

Actual usage = $12/hour
       |
       v
Extra $2 is billed at On-Demand price
```

**Exam clue:** **Commit to a certain amount of usage → Savings Plan**

---

# 24. Spot Instances

**Spot Instances = very low-cost EC2 capacity with interruption risk.**

According to the slides:
- They can provide a very large discount compared with On-Demand.
- You can lose the instance if the current Spot price exceeds your maximum price.
- They are useful for workloads that are resilient to failure.
- Good use cases:
  - batch jobs
  - data analysis
  - image processing
  - distributed workloads
  - workloads with flexible start/end times
- Not suitable for critical jobs or databases.

### Easy example

```text
Spot Instance
      |
      v
Very cheap
      |
      v
Can be interrupted
```

**Exam clue:** **Cheap + can be interrupted + fault-tolerant workload → Spot**

---

# 25. Dedicated Hosts

**Dedicated Host = a physical server with EC2 instance capacity fully dedicated to your use.**

According to the slides, Dedicated Hosts are useful for:
- compliance requirements
- software licensing models
- Bring Your Own License (BYOL)
- per-socket/per-core licensing situations

It is one of the most expensive EC2 options.

### Simple picture

Normal shared physical host:

```text
Physical Host
 + EC2
 + EC2
 + EC2
```

Dedicated Host:

```text
Physical Host
 + Your EC2 capacity
```

**Exam clue:** **Entire physical host + licensing/compliance → Dedicated Host**

---

# 26. Dedicated Instances

Dedicated Instances run on hardware dedicated to your use.

Important difference from Dedicated Hosts:

- The hardware is dedicated to your account.
- Instances may share hardware with other instances in the same account.
- You do not have control over instance placement.
- Hardware can change after Stop/Start.

### Remember

```text
Dedicated Host
= Physical host dedicated to you
+ instance placement control

Dedicated Instance
= Dedicated hardware
+ no placement control
```

---

# 27. Capacity Reservations

**Capacity Reservation = reserve On-Demand EC2 capacity in a specific Availability Zone.**

According to the slides:
- Capacity is reserved in a specific AZ.
- There is no time commitment.
- You can create/cancel it at any time.
- There is no billing discount.
- You are charged at the On-Demand rate whether you run instances or not.
- It is suitable for short-term, uninterrupted workloads that need capacity in a specific AZ.

### Example

Suppose you need guaranteed EC2 capacity in:

```text
Availability Zone
       |
       v
     AZ-a
```

You can use:

```text
Capacity Reservation
       |
       v
Reserve EC2 capacity in that AZ
```

**Exam clue:** **Reserve capacity in a specific AZ → Capacity Reservation**

---

# 28. Purchasing Options — Quick Comparison

| Option | Main idea | Best clue |
|---|---|---|
| On-Demand | No long-term commitment | Short/unpredictable workload |
| Reserved Instance | Long-term reservation | Steady workload |
| Convertible RI | Long-term + flexibility | Need to change attributes |
| Savings Plan | Usage commitment | Commit to $/hour usage |
| Spot | Very cheap but interruptible | Fault-tolerant workload |
| Dedicated Host | Entire physical host | Licensing/compliance |
| Dedicated Instance | Dedicated hardware | No placement control |
| Capacity Reservation | Reserve capacity | Specific AZ |

---

# 29. Hotel Analogy for Purchasing Options

The slides use a hotel analogy. Use it to remember the concepts:

### On-Demand

```text
Come to the hotel whenever you want.
Pay the full price.
No long-term commitment.
```

### Reserved

```text
Plan ahead for a long stay.
Get a discount.
```

### Savings Plans

```text
Commit to a certain amount per hour
for a period.
```

### Spot

```text
Get a cheap empty room,
but you can be asked to leave.
```

### Dedicated Host

```text
Book the entire building.
```

### Capacity Reservation

```text
Reserve a room for a period.
Pay the full price even if you do not stay.
```

---

# 30. Price Comparison

The slides provide an example using **m4.large in us-east-1**.

The important concept is:

```text
On-Demand
     |
     v
Higher/full price

Reserved / Savings Plans
     |
     v
Long-term commitment
     |
     v
Lower effective cost

Spot
     |
     v
Very low cost
     |
     v
Interruption risk
```

**Exam tip:** Do not focus on memorizing the exact dollar amounts from this slide. The slides themselves note that discount percentages change over time.

---

# 31. EC2 Shared Responsibility Model

Security in EC2 is shared between AWS and the customer.

## AWS responsibilities

According to the slides, AWS is responsible for:
- infrastructure
- global network security
- isolation on physical hosts
- replacing faulty hardware
- compliance validation

Simple view:

```text
AWS
 |
 +--> Physical infrastructure
 +--> Physical hardware
 +--> Global infrastructure security
 +--> Hardware replacement
```

## Customer responsibilities

According to the slides, the customer is responsible for:
- Security Group rules
- operating-system patches and updates
- software and utilities installed on EC2
- IAM Roles assigned to EC2
- IAM user access management
- data security on the instance

Simple view:

```text
Customer
 |
 +--> Security Groups
 +--> OS patches
 +--> Installed software
 +--> IAM access
 +--> Data security
```

### Easy rule

```text
AWS
= Security OF the cloud

Customer
= Security IN the cloud
```

---

# 32. EC2 Complete Architecture

Think of the EC2 section as one complete system:

```text
                         Internet
                            |
                            v
                    +----------------+
                    | Security Group |
                    | Firewall Rules |
                    +----------------+
                            |
                            v
                    +----------------+
                    |   EC2 Instance |
                    |----------------|
                    | OS / AMI       |
                    | CPU            |
                    | RAM            |
                    | Application    |
                    +----------------+
                       |          |
                       |          |
                       v          v
                     EBS       User Data
                   Storage      Script

        Multiple EC2 instances
                 |
                 v
               ELB
          Load Distribution
                 |
                 v
                ASG
          Scaling EC2 count
```

Note: ELB, EBS, and ASG are introduced in the EC2 overview; deeper storage/load-balancing/auto-scaling topics are covered elsewhere in the course.

---

# 33. Think of EC2 as a Normal Computer

EC2 is easiest to understand as a rented virtual computer.

```text
Normal Computer
----------------
CPU
RAM
Storage
OS
Network
Firewall
Startup programs

EC2
----------------
vCPU
RAM
EBS / Instance Store
AMI / OS
Network
Security Group
User Data
```

This is a useful mental model for the entire EC2 section.

---

# 34. EC2 Launch Formula

The final EC2 summary can be remembered as:

```text
EC2 Instance
=
AMI / OS
+
Instance Size
+
Storage
+
Security Group
+
EC2 User Data
```

### Example

You want to launch a Linux web server:

```text
AMI
= Linux

Instance Size
= suitable General Purpose size

Storage
= EBS

Security Group
= allow SSH 22
  allow HTTP 80

User Data
= install/start web server
```

Result:

```text
Internet
   |
   | HTTP 80
   v
Security Group
   |
   v
Linux EC2
   |
   v
Web Server
```

---

# 35. Inbound vs Outbound

## Inbound

**Outside → EC2**

Example:

```text
Your Laptop
    |
    | SSH : 22
    v
EC2
```

Another example:

```text
User Browser
    |
    | HTTPS : 443
    v
EC2 Web Server
```

## Outbound

**EC2 → Outside**

Example:

```text
EC2
 |
 | HTTPS : 443
 v
Internet
```

For example, an EC2 application may make an HTTPS request to an external service.

---

# 36. Common Exam Questions

### Q1. EC2 belongs to which cloud service model?

**Answer: IaaS**

### Q2. What is EC2?

**Answer:** Elastic Compute Cloud, an AWS service for running virtual machines.

### Q3. What acts as a firewall for an EC2 instance?

**Answer: Security Group**

### Q4. Which port is SSH?

**Answer: 22**

### Q5. Which port is HTTP?

**Answer: 80**

### Q6. Which port is HTTPS?

**Answer: 443**

### Q7. Which port is RDP?

**Answer: 3389**

### Q8. Which EC2 type is good for CPU-intensive workloads?

**Answer: Compute Optimized**

### Q9. Which EC2 type is good for large datasets in memory?

**Answer: Memory Optimized**

### Q10. Which EC2 type is good for high storage read/write workloads?

**Answer: Storage Optimized**

### Q11. Which EC2 type balances compute, memory, and networking?

**Answer: General Purpose**

### Q12. Which purchasing option is best for short, unpredictable workloads with no commitment?

**Answer: On-Demand**

### Q13. Which purchasing option is best for steady, long-term workloads?

**Answer: Reserved Instances**

### Q14. Which option is cheap but can be interrupted?

**Answer: Spot Instances**

### Q15. Which option is useful for complicated software licensing?

**Answer: Dedicated Host**

### Q16. Which option reserves capacity in a specific Availability Zone?

**Answer: Capacity Reservation**

### Q17. What automatically runs during the first start of an EC2 instance?

**Answer: EC2 User Data**

### Q18. What is SSH used for?

**Answer:** Secure remote command-line access to a Linux EC2 instance.

### Q19. What happens when a Security Group blocks traffic?

**Answer:** The blocked traffic does not reach the EC2 instance.

### Q20. What is the difference between timeout and connection refused?

```text
Timeout
= Check Security Group / network rules

Connection refused
= Check the application/service
```

---

# 37. Most Important Exam Keywords

```text
EC2
= IaaS / Virtual Machine

AMI
= Image/OS used to launch an instance

User Data
= First-start bootstrap script

Security Group
= EC2 firewall

SSH
= Port 22

HTTP
= Port 80

HTTPS
= Port 443

RDP
= Port 3389

General Purpose
= Balanced

Compute Optimized
= CPU-intensive

Memory Optimized
= Large data in memory

Storage Optimized
= High storage I/O

On-Demand
= No commitment

Reserved
= Long-term steady workload

Convertible Reserved
= Long-term + flexibility

Savings Plans
= Usage commitment

Spot
= Cheap + interruptible

Dedicated Host
= Entire physical host

Dedicated Instance
= Dedicated hardware, no placement control

Capacity Reservation
= Capacity in a specific AZ
```

---

# 38. One-Page Revision

```text
                    AMAZON EC2
                        |
        +---------------+----------------+
        |               |                |
     Virtual          Storage         Scaling/
     Machine            EBS           Load
        |                                |
        |                               ELB
        |                               ASG
        |
   +----+---------------------+
   |                          |
Configuration             Security
   |                          |
OS / AMI                 Security Group
CPU                      Inbound
RAM                      Outbound
Storage                  Ports
Network                  IP / SG rules
User Data
```

### Instance Types

```text
General Purpose   -> Balanced
Compute Optimized -> CPU
Memory Optimized  -> RAM / Memory
Storage Optimized -> Storage I/O
```

### Ports

```text
22   SSH
21   FTP
80   HTTP
443  HTTPS
3389 RDP
```

### Purchasing

```text
On-Demand
   -> Short / unpredictable

Reserved
   -> Long-term steady workload

Convertible RI
   -> Long-term + flexible

Savings Plan
   -> Usage commitment

Spot
   -> Cheap + can be interrupted

Dedicated Host
   -> Entire physical server

Dedicated Instance
   -> Dedicated hardware

Capacity Reservation
   -> Specific AZ capacity
```

### Responsibility

```text
AWS
-> Infrastructure
-> Hardware
-> Physical security

Customer
-> Security Groups
-> OS patches
-> Software
-> IAM access
-> Data security
```

---

# 39. Recommended Learning Order

### Day 1 — EC2 Basics

```text
EC2
  ↓
Virtual Machine
  ↓
CPU
  ↓
RAM
  ↓
Storage
  ↓
Network
```

### Day 2 — Instance Types

```text
General Purpose
      ↓
Compute Optimized
      ↓
Memory Optimized
      ↓
Storage Optimized
```

### Day 3 — Security

```text
Security Group
      ↓
Inbound
      ↓
Outbound
      ↓
Ports
      ↓
SSH
```

### Day 4 — Purchasing

```text
On-Demand
      ↓
Reserved
      ↓
Convertible Reserved
      ↓
Savings Plans
      ↓
Spot
      ↓
Dedicated
      ↓
Capacity Reservation
```

### Day 5 — Responsibility + Exam Practice

```text
Shared Responsibility
      ↓
AWS responsibility
      ↓
Customer responsibility
      ↓
Exam questions
```

---

# 40. Golden Memory Map

If you have only a few minutes before an exam, revise this:

```text
EC2 = Virtual Server

EC2 Configuration:
AMI + CPU/RAM + Storage + Network + SG + User Data

Instance Types:
General  = Balanced
Compute  = CPU
Memory   = RAM
Storage  = Read/Write

Security:
SG = Firewall
Inbound  = Outside -> EC2
Outbound = EC2 -> Outside

Ports:
22   = SSH
21   = FTP
80   = HTTP
443  = HTTPS
3389 = RDP

Pricing:
On-Demand       = No commitment
Reserved        = Long term
Convertible RI  = Long term + flexible
Savings Plan    = Usage commitment
Spot            = Cheap + can stop
Dedicated Host  = Entire physical server
Dedicated       = Dedicated hardware
Capacity        = Specific AZ capacity

Responsibility:
AWS
= Cloud infrastructure

Customer
= EC2 configuration and security
```

---
