# 1. What is Amazon S3?

**Amazon S3 = Amazon Simple Storage Service.**

S3 is an AWS **object storage service**. It is used to store files and
data such as images, videos, PDFs, backups, logs, application files,
data for analytics, software files, and static websites.

Think of S3 as a very large online storage system:

``` text
AWS Account
    |
    v
  Bucket
    |
    +---- photo.jpg
    +---- video.mp4
    +---- resume.pdf
    +---- backup.zip
```

S3 is designed to scale to very large amounts of data.

------------------------------------------------------------------------

# 2. S3 Use Cases

Common use cases:

-   Backup and storage
-   Disaster recovery
-   Archive
-   Hybrid cloud storage
-   Application hosting
-   Media hosting
-   Data lakes and big-data analytics
-   Software delivery
-   Static website hosting

**Example:** A company can store customer documents in S3 and also use
the same data for backup or analytics.

------------------------------------------------------------------------

# 3. S3 Buckets

A **bucket** is the main container where S3 objects are stored.

``` text
Bucket: my-company-data
    |
    +-- photo.jpg
    +-- report.pdf
    +-- backup.zip
```

## Bucket name rules

Bucket names must be:

-   Globally unique across AWS
-   3--63 characters long
-   Lowercase
-   Without uppercase letters
-   Without underscores
-   Not an IP address
-   Start with a lowercase letter or number
-   Must not start with `xn--`
-   Must not end with `-s3alias`

Because the name is globally unique, if another AWS customer already
owns `my-bucket`, you cannot create another bucket with exactly that
name.

## Region

S3 looks like a global service, but a bucket is created in a specific
AWS Region.

``` text
Bucket
  |
  +-- Region: ap-south-1
```

------------------------------------------------------------------------

# 4. S3 Objects

A file stored in S3 is called an **object**.

An object contains/has:

-   Data/content
-   Key
-   Metadata
-   Tags
-   Version ID when versioning is enabled

Example:

``` text
photo.jpg  -> S3 object
```

------------------------------------------------------------------------

# 5. Object Key

The **key** is the full name/path of an object.

Example:

``` text
s3://my-bucket/my_file.txt
```

Here:

``` text
Bucket = my-bucket
Key    = my_file.txt
```

Another example:

``` text
s3://my-bucket/photos/2026/photo.jpg
```

The key is:

``` text
photos/2026/photo.jpg
```

You can think of the key as **prefix + object name**:

``` text
Prefix       = photos/2026/
Object name  = photo.jpg
```

### Important

S3 does not technically use normal directories/folders like a computer
file system. The console shows folders for convenience, but internally
the object has a key containing `/`.

------------------------------------------------------------------------

# 6. Maximum S3 Object Size

The maximum size of **one S3 object is 5 TB (5000 GB)**.

This is NOT the total size of a bucket.

``` text
Bucket total size = no fixed maximum
Single object     = maximum 5 TB
```

Example:

``` text
movie.mp4 = 4 TB  -> possible as one object
movie.mp4 = 6 TB  -> cannot be one object
```

## Multipart Upload

For large uploads, S3 supports **multipart upload**. A large file is
divided into parts and those parts are uploaded separately.

``` text
Large File
   |
   +---- Part 1
   +---- Part 2
   +---- Part 3
   +---- Part 4
          |
          v
         S3
```

The course material also highlights multipart upload for uploads larger
than 5 GB.

------------------------------------------------------------------------

# 7. Object Metadata and Tags

## Metadata

Metadata is information about an object. It can be system metadata or
user-defined metadata.

Example:

``` text
Object: photo.jpg
Content-Type: image/jpeg
```

## Tags

Tags are key-value pairs attached to objects. The course material states
up to **10 tags** per object.

Example:

``` text
Environment = Production
Department  = Finance
```

Tags are useful for security, lifecycle management, and organization.

------------------------------------------------------------------------

# 8. S3 Security

Important S3 security controls covered in the course:

1.  IAM policies
2.  S3 bucket policies
3.  Object ACLs
4.  Bucket ACLs
5.  Encryption
6.  Block Public Access

------------------------------------------------------------------------

# 9. IAM Policies

IAM policies provide **user/role-based permissions**.

``` text
IAM User/Role
      |
      v
  IAM Policy
      |
      v
      S3
```

A policy can allow or deny actions such as:

``` text
s3:GetObject
s3:PutObject
s3:DeleteObject
```

Example: if a user needs to download objects, the policy can allow
`s3:GetObject`.

------------------------------------------------------------------------

# 10. S3 Bucket Policy

A **bucket policy** is a resource-based JSON policy attached to an S3
bucket.

It can control access to the bucket and its objects.

Common uses:

-   Grant public access when appropriate
-   Force objects to be encrypted at upload
-   Grant access to another AWS account
-   Cross-account access

Example:

``` text
Internet User
     |
     v
S3 Bucket
     ^
     |
Bucket Policy
allows public read
```

------------------------------------------------------------------------

# 11. IAM Policy vs Bucket Policy

**IAM Policy:** attached to a user/group/role and defines what that
identity can do.

**Bucket Policy:** attached to an S3 bucket and defines resource access.

Easy memory:

> IAM Policy = permissions for an identity.
>
> Bucket Policy = permissions/access rules for an S3 resource.

------------------------------------------------------------------------

# 12. S3 Access Decision

The course gives this simplified rule:

An IAM principal can access an S3 object when IAM permissions allow it
**OR** a resource policy allows it, **AND there is no explicit DENY**.

``` text
Allow
  +
No Explicit DENY
  |
  v
Access
```

If there is an explicit deny, access is denied.

------------------------------------------------------------------------

# 13. Block Public Access

S3 Block Public Access settings were created to help prevent company
data leaks.

If you know a bucket should never be public, keep these protections
enabled.

They can also be configured at the account level.

Example:

``` text
Private Customer Data
        |
        v
    S3 Bucket
        |
Block Public Access = ON
```

------------------------------------------------------------------------

# 14. Static Website Hosting

S3 can host a **static website** that can be accessed over the Internet.

Typical static website files:

-   `index.html`
-   `style.css`
-   `script.js`
-   Images

Basic flow:

``` text
User
  |
  v
Website URL
  |
  v
S3 Bucket
  |
  +-- index.html
  +-- style.css
  +-- script.js
```

The course shows region-dependent S3 website endpoint formats. If a
website returns **403 Forbidden**, check that the required bucket policy
allows public reads.

------------------------------------------------------------------------

# 15. S3 Versioning

**Versioning** keeps multiple versions of an object. It is enabled at
the **bucket level**.

Example:

``` text
report.pdf
   |
   +-- Version 1
   +-- Version 2
   +-- Version 3
```

Benefits:

-   Protects against unintended deletes
-   Allows restoring an older version
-   Makes rollback easy

### Important notes

-   Versioning is enabled at the bucket level.
-   A file that existed before versioning was enabled can have version
    ID `null`.
-   Suspending versioning does not delete previous versions.
-   With versioning enabled, a delete can create a delete marker while
    older versions remain available.

------------------------------------------------------------------------

# 16. S3 Replication

S3 replication copies objects from one bucket to another.

Two important types:

-   **CRR = Cross-Region Replication**
-   **SRR = Same-Region Replication**

## CRR

Copies objects to a bucket in another AWS Region.

``` text
Bucket A
us-east-1
    |
    | asynchronous replication
    v
Bucket B
ap-south-1
```

Use cases from the course:

-   Compliance
-   Lower-latency access in another Region
-   Replication across accounts

## SRR

Copies objects to another bucket in the same Region.

Use cases:

-   Log aggregation
-   Live replication between production and test environments

### Important replication points

-   Versioning must be enabled in source and destination buckets.
-   Copying is asynchronous.
-   Buckets can be in different AWS accounts.
-   Proper IAM permissions must be given to S3.

**Asynchronous** means the destination copy can happen after the source
upload; it is not necessarily simultaneous.

------------------------------------------------------------------------

# 17. S3 Storage Classes

The course covers these storage classes:

1.  S3 Standard
2.  S3 Standard-Infrequent Access (Standard-IA)
3.  S3 One Zone-Infrequent Access (One Zone-IA)
4.  S3 Intelligent-Tiering
5.  S3 Glacier Instant Retrieval
6.  S3 Glacier Flexible Retrieval
7.  S3 Glacier Deep Archive

Objects can be moved between classes manually or with **S3 Lifecycle
configurations**.

General idea:

``` text
Frequently accessed
        |
        v
   S3 Standard

Less frequently accessed
        |
        v
   S3 IA classes

Archive data
        |
        v
   S3 Glacier classes
```

------------------------------------------------------------------------

# 18. Durability vs Availability

These two terms are important.

## Durability

Durability means how safely the stored data is protected from loss.

The course states S3 durability as **99.999999999% (11 nines)** and
notes this is the same for the listed storage classes.

## Availability

Availability means how readily the service/data is available when you
need it. Availability varies by storage class.

Easy difference:

``` text
Durability  = Will my data be lost?
Availability = Can I access it when I need it?
```

------------------------------------------------------------------------

# 19. S3 Standard

S3 Standard is the general-purpose class for **frequently accessed
data**.

Course characteristics:

-   99.99% availability
-   Low latency
-   High throughput
-   Designed to sustain concurrent facility failures

Use cases:

-   Big-data analytics
-   Mobile applications
-   Gaming applications
-   Content distribution

Example:

``` text
Popular website images
        |
        v
   S3 Standard
```

------------------------------------------------------------------------

# 20. S3 Standard-IA

**IA = Infrequent Access.**

Use it for data that is accessed less frequently but still needs rapid
access when needed.

It costs less than S3 Standard for storage.

Course use cases:

-   Disaster recovery
-   Backups

Example:

``` text
Rarely opened backup
        |
        v
    Standard-IA
```

------------------------------------------------------------------------

# 21. S3 One Zone-IA

One Zone-IA is for infrequently accessed data stored in **one
Availability Zone**.

Course characteristics:

-   High durability in a single AZ
-   99.5% availability
-   Data can be lost if that AZ is destroyed

Use it for data that can be recreated, such as secondary backup copies.

Easy idea:

``` text
One Zone-IA
     |
     v
 One AZ only
```

------------------------------------------------------------------------

# 22. S3 Glacier Storage Classes

S3 Glacier classes are low-cost object storage classes intended for
**archiving and backup**.

Main Glacier classes:

-   Glacier Instant Retrieval
-   Glacier Flexible Retrieval
-   Glacier Deep Archive

In general, Glacier trades retrieval speed/cost characteristics for
cheaper storage.

------------------------------------------------------------------------

# 23. Glacier Instant Retrieval

Use it when archived data is rarely accessed but needs very fast
retrieval.

Course points:

-   Millisecond retrieval
-   Good for data accessed around once a quarter
-   Minimum storage duration: 90 days

Example:

``` text
Old data + very fast retrieval needed
            |
            v
   Glacier Instant Retrieval
```

------------------------------------------------------------------------

# 24. Glacier Flexible Retrieval

For archive data where retrieval can take longer.

Course retrieval options:

-   Expedited: about 1--5 minutes
-   Standard: about 3--5 hours
-   Bulk: about 5--12 hours
-   Minimum storage duration: 90 days

Example: old records that are rarely needed.

------------------------------------------------------------------------

# 25. Glacier Deep Archive

For **long-term archival**.

Course retrieval times:

-   Standard: about 12 hours
-   Bulk: about 48 hours
-   Minimum storage duration: 180 days

Example:

``` text
Years-old records that must be retained
                |
                v
      Glacier Deep Archive
```

------------------------------------------------------------------------

# 26. S3 Intelligent-Tiering

Intelligent-Tiering automatically moves objects between access tiers
based on usage.

There is a small monitoring/auto-tiering fee. The course states there
are **no retrieval charges** in Intelligent-Tiering.

Course tiers:

-   Frequent Access --- default
-   Infrequent Access --- objects not accessed for 30 days
-   Archive Instant Access --- objects not accessed for 90 days
-   Archive Access --- optional, configurable
-   Deep Archive Access --- optional, configurable

Simple flow:

``` text
New object
   |
   v
Frequent Access
   |
   | not accessed
   v
Infrequent Access
   |
   | longer period
   v
Archive tier
```

It is useful when you do not know how often data will be accessed.

------------------------------------------------------------------------

# 27. Storage Class Quick Comparison

  -----------------------------------------------------------------------
  Storage Class           Best For                Main Idea
  ----------------------- ----------------------- -----------------------
  S3 Standard             Frequently accessed     General purpose
                          data                    

  Standard-IA             Less frequent access    Lower storage cost +
                                                  rapid access

  One Zone-IA             Re-creatable infrequent One AZ
                          data                    

  Intelligent-Tiering     Unknown/changing access Automatic tiering

  Glacier Instant         Archive needing very    Millisecond retrieval
  Retrieval               fast access             

  Glacier Flexible        Archive with            Flexible retrieval
  Retrieval               minutes/hours           
                          acceptable              

  Glacier Deep Archive    Long-term archive       Slowest retrieval,
                                                  low-cost archive
  -----------------------------------------------------------------------

------------------------------------------------------------------------

# 28. S3 Express One Zone

S3 Express One Zone is a high-performance storage class.

Course points:

-   High performance
-   Data stored in a single Availability Zone
-   Very high request rates
-   Single-digit millisecond latency
-   Designed for latency-sensitive and data-intensive applications
-   Useful for AI/ML training, financial modeling, media processing, and
    HPC
-   Co-locating compute and storage in the same AZ can reduce latency

Simple idea:

``` text
Compute
   |
   | same AZ
   v
S3 Express One Zone
```

------------------------------------------------------------------------

# 29. S3 Encryption

Encryption protects S3 data.

The course shows two basic approaches.

## Server-Side Encryption

The user uploads the file and S3 encrypts it after receiving it.

``` text
User
  |
  | upload
  v
S3
  |
  | encrypt
  v
Encrypted object
```

## Client-Side Encryption

The file is encrypted **before** uploading it to S3.

``` text
User
  |
  | encrypt
  v
Encrypted file
  |
  | upload
  v
S3
```

Easy memory:

> Server-side = S3 encrypts.
>
> Client-side = client encrypts before upload.

------------------------------------------------------------------------

# 30. IAM Access Analyzer for S3

IAM Access Analyzer for S3 helps identify whether S3 buckets have access
that may not be intended.

It can evaluate:

-   S3 Bucket Policies
-   S3 ACLs
-   S3 Access Point Policies

Example: it can help identify a bucket that is publicly accessible or
shared with another AWS account when that access was not intended.

------------------------------------------------------------------------

# 31. S3 Shared Responsibility Model

AWS and the customer have different responsibilities.

## AWS responsibility

AWS is responsible for the underlying cloud infrastructure, including
areas such as:

-   Global security
-   Durability
-   Availability
-   Underlying infrastructure
-   Compliance validation

## Customer responsibility

The customer is responsible for configuration and data-related settings
such as:

-   S3 Versioning
-   S3 Bucket Policies
-   Replication setup
-   Logging and monitoring
-   Storage class selection
-   Data encryption configuration
-   Encryption in transit/at rest configuration

Easy memory:

``` text
AWS
 |
 +-- Security OF the cloud

Customer
 |
 +-- Security IN the cloud
```

------------------------------------------------------------------------

# 32. AWS Snowball

Snowball is a secure, portable physical AWS device used to
collect/process data at the edge and migrate large amounts of data into
or out of AWS.

The course describes it as useful for migrations of up to petabytes of
data.

Basic idea:

``` text
Your Data
   |
   v
Snowball Device
   |
   | physical shipping
   v
AWS
   |
   v
S3
```

------------------------------------------------------------------------

# 33. Why Snowball?

Network transfer can take a very long time when there is:

-   Limited connectivity
-   Limited bandwidth
-   High network cost
-   Shared bandwidth
-   Connection instability

The course gives transfer examples showing that very large datasets can
take days, months, or years at lower network speeds.

Course rule of thumb:

> If it takes more than about a week to transfer over the network,
> consider Snowball devices.

------------------------------------------------------------------------

# 34. Snowball Device Types in the Course

The provided course material lists:

### Snowball Edge Storage Optimized

-   104 vCPUs
-   416 GB memory
-   210 TB storage

### Snowball Edge Compute Optimized

-   104 vCPUs
-   416 GB memory
-   28 TB storage

These are course-slide specifications and may change over time.

------------------------------------------------------------------------

# 35. Direct S3 Transfer vs Snowball

## Direct upload

``` text
Client
  |
  | Internet
  v
S3 Bucket
```

## Snowball

``` text
Client
  |
  v
Snowball
  |
  | physical shipping
  v
AWS
  |
  v
S3 Bucket
```

Use the Snowball concept when network transfer is impractical.

------------------------------------------------------------------------

# 36. Edge Computing

Edge computing means processing data **where it is being created**,
instead of always sending everything to the central cloud first.

Examples from the course:

-   Truck on a road
-   Ship at sea
-   Mining station
-   Remote location with limited Internet

Example:

``` text
Sensors
   |
   v
Snowball Edge
   |
Process data locally
   |
   v
AWS later
```

The course lists these possible edge workloads:

-   Data preprocessing
-   Machine learning
-   Media transcoding
-   EC2 instances at the edge
-   Lambda functions at the edge

------------------------------------------------------------------------

# 37. Snowball Pricing

The course material describes Snowball pricing as including:

-   Device usage
-   Data transfer out of AWS
-   Data transfer IN to Amazon S3 shown as **\$0.00 per GB**

## On-Demand examples in the course

### 80 TB Storage Optimized

-   One-time service fee per job
-   Includes 10 days of usage

### 210 TB Storage Optimized

-   One-time service fee per job
-   Includes 15 days of usage

Shipping days are not counted in the included usage days. Additional
usage days are charged separately.

## Committed Upfront

The course also describes advance payment/commitment for monthly,
1-year, and 3-year edge-computing usage, with discounted pricing.

------------------------------------------------------------------------

# 38. Current Snowball Availability Note

The provided slides contain Snowball concepts and course pricing. AWS
product availability can change over time, so current hands-on ordering
should always be checked against current AWS documentation/console.

For the course/exam concept, remember:

**Snowball = physical device for large data migration and edge
computing.**

------------------------------------------------------------------------

# 39. Hybrid Cloud for Storage

Hybrid cloud means using both:

-   On-premises infrastructure
-   Cloud infrastructure

at the same time.

``` text
Company Data Center       AWS Cloud
       |                      |
   Local storage             S3
```

Reasons for hybrid cloud in the course:

-   Long cloud migrations
-   Security requirements
-   Compliance requirements
-   IT strategy

S3 is object storage and does not behave exactly like traditional
on-premises file storage. AWS Storage Gateway helps expose/use cloud
storage from on-premises environments.

------------------------------------------------------------------------

# 40. AWS Storage Gateway

**AWS Storage Gateway = bridge between on-premises data and AWS Cloud
data in S3.**

It is a hybrid storage service that lets on-premises environments use
AWS Cloud storage.

Course use cases:

-   Disaster recovery
-   Backup and restore
-   Tiered storage

The course lists three types:

1.  File Gateway
2.  Volume Gateway
3.  Tape Gateway

For the course/exam section, the slide says detailed knowledge of these
types is not required.

Simple diagram:

``` text
On-Premises
     |
     v
Storage Gateway
     |
     v
AWS Cloud / S3
```

Easy memory:

> Storage Gateway = bridge between on-premises storage and AWS Cloud/S3.

------------------------------------------------------------------------

# 41. AWS Storage Type Context

The course also compares storage models:

## Block storage

-   Amazon EBS
-   EC2 Instance Store

## File storage

-   Amazon EFS

## Object storage / archive

-   Amazon S3
-   S3 Glacier

Simple memory:

``` text
BLOCK  -> EBS / Instance Store
FILE   -> EFS
OBJECT -> S3 / Glacier
```

------------------------------------------------------------------------

# 42. S3 Important Exam Concepts

### Bucket

Container for S3 objects.

### Object

A file/data stored in S3.

### Key

Full object name/path.

### Maximum single object

**5 TB.**

### Bucket total size

No fixed maximum total storage size.

### Versioning

Keeps multiple versions of objects and helps recover from unintended
deletes/overwrites.

### Replication

Copies objects between buckets.

-   CRR = Cross-Region Replication
-   SRR = Same-Region Replication

### Storage classes

Choose based on access pattern, retrieval needs, resilience, and cost.

### Glacier

For archive/backup.

### Intelligent-Tiering

Automatically moves objects between access tiers based on usage.

### Encryption

Protects data.

### Bucket Policy

Resource-based access policy for S3.

### Block Public Access

Helps prevent accidental public exposure.

### Snowball

Physical device for large data migration and edge computing.

### Storage Gateway

Bridge between on-premises storage and AWS Cloud storage.

------------------------------------------------------------------------

# 43. Easy Real-Life Example

Imagine you run an online shopping website.

You have:

``` text
Website
  |
  +-- Product images
  +-- Product videos
  +-- Customer documents
  +-- Backups
  +-- Logs
```

Store them in an S3 bucket:

``` text
S3 Bucket
 |
 +-- products/
 |     +-- shoes.jpg
 |     +-- phone.jpg
 |
 +-- documents/
 |     +-- invoice.pdf
 |
 +-- backups/
       +-- backup.zip
```

Use storage classes according to access:

``` text
Frequently used product images -> S3 Standard
Less frequently used backups   -> Standard-IA
Long-term old backups          -> Glacier
Unknown access pattern         -> Intelligent-Tiering
```

Other requirements:

``` text
Accidental delete protection -> Versioning
Another Region copy         -> CRR
Huge data migration          -> Snowball concept
On-premises + S3             -> Storage Gateway
```

------------------------------------------------------------------------

# 44. Full S3 Flow

``` text
                    AMAZON S3
                         |
        +----------------+----------------+
        |                |                |
     BUCKETS          SECURITY         STORAGE
        |                |                |
     OBJECTS        IAM / Policies     Standard
        |             Encryption          IA
        |          Block Public Access    One Zone-IA
        |                                  Intelligent
        |                                  Glacier
        |
    Versioning
        |
   +----+----+
   |         |
  CRR       SRR

Large data migration
        |
     Snowball

On-premises + AWS
        |
 Storage Gateway
```

------------------------------------------------------------------------

# 45. Final S3 Cheat Sheet

  Concept                 Remember
  ----------------------- ------------------------------------------------
  S3 full form            Simple Storage Service
  Storage type            Object storage
  Bucket                  Container for objects
  Bucket name             Globally unique
  Bucket region           Bucket is created in a Region
  Object                  File/data stored in S3
  Key                     Full object name/path
  Single object maximum   5 TB
  Multipart upload        Upload large objects in parts
  IAM Policy              Identity-based permissions
  Bucket Policy           Resource-based S3 permissions
  Block Public Access     Helps prevent public exposure
  Encryption              Protects data
  Versioning              Keeps object versions
  CRR                     Cross-Region Replication
  SRR                     Same-Region Replication
  Replication             Asynchronous
  Standard                Frequently accessed data
  Standard-IA             Infrequent access + rapid retrieval
  One Zone-IA             Infrequent, re-creatable data in one AZ
  Intelligent-Tiering     Automatic tiering
  Glacier Instant         Archive + very fast retrieval
  Glacier Flexible        Archive + retrieval in minutes/hours
  Glacier Deep Archive    Long-term archive
  S3 Express One Zone     High-performance single-AZ storage
  Snowball                Physical large-data migration / edge computing
  Storage Gateway         On-premises + cloud storage bridge

------------------------------------------------------------------------

# 46. One-Line Memory Tricks

``` text
S3       = Object storage
Bucket   = Container
Object   = File
Key      = Full object path/name
5 TB     = Maximum size of one S3 object
Versioning = Keep old versions
CRR      = Copy to another Region
SRR      = Copy in same Region
Standard = Frequently accessed
IA       = Infrequently accessed
Glacier  = Archive
Intelligent-Tiering = AWS automatically moves objects between tiers
Encryption = Protect data
Bucket Policy = Control resource access
Block Public Access = Prevent accidental public exposure
Snowball = Physical device for large data migration
Storage Gateway = On-premises <-> AWS storage bridge
```

------------------------------------------------------------------------

# 47. Quick Self-Test

### Q1. What is S3?

Amazon S3 is an object storage service.

### Q2. What is a bucket?

A bucket is a container for S3 objects.

### Q3. What is the maximum size of one S3 object?

**5 TB.**

### Q4. Does an S3 bucket have a fixed total storage limit?

There is no fixed maximum total bucket size.

### Q5. What protects against accidental object deletion/overwrite?

**S3 Versioning.**

### Q6. What is CRR?

**Cross-Region Replication.**

### Q7. What is SRR?

**Same-Region Replication.**

### Q8. Which class is good for frequently accessed data?

**S3 Standard.**

### Q9. Which classes are mainly used for archive?

**S3 Glacier classes.**

### Q10. What does Intelligent-Tiering do?

It automatically moves objects between access tiers based on usage.

### Q11. What is Snowball?

A physical AWS device used for large-scale data migration and edge
computing.

### Q12. What is Storage Gateway?

A hybrid service that connects on-premises storage environments with AWS
Cloud storage.

------------------------------------------------------------------------

# 48. Final Concept

> **Amazon S3 is AWS object storage. You create buckets and store
> objects inside them. You control access using IAM policies, bucket
> policies, ACLs, Block Public Access, and encryption. Versioning
> protects previous versions, replication creates copies, storage
> classes match different access patterns, Snowball is the physical
> large-data migration/edge-computing concept, and Storage Gateway
> connects on-premises storage with AWS Cloud storage.**
