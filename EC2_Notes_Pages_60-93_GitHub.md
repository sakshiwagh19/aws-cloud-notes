# Amazon EC2 — Complete Notes (Pages 60–93)

> Based on the uploaded **AWS Certified Cloud Practitioner Slides v42(2)**, pages 60–93.
> Language: simple Hindi (English script) + English AWS terms + practical examples.

---

## 1. Amazon EC2 kya hai?

**EC2 = Elastic Compute Cloud**

EC2 AWS ka **Infrastructure as a Service (IaaS)** hai. Iska main kaam cloud me virtual servers provide karna hai.

Simple meaning:

**EC2 = AWS par ek virtual computer/server rent karna.**

Is virtual server par hum:
- website chala sakte hain
- backend/API chala sakte hain
- applications install kar sakte hain
- Linux/Windows OS use kar sakte hain

### EC2 ke around important services

EC2 section me ye concepts important hain:

1. **EC2** → virtual machine/server
2. **EBS** → virtual/network storage
3. **ELB** → load ko multiple machines me distribute karta hai
4. **ASG** → demand ke according EC2 instances ko scale karta hai

### Real-life example

Maan lo tumhari website hai:

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
- EBS = server ka storage
- ELB = traffic distribute karta hai
- ASG = servers ki quantity automatically manage kar sakta hai

**Exam line:** EC2 is IaaS and provides virtual machines.

---

# 2. EC2 Sizing & Configuration

EC2 launch karte waqt hum sirf "server" select nahi karte. Hume server ki configuration choose karni hoti hai.

Important configuration:

### 1. Operating System (OS)

Examples:
- Linux
- Windows
- Mac OS

OS decide karta hai server ke andar ka environment.

### 2. CPU

CPU = processing power.

More CPU → generally more compute capability.

Example:

```text
Small website      -> low CPU
Video processing   -> high CPU
Scientific work    -> very high CPU
```

### 3. RAM

RAM = temporary working memory.

Agar application ko bahut data memory me process karna hai, to zyada RAM useful hoti hai.

Example:

```text
Simple web server -> moderate RAM
Large database    -> high RAM
```

### 4. Storage

Storage ke examples:
- **EBS / EFS** network-attached storage
- **EC2 Instance Store** hardware/local storage

### 5. Network card

Network configuration me:
- network performance
- public IP address
etc. important hote hain.

### 6. Security Group

Security Group EC2 ke network traffic ke rules control karta hai.

### 7. EC2 User Data

First launch par commands automatically run karne ke liye User Data use hota hai.

---

# 3. EC2 User Data

**EC2 User Data = first start par automatically run hone wali bootstrap script.**

"Bootstrap" ka simple meaning:

> Machine start hote hi kuch commands automatically execute karna.

Slides ke according User Data:
- first start par run hota hai
- updates install kar sakta hai
- software install kar sakta hai
- internet se common files download kar sakta hai
- other boot tasks automate kar sakta hai
- root user ke saath run hota hai

### Example

Maan lo tum EC2 launch karte hi web server install karna chahte ho.

Concept:

```text
EC2 Launch
   |
   v
User Data Script
   |
   +--> update system
   +--> install web server
   +--> start web server
   |
   v
Web Server Ready
```

### Important exam point

**User Data script first start par run hoti hai.**

---

# 4. EC2 Instance Types

AWS different workloads ke liye different EC2 instance types provide karta hai.

Instance name example:

```text
m5.2xlarge
```

Isko 3 parts me samjho:

```text
m      5       2xlarge
|      |          |
class  generation size
```

- **m** = instance class
- **5** = generation
- **2xlarge** = instance size

---

# 5. General Purpose Instances

General Purpose = balanced machine.

Balance between:
- Compute
- Memory
- Networking

Use cases:
- Web servers
- Code repositories
- General applications

Course example:

```text
t2.micro
```

### Easy example

Agar tum ek normal website/backend run kar rahe ho aur koi special CPU/RAM requirement nahi hai:

```text
General Purpose
       |
       v
Balanced CPU + RAM + Network
```

**Exam clue:** "balanced" → General Purpose.

---

# 6. Compute Optimized Instances

Compute Optimized instances ka focus **high-performance processors / CPU-heavy workloads** par hota hai.

Use cases:
- Batch processing
- Media transcoding
- High-performance web servers
- High Performance Computing (HPC)
- Scientific modeling
- Machine learning
- Dedicated gaming servers

### Easy example

Agar tumhe bahut CPU calculation karni hai:

```text
Heavy calculation
       |
       v
Compute Optimized
```

**Exam clue:** "compute-intensive / CPU-heavy" → Compute Optimized.

---

# 7. Memory Optimized Instances

Memory Optimized ka focus **large datasets ko memory me process karna** hai.

Use cases:
- High-performance relational databases
- High-performance non-relational databases
- Distributed cache stores
- In-memory databases
- Business Intelligence
- Real-time processing of large unstructured data

### Easy example

Agar application ko bahut bada dataset RAM me rakhkar quickly process karna hai:

```text
Large data
   |
   v
RAM-heavy workload
   |
   v
Memory Optimized
```

**Exam clue:** "large data in memory" → Memory Optimized.

---

# 8. Storage Optimized Instances

Storage Optimized instances ka focus **high read/write access to large datasets on local storage** par hota hai.

Use cases:
- High-frequency OLTP systems
- Relational databases
- NoSQL databases
- Redis/cache workloads
- Data warehousing
- Distributed file systems

### Easy example

Agar workload ko storage par bahut fast read/write karna hai:

```text
Large data
   |
   v
High read/write
   |
   v
Storage Optimized
```

**Exam clue:** "high sequential read/write" → Storage Optimized.

---

# 9. Instance Type ko yaad rakhne ka shortcut

```text
General Purpose
= Balanced

Compute Optimized
= CPU / calculations

Memory Optimized
= RAM / large data in memory

Storage Optimized
= Fast storage read/write
```

### Exam scenario trick

| Question me clue | Answer |
|---|---|
| Balanced workload | General Purpose |
| CPU-intensive | Compute Optimized |
| Large data in memory | Memory Optimized |
| High storage read/write | Storage Optimized |

---

# 10. EC2 Instance Size

Same family ke andar different sizes available ho sakte hain.

Example from slides:

```text
t2.micro
t2.xlarge
```

Slides example:

```text
t2.micro  -> 1 vCPU, 1 GiB memory
t2.xlarge -> 4 vCPU, 16 GiB memory
```

General idea:

```text
Small size
   |
   v
less resources

Large size
   |
   v
more resources
```

Instance specifications include:
- vCPU
- memory
- storage
- network performance
- EBS bandwidth

---

# 11. Security Groups

**Security Group = EC2 instance ka firewall.**

Slides ke according Security Groups:
- inbound traffic control karte hain
- outbound traffic control karte hain
- ports regulate karte hain
- authorized IP ranges define kar sakte hain
- IP ya another Security Group ko reference kar sakte hain

### Inbound vs Outbound

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

Inbound = bahar se EC2 ke andar aane wala traffic.

```text
EC2
 |
 | OUTBOUND
 v
Internet / Other service
```

Outbound = EC2 se bahar jaane wala traffic.

---

# 12. Security Group ko Firewall ki tarah samjho

Example:

Tumhare EC2 par web server hai.

Agar HTTP port 80 allow hai:

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

Agar port 80 allow nahi hai:

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

EC2 ko blocked traffic dikhega hi nahi.

---

# 13. Security Group Important Rules

Slides ke according:

- Ek Security Group multiple EC2 instances ko attach ho sakta hai.
- Security Group region/VPC combination se tied hota hai.
- Security Group EC2 ke bahar traffic filter karta hai.
- SSH access ke liye separate Security Group maintain karna useful hai.
- Inbound traffic default se blocked hota hai.
- Outbound traffic default se authorized hota hai.

### Timeout vs Connection Refused

**Application timeout:**

```text
Request
  |
  v
Security Group
  |
 BLOCK
  X
```

Possible security group issue.

**Connection refused:**

Server tak connection pahunch gaya, lekin application/service listen nahi kar rahi ho sakti hai.

So:

```text
Timeout
= security group ko check karo

Connection refused
= application/service ko check karo
```

---

# 14. Security Group can reference another Security Group

Security Group rule me IP address ke alawa another Security Group ko reference kiya ja sakta hai.

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

EC2 ka rule conceptually:

```text
Allow HTTP
FROM SG-ALB
```

Iska benefit: rule specific group ko trust karta hai instead of manually maintaining IP addresses.

---

# 15. Important Ports

Ye ports exam ke liye important hain:

| Port | Protocol | Use |
|---:|---|---|
| 22 | SSH | Linux instance login |
| 21 | FTP | File Transfer |
| 22 | SFTP | Secure file transfer using SSH |
| 80 | HTTP | Unsecured websites |
| 443 | HTTPS | Secured websites |
| 3389 | RDP | Windows remote desktop |

### Memory trick

```text
22   = SSH
21   = FTP
80   = HTTP
443  = HTTPS
3389 = RDP
```

### Example

Linux EC2 me terminal se login:

```text
Your Computer
     |
     | SSH : 22
     v
Security Group
     |
     v
EC2 Linux
```

Windows EC2 remote desktop:

```text
Your Computer
     |
     | RDP : 3389
     v
EC2 Windows
```

---

# 16. SSH

**SSH = Secure Shell**

SSH se remote EC2 machine ko command line se control kar sakte ho.

Example:

```text
Your Laptop
     |
     | SSH
     | Port 22
     v
EC2 Linux
```

SSH ke liye Security Group me port 22 allow hona chahiye.

### Important

SSH = Linux remote command-line access.

---

# 17. Windows se EC2 access

Slides me Windows ke liye:
- PuTTY
- Windows SSH support
- EC2 Instance Connect

jaise methods discuss kiye gaye hain.

Main concept:

```text
Windows PC
    |
    | SSH
    v
Linux EC2
```

---

# 18. EC2 Instance Connect

**EC2 Instance Connect = browser ke andar se EC2 se connect karna.**

Slides ke according:
- browser se EC2 connect kar sakte ho
- downloaded key file ki need nahi hoti
- AWS temporary key EC2 par upload karta hai
- slides ke context me Amazon Linux 2 ke saath out-of-the-box support mention hai
- port 22 open hona chahiye

### Easy flow

```text
Browser
   |
   v
EC2 Instance Connect
   |
   | temporary key
   v
EC2
```

---

# 19. EC2 Purchasing Options

EC2 ko use karne ke multiple pricing/purchasing options hain:

1. On-Demand
2. Reserved Instances
3. Convertible Reserved Instances
4. Savings Plans
5. Spot Instances
6. Dedicated Hosts
7. Dedicated Instances
8. Capacity Reservations

Ab ek-ek ko easy example se samjho.

---

# 20. On-Demand Instances

**On-Demand = jab chahiye tab use karo, full price pay karo, long-term commitment nahi.**

Slides ke according:
- pay for what you use
- no long-term commitment
- highest cost among the compared options
- short-term / unpredictable workloads ke liye useful

### Real-life example

Hotel analogy:

```text
Aaj room chahiye
     |
     v
Room book karo
     |
     v
Full price pay karo
```

Cloud:

```text
EC2 On-Demand
     |
     v
Use when needed
     |
     v
Pay for usage
```

**Exam clue:** short-term + unpredictable + no commitment → On-Demand.

---

# 21. Reserved Instances

Reserved Instance = long-term commitment ke badle lower price.

Slides:
- 1 year or 3 year reservation
- steady-state workloads ke liye recommended
- specific instance attributes reserve kiye ja sakte hain
- payment options:
  - No Upfront
  - Partial Upfront
  - All Upfront

### Example

Database ko 24x7 continuously chalana hai:

```text
Database
   |
   | predictable
   | long-term
   v
Reserved Instance
```

**Exam clue:** steady workload + 1/3 year commitment → Reserved Instance.

---

# 22. Convertible Reserved Instance

Convertible RI me flexibility zyada hoti hai.

Slides ke according:
- instance type change kar sakte hain
- instance family change kar sakte hain
- OS change kar sakte hain
- scope change kar sakte hain
- tenancy change kar sakte hain

Trade-off:

```text
More flexibility
      |
      v
Convertible RI
```

Exam me exact discount percentage par focus mat karo; slides bhi note karti hain ki AWS discounts time ke saath change kar sakta hai.

---

# 23. Savings Plans

Savings Plan me tum ek certain usage commitment karte ho.

Slides example:

```text
$10/hour
for 1 or 3 years
```

Usage commitment ke andar discount milta hai.

Usage commitment se beyond usage:

```text
Extra usage
    |
    v
On-Demand price
```

Slides ke according EC2 Savings Plan specific instance family + AWS Region ke context me locked hota hai, lekin:
- instance size
- OS
- tenancy

me flexibility deta hai.

### Easy example

```text
Commit:
$10/hour

Actual:
$8/hour -> covered
$12/hour -> extra $2 On-Demand
```

**Exam clue:** commitment to a certain amount of usage → Savings Plans.

---

# 24. Spot Instances

**Spot = very cheap but interruption risk.**

Slides ke according:
- On-Demand ke comparison me very large discount possible
- instance lose ho sakta hai
- workloads failure-resilient hone chahiye
- batch jobs
- data analysis
- image processing
- distributed workloads
- flexible start/end time

Not suitable for:
- critical jobs
- databases

### Real-life example

Hotel analogy:

```text
Empty room
   |
   v
Cheap bid
   |
   v
Room mil gaya
   |
   v
Hotel ko room chahiye
   |
   v
You can lose room
```

Cloud:

```text
Spot EC2
   |
   v
Very cheap
   |
   v
Can be interrupted
```

**Exam clue:** cheapest + interruptible + fault tolerant → Spot.

---

# 25. Dedicated Hosts

**Dedicated Host = entire physical server dedicated to your use.**

Slides ke according useful for:
- compliance requirements
- complicated software licensing
- BYOL (Bring Your Own License)

It is the most expensive option in this comparison.

### Easy example

Normal EC2:

```text
Physical Host
 + EC2
 + EC2
 + EC2
```

Dedicated Host:

```text
Physical Host
 + Your EC2 instances
 + Your control/placement
```

Think:

> "Mujhe poora physical server chahiye."

**Exam clue:** licensing/compliance + entire physical host → Dedicated Host.

---

# 26. Dedicated Instances

Dedicated Instances run on hardware dedicated to you.

But important difference from Dedicated Host:

- hardware is dedicated to your account
- instances in the same account may share the hardware
- you do not control instance placement

### Remember

```text
Dedicated Host
= entire physical server + placement control

Dedicated Instance
= dedicated hardware
  but no placement control
```

---

# 27. Capacity Reservations

Capacity Reservation ka purpose discount nahi hai.

Purpose:

> Specific Availability Zone me EC2 capacity reserve karna.

Slides ke according:
- On-Demand capacity reserve hoti hai
- specific AZ
- any duration
- no long-term commitment
- no billing discount
- create/cancel anytime
- instance run kare ya na kare, On-Demand rate charge hota hai
- short-term uninterrupted workload needing a specific AZ ke liye useful

### Example

Maan lo:

```text
AZ = ap-south-1a

Mujhe guarantee chahiye:
"Jab mujhe EC2 chahiye, capacity available ho."
```

Then:

```text
Capacity Reservation
        |
        v
Specific AZ capacity reserved
```

**Exam clue:** reserve capacity in a specific AZ → Capacity Reservation.

---

# 28. Purchasing Options — Super Easy Comparison

| Option | Main idea | Best clue |
|---|---|---|
| On-Demand | No commitment | Short/unpredictable |
| Reserved | Long-term reservation | Steady workload |
| Convertible RI | Long-term + more flexibility | Need to change attributes |
| Savings Plans | Commit to usage amount | Usage commitment |
| Spot | Very cheap, can interrupt | Fault-tolerant workload |
| Dedicated Host | Entire physical host | Licensing/compliance |
| Dedicated Instance | Dedicated hardware | No shared hardware with other customers |
| Capacity Reservation | Reserve capacity | Specific AZ |

---

# 29. Hotel Analogy for Purchasing Options

Is analogy ko exam ke time yaad rakho:

### On-Demand

```text
Hotel me kabhi bhi jao
Full price
No commitment
```

### Reserved

```text
Pehle se booking
Long stay
Discount
```

### Savings Plan

```text
Certain spending/hour commit
Different room sizes me flexibility
```

### Spot

```text
Cheap empty room
Hotel kabhi bhi room wapas le sakta hai
```

### Dedicated Host

```text
Poora hotel/building book
```

### Capacity Reservation

```text
Room reserve kiya
Use karo ya na karo
Full price
```

---

# 30. Price Comparison

Slides me m4.large, us-east-1 ka example diya gaya hai.

Important concept:

```text
On-Demand
   |
   v
Full / higher price

Reserved / Savings Plan
   |
   v
Long commitment
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

**Exam ke liye exact dollar values yaad karna zaroori nahi hai.**

---

# 31. EC2 Shared Responsibility Model

EC2 me AWS aur customer dono ki responsibilities hoti hain.

## AWS ki responsibility

Slides ke according AWS handles:

- Infrastructure
- Global network security
- Physical host isolation
- Replacing faulty hardware
- Compliance validation

Simple:

```text
AWS
 |
 +--> Physical infrastructure
 +--> Hardware
 +--> Global infrastructure security
```

## Customer ki responsibility

Customer handles:

- Security Group rules
- Operating-system patches and updates
- Software and utilities installed on EC2
- IAM Roles assigned to EC2
- IAM user access management
- Data security on the instance

Simple:

```text
Customer
 |
 +--> Security Groups
 +--> OS updates
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

Ab poore section ko ek picture ki tarah samjho:

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

Note: ELB, EBS, and ASG are introduced in the EC2 overview; deeper storage topics are covered in the following section of the course.

---

# 33. EC2 Instance ko ek real computer ki tarah samjho

EC2 = rented virtual computer.

So:

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

This is one of the easiest ways to understand EC2.

---

# 34. EC2 Launch Formula

Slides ke summary ko simple formula me:

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

Tum ek Linux web server launch karna chahte ho:

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

# 35. Inbound vs Outbound — Important

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

or

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

Example: EC2 application downloads something from an external HTTPS service.

---

# 36. Common Exam Questions

### Q1. EC2 belongs to which cloud model?

**Answer: IaaS**

---

### Q2. What is EC2?

**Answer:** Elastic Compute Cloud, a service for running virtual machines.

---

### Q3. Which service acts as firewall for EC2?

**Answer: Security Group**

---

### Q4. Which port is SSH?

**Answer: 22**

---

### Q5. Which port is HTTP?

**Answer: 80**

---

### Q6. Which port is HTTPS?

**Answer: 443**

---

### Q7. Which port is RDP?

**Answer: 3389**

---

### Q8. Which EC2 type is good for CPU-intensive workloads?

**Answer: Compute Optimized**

---

### Q9. Which type is good for large datasets in memory?

**Answer: Memory Optimized**

---

### Q10. Which type is good for high read/write storage workloads?

**Answer: Storage Optimized**

---

### Q11. Which type provides a balance of compute, memory and networking?

**Answer: General Purpose**

---

### Q12. Which option is best for short unpredictable workload with no commitment?

**Answer: On-Demand**

---

### Q13. Which option is best for steady long-term workload?

**Answer: Reserved Instances**

---

### Q14. Which option can be interrupted and is very cheap?

**Answer: Spot Instances**

---

### Q15. Which option is useful for complex software licensing?

**Answer: Dedicated Host**

---

### Q16. Which option reserves capacity in a specific AZ?

**Answer: Capacity Reservation**

---

### Q17. What runs automatically at first EC2 start?

**Answer: EC2 User Data**

---

### Q18. What is SSH used for?

**Answer:** Secure remote command-line access to a Linux EC2 instance.

---

### Q19. What happens if Security Group blocks traffic?

**Answer:** The EC2 instance does not see the blocked traffic.

---

### Q20. Timeout vs Connection Refused?

```text
Timeout
= likely Security Group/network rule issue

Connection refused
= application/service issue or application not launched
```

---

# 37. Most Important Exam Keywords

```text
EC2
= IaaS / Virtual Machine

AMI
= OS/image used to launch instance

User Data
= first-start bootstrap script

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
= balanced

Compute Optimized
= CPU-intensive

Memory Optimized
= large data in memory

Storage Optimized
= high read/write

On-Demand
= no commitment

Reserved
= long-term steady workload

Convertible Reserved
= long-term + flexibility

Savings Plans
= usage commitment

Spot
= cheap + interruptible

Dedicated Host
= entire physical host

Dedicated Instance
= dedicated hardware, no placement control

Capacity Reservation
= capacity in specific AZ
```

---

# 38. Final One-Page Revision

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
Memory Optimized  -> RAM / memory
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
   -> short/unpredictable

Reserved
   -> steady/long-term

Convertible RI
   -> steady + flexibility

Savings Plans
   -> usage commitment

Spot
   -> cheapest + interruptible

Dedicated Host
   -> entire physical server

Dedicated Instance
   -> dedicated hardware

Capacity Reservation
   -> reserve capacity in specific AZ
```

### Responsibility

```text
AWS
-> infrastructure
-> hardware
-> physical security

YOU
-> Security Groups
-> OS patches
-> software
-> IAM access
-> data security
```

---

# 39. Best Way to Learn This Section

EC2 ko ratne ke bajay is order me samjho:

### Day 1
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

### Day 2
```text
Instance Types
↓
General
↓
Compute
↓
Memory
↓
Storage
```

### Day 3
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

### Day 4
```text
Purchasing Options
↓
On-Demand
↓
Reserved
↓
Savings Plans
↓
Spot
↓
Dedicated
↓
Capacity Reservation
```

### Day 5
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

Agar exam se just pehle sirf ek cheez revise karni ho:

```text
EC2 = Virtual Server

EC2 Configuration:
AMI + CPU/RAM + Storage + Network + SG + User Data

Instance Types:
General = Balanced
Compute = CPU
Memory = RAM
Storage = I/O

Security:
SG = Firewall
Inbound = Outside -> EC2
Outbound = EC2 -> Outside

Ports:
22 = SSH
21 = FTP
80 = HTTP
443 = HTTPS
3389 = RDP

Pricing:
On-Demand = No commitment
Reserved = Long term
Convertible = Long term + flexible
Savings Plan = Usage commitment
Spot = Cheap + can stop
Dedicated Host = Entire physical server
Dedicated Instance = Dedicated hardware
Capacity Reservation = Specific AZ capacity

Responsibility:
AWS = Cloud infrastructure
Customer = EC2 configuration/security
```

## Source

These notes are based on pages **60–93** of the uploaded AWS Certified Cloud Practitioner slides. The source's EC2 summary identifies EC2 as IaaS, Security Groups as the EC2 firewall, User Data as the first-start script, SSH as port 22, and the listed purchasing options. 
