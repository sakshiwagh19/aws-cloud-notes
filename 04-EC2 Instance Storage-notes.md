# 04 - EC2 Instance Storage


## 1. What is EC2 Instance Storage?

When we launch an EC2 instance, we need storage for things such as:

- Operating system files
- Applications
- Configuration files
- Logs
- User data
- Database files
- Temporary files

AWS provides several storage choices around EC2. The main concepts in this section are:

1. **Amazon EBS (Elastic Block Store)**
2. **EBS Snapshots**
3. **AMI (Amazon Machine Image)**
4. **EC2 Image Builder**
5. **EC2 Instance Store**
6. **Amazon EFS (Elastic File System)**
7. **EFS Infrequent Access (EFS-IA)**
8. **Amazon FSx for Windows File Server**
9. **Amazon FSx for Lustre**

The most important exam idea is understanding **which storage type should be used for which situation**.

---

# 2. Amazon EBS (Elastic Block Store)

## What is EBS?

**EBS = Elastic Block Store**

An EBS volume is a **network-attached block storage volume** that can be connected to an EC2 instance.

Think of EBS as a:

> **"Network USB drive for an EC2 instance."**

Unlike a normal USB drive, EBS is provided through AWS infrastructure and communicates with the EC2 instance over the network.

### Simple example

Suppose you create:

```text
EC2 Instance
    |
    +---- EBS Volume (100 GB)
```

The EC2 instance uses the EBS volume to store its files.

If the EC2 instance is terminated, the EBS data can remain depending on the volume's **Delete on Termination** setting.

### Real-life example

Think of an EC2 server as a laptop and EBS as an external hard drive.

You can move the external drive from one compatible computer to another. Similarly, an EBS volume can be detached from an EC2 instance and attached to another instance, subject to its Availability Zone and attachment rules.

---

# 3. Important EBS Characteristics

## 3.1 EBS is network storage

EBS is not a physical disk directly installed inside the EC2 instance.

The EC2 instance communicates with the EBS volume through AWS networking.

Because it uses the network, there can be some network-related latency.

---

## 3.2 EBS is persistent storage

EBS is designed to keep data independently from the lifecycle of the EC2 instance.

Example:

```text
EC2 A
  |
EBS 100 GB
  |
Detach
  |
EC2 B
```

You can detach the EBS volume and attach it to another EC2 instance when appropriate.

---

## 3.3 EBS is tied to an Availability Zone

An EBS volume belongs to a specific Availability Zone.

Example:

```text
Region: us-east-1

AZ: us-east-1a
    EC2
     |
    EBS

AZ: us-east-1b
    EC2
```

An EBS volume created in `us-east-1a` cannot directly be attached to an instance in `us-east-1b`.

### How do you move it?

Use an **EBS Snapshot**:

```text
EBS in AZ-A
     |
   Snapshot
     |
Create EBS in AZ-B
```

---

## 3.4 EBS has provisioned capacity

When creating an EBS volume, you specify storage capacity such as:

```text
10 GB
50 GB
100 GB
500 GB
```

Depending on the EBS volume type, performance characteristics such as IOPS can also be provisioned/configured.

You are charged based on the provisioned storage capacity according to the applicable AWS pricing.

---

## 3.5 EBS capacity can be increased

If an application needs more storage, the EBS volume capacity can be increased over time.

Example:

```text
Initial: 50 GB
        |
Need more storage
        |
Increase
        |
100 GB
```

---

# 4. EBS Availability Zone Example

Suppose you have:

```text
US-EAST-1
│
├── us-east-1a
│   ├── EC2
│   ├── EBS 10 GB
│   ├── EBS 100 GB
│   └── EBS 50 GB
│
└── us-east-1b
    ├── EC2
    ├── EBS 50 GB
    └── EBS 10 GB (unattached)
```

The important point is:

> An EBS volume is associated with one Availability Zone.

To use its data in another AZ, create a snapshot and restore/create a volume in the target AZ.

---

# 5. Delete on Termination

The **Delete on Termination** attribute controls what happens to an EBS volume when its attached EC2 instance is terminated.

According to the course material:

- The root EBS volume is configured to be deleted by default.
- Additional attached EBS volumes are configured not to be deleted by default.
- This setting can be controlled through the AWS Console or AWS CLI.

## Example

Suppose:

```text
EC2
├── Root EBS = 20 GB
└── Data EBS = 100 GB
```

If the instance is terminated:

```text
Root EBS
   -> normally deleted by default

Data EBS
   -> normally preserved by default
```

### Real-life example

Imagine a computer with:

- C: drive = operating system
- D: drive = important company files

If the computer is replaced, you may want to keep the data drive.

The same idea helps explain why the Delete on Termination setting matters.

---

# 6. EBS Snapshots

## What is an EBS Snapshot?

An **EBS Snapshot** is a backup of an EBS volume at a point in time.

Conceptually:

```text
EBS Volume
    |
 Snapshot
    |
 Backup copy
```

### Example

Suppose:

```text
EBS = 100 GB
```

You create a snapshot before making a major change.

If something goes wrong, the snapshot can be used to restore/create an EBS volume.

---

## Do we need to detach the EBS volume first?

No. The course notes state that detaching is not required to create a snapshot, although detaching is recommended in the course context.

---

## Snapshot and Availability Zones

Snapshots can be copied and used to move data across Availability Zones or Regions.

Example:

```text
us-east-1a
EBS 50 GB
   |
Snapshot
   |
us-east-1b
New EBS 50 GB
```

This is one of the key ways to move EBS data between AZs.

---

# 7. EBS Snapshot Archive

EBS Snapshot Archive is a lower-cost storage tier for snapshots that do not need to be accessed frequently.

The course material describes it as approximately **75% cheaper** than the regular snapshot tier, with restoration taking **24–72 hours**.

### When would it make sense?

For example:

```text
Old backup
    |
Rarely needed
    |
Archive
```

It is useful when lower storage cost is more important than fast restoration.

---

# 8. EBS Snapshot Recycle Bin

The EBS Snapshot Recycle Bin helps protect against accidental deletion.

You can configure retention rules so that deleted snapshots are kept for a specified period and can be recovered.

The course material gives a retention range of:

```text
1 day → 1 year
```

### Real-life example

Imagine accidentally deleting an important photo from your phone and having a recycle bin where it remains for some time.

The EBS Snapshot Recycle Bin provides a similar protection concept for snapshots.

---

# 9. AMI - Amazon Machine Image

## What is an AMI?

**AMI = Amazon Machine Image**

An AMI is a packaged image used to launch EC2 instances.

It can contain customized:

- Operating system
- Software
- Configuration
- Monitoring setup
- Other required instance settings

The major benefit is that your environment can be pre-packaged instead of manually configured every time.

---

## Why use an AMI?

Without a custom AMI:

```text
Launch EC2
   ↓
Install OS/software
   ↓
Configure application
   ↓
Configure monitoring
   ↓
Ready
```

With a prepared AMI:

```text
Select AMI
   ↓
Launch EC2
   ↓
Preconfigured environment
```

This can make instance setup faster and more consistent.

---

# 10. Types of AMI

The course describes three common sources:

### 1. Public AMI

An AMI provided publicly by AWS or another publisher.

### 2. Your own AMI

An AMI that you create and maintain.

### 3. AWS Marketplace AMI

An AMI provided through AWS Marketplace, potentially as a paid product.

---

# 11. AMI and Region

AMI images are built for a specific AWS Region.

They can be copied to other Regions when required.

Example:

```text
Custom AMI
    |
    +---- us-east-1
    |
    +---- another Region
```

This is useful when an application needs to be deployed in multiple Regions.

---

# 12. Creating a Custom AMI

A typical process is:

```text
1. Launch EC2
       ↓
2. Customize the instance
       ↓
3. Stop the instance
       ↓
4. Create AMI
       ↓
5. Launch new EC2 instances from the AMI
```

The course recommends stopping the instance before creating the AMI for data integrity.

Creating the AMI also creates EBS snapshots as part of the image process.

---

## Real-life example

Suppose a company has a standard web server configuration:

```text
Linux
Nginx
Application
Monitoring tools
Required configuration
```

Instead of manually installing everything on every server, the company can create a custom AMI.

Then:

```text
Custom AMI
   |
   +---- Web Server 1
   +---- Web Server 2
   +---- Web Server 3
```

This provides a repeatable starting configuration.

---

# 13. EC2 Image Builder

## What is EC2 Image Builder?

EC2 Image Builder automates the process of creating, maintaining, validating, testing, and distributing EC2 AMIs.

Instead of manually creating an AMI every time software changes, Image Builder can automate the workflow.

### Basic flow

```text
EC2 Image Builder
       ↓
Builder EC2 instance
       ↓
Apply build components
       ↓
Create new AMI
       ↓
Test AMI
       ↓
Distribute AMI
```

It can run on a schedule, for example when packages are updated.

---

## Real-life example

A company updates its standard web-server software every week.

Manual approach:

```text
Every week:
Create server
Install updates
Configure
Test
Create AMI
```

Image Builder can automate much of this process.

---

# 14. EC2 Instance Store

## What is EC2 Instance Store?

EC2 Instance Store is **local hardware storage** attached to certain EC2 instances.

Compared with EBS, it can provide very high I/O performance.

The key difference is persistence.

### EBS

```text
Persistent network storage
```

### Instance Store

```text
High-performance local storage
+
Ephemeral
```

---

# 15. What does Ephemeral mean?

**Ephemeral** means temporary.

According to the course material, data on EC2 Instance Store is lost when the instance is stopped, and there is also a risk of data loss if the underlying hardware fails.

Therefore, Instance Store should not be treated like durable primary storage for important data.

---

## Good use cases for Instance Store

Instance Store is useful for:

- Buffers
- Cache
- Scratch data
- Temporary content
- Workloads requiring very high I/O performance

### Example

Suppose an application is processing a large file.

It can use Instance Store as temporary working space:

```text
Input data
   ↓
Temporary processing
   ↓
Instance Store
   ↓
Processing complete
   ↓
Final result stored in durable storage
```

If temporary data is lost, the application can recreate it.

---

# 16. Instance Store vs EBS

| Feature | EBS | EC2 Instance Store |
|---|---|---|
| Storage type | Network block storage | Local hardware storage |
| Persistence | Designed for persistent storage | Ephemeral |
| Performance | Good, network-based | Very high I/O performance |
| Survives instance stop? | Designed to persist | Data is lost |
| Good for | OS, applications, persistent data | Cache, scratch, temporary data |
| Backup | EBS Snapshots | Backup/replication is your responsibility |

### Easy trick

```text
EBS = Persistent
Instance Store = Temporary + Fast
```

---

# 17. Amazon EFS

## What is EFS?

**EFS = Elastic File System**

EFS is a managed **network file system**.

The course describes it as a managed NFS file system that can be mounted by hundreds of EC2 instances.

EFS is designed for Linux EC2 instances and works across multiple Availability Zones.

---

## EFS basic idea

With EBS, think:

```text
One EC2
   |
EBS
```

With EFS, think:

```text
EC2 1 ──┐
EC2 2 ──┤
EC2 3 ──┤
EC2 4 ──┤── EFS
EC2 5 ──┤
...     │
```

Multiple EC2 instances can access the same file system.

---

# 18. Real-life EFS Example

Imagine a company has several web servers:

```text
Web Server 1
Web Server 2
Web Server 3
Web Server 4
       |
     EFS
       |
Shared files
```

If all web servers need access to the same files, a shared file system such as EFS can be useful.

### Example

Suppose an application stores uploaded files:

```text
User uploads profile.jpg
          ↓
         EFS
          ↓
Any web server can access the file
```

This is different from storing the file only on one EC2 instance.

---

# 19. EFS Characteristics

According to the course material:

- Managed NFS file system
- Can be mounted by many EC2 instances
- Works across multiple AZs
- Highly available
- Scalable
- Pay for usage
- No capacity planning is required
- More expensive than the referenced EBS example

The important idea is:

> **EFS is shared file storage for multiple instances.**

---

# 20. EBS vs EFS

This is an important comparison.

| Feature | EBS | EFS |
|---|---|---|
| Storage model | Block storage | File storage |
| Typical access | Attached to EC2 | Shared over network |
| Multiple EC2 instances | Course-level: one instance at a time | Many instances |
| AZ | Bound to one AZ | Multi-AZ |
| Example | EC2 OS/data disk | Shared application files |
| Backup | EBS Snapshots | EFS has its own storage/backup considerations |

### Easy memory trick

```text
EBS = Block = EC2 disk
EFS = File = Shared files
```

---

# 21. EFS Infrequent Access (EFS-IA)

## What is EFS-IA?

**EFS-IA = EFS Infrequent Access**

It is a cost-optimized EFS storage class for files that are not accessed frequently.

The course notes state that it can be up to **92% lower cost** than EFS Standard.

---

## How does it work?

EFS can automatically move files based on access patterns using a Lifecycle Policy.

Example:

```text
EFS Standard
     |
File not accessed for 60 days
     |
Lifecycle Policy
     ↓
EFS-IA
```

The course example uses 60 days.

The application does not need to manually move the files.

### Real-life example

A company stores reports:

```text
Current reports → frequently accessed
Old reports → rarely accessed
```

Old reports can be moved to the lower-cost EFS-IA storage class.

---

# 22. Amazon FSx

## What is FSx?

Amazon FSx provides fully managed third-party high-performance file systems on AWS.

The course introduces:

- FSx for Windows File Server
- FSx for Lustre
- FSx for NetApp ONTAP

The two covered in detail in this section are Windows File Server and Lustre.

---

# 23. FSx for Windows File Server

FSx for Windows File Server is a managed Windows-native shared file system.

Important characteristics from the course:

- Fully managed
- Highly reliable
- Scalable
- Based on Windows File Server
- Supports SMB
- Supports Windows NTFS
- Integrates with Microsoft Active Directory
- Can be accessed from AWS and on-premises infrastructure

---

## Real-life example

Suppose a company has a Windows environment:

```text
Windows users
      |
Windows application
      |
EC2 Windows server
      |
FSx for Windows File Server
      |
Shared company files
```

A Windows-based company can use FSx for Windows when it needs a Windows-native shared file system.

---

# 24. SMB and NTFS

Two terms to remember:

### SMB

**SMB = Server Message Block**

It is a protocol used for accessing shared files and resources in Windows environments.

### NTFS

**NTFS = New Technology File System**

It is a Windows file system.

For the exam, remember:

```text
FSx for Windows
        ↓
SMB + NTFS
        ↓
Windows environment
```

---

# 25. FSx for Lustre

FSx for Lustre is a managed, high-performance file system designed for demanding workloads such as:

- High Performance Computing (HPC)
- Machine Learning
- Analytics
- Video processing
- Financial modeling

The course describes Lustre as being derived from **Linux + cluster**.

It can provide very high throughput, millions of IOPS, and very low latency for appropriate workloads.

---

## Real-life example

Imagine a research company processing thousands of large datasets:

```text
Compute instances
      |
      ↓
FSx for Lustre
      |
Large datasets
```

Many compute instances can work with high-performance shared storage.

---

# 26. FSx for Lustre + Amazon S3

The course diagram shows FSx for Lustre connected with Amazon S3.

Conceptually:

```text
Amazon S3
   ↕
FSx for Lustre
   ↕
Compute instances
```

This can be useful for workloads that need high-performance file-system access while using S3 for data storage.

---

# 27. Shared Responsibility for EC2 Storage

AWS and the customer have different responsibilities.

## AWS responsibility

The course highlights responsibilities such as:

- Underlying infrastructure
- Replication for EBS volumes and EFS drives
- Replacing faulty hardware
- Protecting customer data from access by AWS employees

## Customer responsibility

The customer is responsible for things such as:

- Setting up backup/snapshot procedures
- Configuring data encryption as required
- Managing data stored on the drives
- Understanding the risk of using EC2 Instance Store

### Important idea

```text
AWS secures the underlying cloud infrastructure.

Customer manages and protects the data/configuration
they put into the storage services.
```

---

# 28. Complete Storage Decision Guide

Use this simple decision process.

### Need a normal persistent disk for EC2?

```text
→ EBS
```

### Need to back up an EBS volume?

```text
→ EBS Snapshot
```

### Need a reusable preconfigured EC2 environment?

```text
→ AMI
```

### Need to automate AMI creation and testing?

```text
→ EC2 Image Builder
```

### Need extremely fast local temporary storage?

```text
→ EC2 Instance Store
```

### Need shared file storage for many Linux EC2 instances?

```text
→ EFS
```

### Need lower-cost storage for files that are accessed infrequently?

```text
→ EFS-IA
```

### Need a Windows-native shared file system?

```text
→ FSx for Windows File Server
```

### Need high-performance shared storage for HPC / ML / analytics?

```text
→ FSx for Lustre
```

---

# 29. One Big Real-World Example

Imagine an e-commerce company running an online shopping website.

## Architecture

```text
                    Users
                      |
                      v
                Web Application
                      |
          +-----------+-----------+
          |           |           |
        EC2-1       EC2-2       EC2-3
          |           |           |
          +-----------+-----------+
                      |
                     EFS
                Shared files
                      |
             Application uploads
```

### Where can EBS be used?

Each EC2 instance can have EBS for its operating system and persistent block storage needs.

```text
EC2-1 → EBS
EC2-2 → EBS
EC2-3 → EBS
```

### Where can EFS be used?

If all web servers need access to the same uploaded files:

```text
EC2-1 ─┐
EC2-2 ─┼── EFS
EC2-3 ─┘
```

### Where can snapshots be used?

Before a major change, create an EBS snapshot.

```text
EBS
 ↓
Snapshot
 ↓
Backup / recovery point
```

### Where can Instance Store be used?

Suppose the application performs temporary image processing.

```text
Image
 ↓
Temporary processing
 ↓
Instance Store
 ↓
Final result
```

The temporary data should not be treated as durable because Instance Store is ephemeral.

### Where can an AMI be used?

Create a standard application server image:

```text
Custom AMI
   |
   +--- EC2-1
   +--- EC2-2
   +--- EC2-3
```

This makes new server deployment more consistent.

---

# 30. EBS vs Instance Store vs EFS vs FSx

| Storage | Main Idea | Persistence / Sharing | Best Fit |
|---|---|---|---|
| **EBS** | Network block storage | Persistent, attached to EC2 | OS, applications, persistent block data |
| **Instance Store** | Local hardware disk | Ephemeral | Cache, buffer, scratch, temporary high-I/O data |
| **EFS** | Managed network file system | Shared across many EC2 instances | Shared Linux files |
| **FSx Windows** | Managed Windows file system | Shared | Windows/SMB/NTFS workloads |
| **FSx Lustre** | High-performance file system | Shared | HPC, ML, analytics, video processing |

---

# 31. Most Important Exam Points

Remember these:

### EBS

```text
EBS = Network Block Storage
EBS = tied to an Availability Zone
EBS = persistent storage
EBS Snapshot = backup / move data between AZs or Regions
```

### AMI

```text
AMI = Amazon Machine Image
AMI = preconfigured template/image for launching EC2
```

### Image Builder

```text
Image Builder = automate AMI creation, testing and distribution
```

### Instance Store

```text
Instance Store = local hardware disk
Instance Store = very high I/O
Instance Store = ephemeral
```

### EFS

```text
EFS = managed network file system
EFS = shared by many EC2 instances
EFS = multi-AZ
```

### EFS-IA

```text
EFS-IA = lower-cost class for infrequently accessed files
```

### FSx

```text
FSx for Windows = Windows / SMB / NTFS
FSx for Lustre = HPC / ML / analytics / high performance
```

---

# 32. Easy Memory Trick

```text
EBS
↓
EC2 Disk

Snapshot
↓
EBS Backup

AMI
↓
EC2 Template

Image Builder
↓
Automated AMI Factory

Instance Store
↓
Fast + Temporary

EFS
↓
Shared Linux Files

EFS-IA
↓
Rarely Used Files + Lower Cost

FSx Windows
↓
Windows File System

FSx Lustre
↓
High Performance / HPC
```

---

# 33. Quick Revision Questions

### Q1. What is EBS?

A network-attached block storage service used with EC2.

### Q2. Can an EBS volume in one AZ directly attach to an EC2 instance in another AZ?

No. Create/use a snapshot to move the data to another AZ.

### Q3. What is an EBS Snapshot?

A point-in-time backup of an EBS volume.

### Q4. What is AMI?

A packaged/customized image used to launch EC2 instances.

### Q5. What does EC2 Image Builder do?

It automates creation, maintenance, testing, and distribution of EC2 AMIs.

### Q6. Which storage gives high-performance local storage but is ephemeral?

EC2 Instance Store.

### Q7. Which storage is suitable when many EC2 instances need shared Linux file storage?

EFS.

### Q8. What is EFS-IA?

A lower-cost EFS storage class for files that are not accessed frequently.

### Q9. Which FSx service is designed for Windows file workloads?

FSx for Windows File Server.

### Q10. Which FSx service is designed for HPC and high-performance workloads?

FSx for Lustre.

---

# 34. Final Summary

The EC2 Instance Storage section is mainly about choosing the correct storage model.

```text
                    EC2 Storage
                         |
       +-----------------+------------------+
       |                 |                  |
      EBS              EFS              Instance Store
       |                 |                  |
   Block storage     File storage       Local storage
   Persistent        Shared             Temporary
       |                 |                  |
   Snapshot          Multi-AZ            High I/O
       |
      AMI
       |
   EC2 template

Other managed file systems:
       |
       +-- FSx for Windows → Windows / SMB / NTFS
       |
       +-- FSx for Lustre  → HPC / ML / Analytics
```

The most important distinction is:

> **EBS is persistent block storage for EC2, EFS is shared file storage, Instance Store is very fast but temporary local storage, and FSx provides specialized managed file systems.**

---

