
# 1. Databases Introduction

## Why do we need databases?

Data can be stored on disk using services such as EFS, EBS, EC2 Instance Store, or S3. But normal storage has limitations when we need to **structure, search, and relate data**.

A database helps us:

- Structure data.
- Build indexes for efficient searching.
- Define relationships between datasets.
- Use a database optimized for a particular purpose.

### Simple example

Suppose an online college application has 1,00,000 students.

With a database, we can store:

- Student ID
- Name
- Email
- Department
- Subjects

Then we can quickly ask:

```sql
SELECT * FROM Students WHERE Student_ID = 101;
```

The database can use indexes to find the required record efficiently.

---

# 2. Relational Databases

A **relational database** stores data in tables, similar to Excel spreadsheets, and tables can have relationships between them.

It commonly uses **SQL** for queries and lookups.

## Example

### Students table

| Student ID | Dept ID | Name | Email |
|---|---|---|---|
| 1 | M01 | Joe Miller | joe@abc.com |
| 2 | B01 | Sarah T | sarah@abc.com |

### Subjects table

| Student ID | Subject |
|---|---|
| 1 | Physics |
| 1 | Chemistry |
| 1 | Math |
| 2 | History |
| 2 | Geography |
| 2 | Economics |

### Departments table

| Dept ID | SPOC | Email | Phone |
|---|---|---|---|
| M01 | Kelly Jones | kelly@abc.com | +1234567890 |
| B01 | Satish Kumar | satish@abc.com | +1234567891 |

Here `Student ID` connects Students and Subjects, while `Dept ID` connects Students and Departments.

### AWS examples

- Amazon RDS
- Amazon Aurora

---

# 3. NoSQL Databases

**NoSQL = non-SQL / non-relational database.**

NoSQL databases are purpose-built for specific data models and use **flexible schemas**.

## Main benefits

### 1. Flexibility
The data model can evolve easily.

### 2. Scalability
Designed to scale out using distributed clusters.

### 3. High performance
Optimized for a particular data model.

### 4. Highly functional
Provides features optimized for the selected data model.

## NoSQL types mentioned in the slides

- Key-value
- Document
- Graph
- In-memory
- Search databases

---

# 4. JSON and NoSQL

JSON (JavaScript Object Notation) is a common format that fits a NoSQL model.

Example:

```json
{
  "name": "John",
  "age": 30,
  "cars": [
    "Ford",
    "BMW",
    "Fiat"
  ],
  "address": {
    "type": "house",
    "number": 23,
    "street": "Dream Road"
  }
}
```

## Why JSON fits NoSQL

Data can be:

- Nested.
- Changed over time.
- Extended with new types such as arrays.

### Simple idea

Relational:

```text
Student Table
ID | Name | Age
```

NoSQL document:

```text
{
  "id": 1,
  "name": "Sakshi",
  "age": 20,
  "subjects": ["AWS", "Java", "DBMS"]
}
```

The document can contain nested data directly.

---

# 5. Databases and AWS Shared Responsibility

AWS provides managed database services.

Benefits include:

- Quick provisioning.
- High availability.
- Vertical scaling.
- Horizontal scaling.
- Automated backup and restore.
- Automated operations and upgrades.
- OS patching handled by AWS.
- Monitoring and alerting.

## Database on EC2 vs Managed Database

You can run many database technologies on EC2, but then **you manage more responsibilities yourself**, such as:

- Resiliency.
- Backups.
- Patching.
- High availability.
- Fault tolerance.
- Scaling.

### Example

If you install MySQL yourself on EC2:

```text
EC2
 |
 +-- MySQL
 +-- OS patches -> You
 +-- Backups -> You
 +-- HA -> You
 +-- Scaling -> You
```

With RDS:

```text
Application
     |
     v
    RDS
     |
 AWS manages many database operations
```

---

# 6. Amazon RDS

**RDS = Relational Database Service.**

RDS is a **managed relational database service**.

It uses **SQL** as the query language.

AWS manages the database infrastructure for you.

## Database engines supported in the slides

- PostgreSQL
- MySQL
- MariaDB
- Oracle
- Microsoft SQL Server
- IBM DB2
- Aurora (AWS proprietary database)

---

# 7. RDS vs Database on EC2

RDS provides managed features such as:

- Automated provisioning.
- OS patching.
- Continuous backups.
- Point-in-Time Restore.
- Monitoring dashboards.
- Read Replicas.
- Multi-AZ setup for disaster recovery.
- Maintenance windows for upgrades.
- Vertical and horizontal scaling.
- Storage backed by EBS.

## Important limitation

With RDS:

**You cannot SSH into the database instances.**

### Example

Without RDS:

```text
EC2
 |
 +-- Install MySQL
 +-- Configure MySQL
 +-- Patch OS
 +-- Backup
 +-- Maintain HA
```

With RDS:

```text
Application
     |
     v
RDS MySQL
     |
 AWS manages infrastructure
```

---

# 8. RDS Architecture

A common architecture can be:

```text
Users
  |
  v
Elastic Load Balancer
  |
  v
EC2 Instances
(possibly in ASG)
  |
  v
RDS Database
(SQL / relational)
```

The EC2 application servers communicate with the RDS database.

---

# 9. Amazon Aurora

**Aurora** is an AWS proprietary database technology.

The slides describe it as AWS cloud-optimized.

## Compatible database types

Aurora supports:

- PostgreSQL
- MySQL

## Important features from the slides

- AWS proprietary technology.
- Supports PostgreSQL and MySQL.
- Designed for higher performance than standard RDS database engines.
- Storage automatically grows in 10 GB increments.
- Storage can scale up to 256 TB.
- Costs more than RDS according to the slide example, but can be more efficient.

### Simple example

If your application needs a MySQL-compatible relational database:

```text
Application
     |
     v
Aurora MySQL
```

Aurora provides AWS-managed database capabilities while maintaining MySQL compatibility.

---

# 10. Aurora Serverless

Aurora Serverless provides automated database instantiation and scaling based on actual usage.

## Features

- Automatically scales based on usage.
- No capacity planning required.
- Less management overhead.
- Pay per second.
- Supports Aurora PostgreSQL and Aurora MySQL.
- Useful for:
  - Infrequent workloads.
  - Intermittent workloads.
  - Unpredictable workloads.

### Example

Suppose a college result website is used heavily only during result time.

```text
Normal usage       -> Low capacity
Result announced   -> Capacity increases
Traffic decreases  -> Capacity decreases
```

This is a good type of workload for Aurora Serverless.

---

# 11. RDS Read Replicas

A **Read Replica** is used to scale the **read workload** of a database.

## How it works

```text
             +--> Read Replica --> READ
             |
Main DB -----+--> Read Replica --> READ
             |
             +--> Application --> WRITE
```

Important points from the slides:

- Data is written to the main database.
- Read Replicas receive replicated data.
- Applications can read from replicas.
- The slides state that up to 15 Read Replicas can be created.

## Example

An online shopping website has:

```text
1000 READ requests
100 WRITE requests
```

Instead of sending every read to the main DB:

```text
Main DB
  |
  +--> Read Replica 1
  +--> Read Replica 2
  +--> Read Replica 3
```

Reads can be distributed across replicas.

### Main purpose

**Read Replicas = improve read performance / scale reads.**

---

# 12. RDS Multi-AZ

Multi-AZ is mainly for **high availability and failover**.

The slides describe:

- A main database.
- A standby/failover database in another AZ.
- Replication across AZs.
- Applications read/write to the main database.
- If the main database has an issue, failover can occur.

### Diagram

```text
Application
     |
     v
 Main RDS
    |
    | replication
    v
Failover DB
(another AZ)
```

### Example

Main DB is in AZ-1.

If AZ-1 has an outage:

```text
AZ-1                 AZ-2
Main DB  ----->      Failover DB
   X                     |
                         v
                    Application
```

### Key point

**Multi-AZ = High Availability / Failover.**

It is not primarily for scaling read traffic.

---

# 13. Read Replica vs Multi-AZ

| Feature | Read Replica | Multi-AZ |
|---|---|---|
| Main purpose | Read scaling | High availability |
| Reads | Can read from replicas | Main DB handles reads/writes |
| Replication | Replica receives data | Cross-AZ replication |
| Failover | Not the primary purpose | Yes |
| Use case | Heavy read workload | AZ failure / disaster recovery |

### Easy memory trick

```text
Read Replica -> READ performance
Multi-AZ     -> AVAILABILITY
```

---

# 14. RDS Multi-Region Read Replicas

Multi-Region Read Replicas are used when replicas are placed in different AWS Regions.

## Benefits

### 1. Disaster recovery
Can help if a Region has an issue.

### 2. Local performance for global reads
Users can read from a database closer to them.

### 3. Global applications
Useful when applications are distributed across multiple Regions.

### Example

```text
                Main DB
              us-east-2
                  |
          replication
        /           \
       v             v
eu-west-1      ap-southeast-2
Read Replica    Read Replica
```

### Important

Multi-Region replication has a replication cost.

---

# 15. Amazon ElastiCache

ElastiCache provides managed:

- Redis
- Memcached

It is an **in-memory database/cache**.

## Why use a cache?

In-memory data access is very fast and has low latency.

It can reduce the load on a database, especially for read-intensive workloads.

AWS manages:

- OS maintenance.
- Patching.
- Optimizations.
- Setup.
- Configuration.
- Monitoring.
- Failure recovery.
- Backups.

---

# 16. ElastiCache Architecture

Without cache:

```text
User
 |
 v
EC2
 |
 v
RDS
 |
 v
Data
```

With cache:

```text
User
 |
 v
EC2
 |
 +------> ElastiCache
 |            |
 |            +--> Fast
 |
 +------> RDS
            |
            +--> Slower than cache
```

## Example

Suppose 10,000 users repeatedly request:

```text
"Today's product list"
```

Instead of querying RDS every time:

```text
First request -> RDS
                |
                v
             Cache

Next requests -> Cache
```

This reduces database load and improves response time.

### Key point

**ElastiCache = managed in-memory cache/database for fast access.**

---

# 17. DynamoDB

DynamoDB is:

- Fully managed.
- NoSQL.
- Highly available.
- A distributed/serverless database.
- Designed for massive workloads.
- Key/value based.
- Low latency.

The slides mention:

- Replication across 3 AZs.
- Millions of requests per second.
- Trillions of rows.
- Hundreds of TB of storage.
- Single-digit millisecond latency.
- IAM integration.
- Auto scaling.
- Standard and Standard-IA table classes.

## Example

For a shopping cart:

```text
User ID -> Cart Data
101     -> [Laptop, Mouse]
102     -> [Phone]
103     -> [Keyboard, Monitor]
```

This key/value model is suitable for DynamoDB.

### Important

```text
DynamoDB != RDS

RDS       -> Relational / SQL
DynamoDB  -> NoSQL / Key-value
```

---

# 18. DynamoDB Accelerator (DAX)

**DAX = DynamoDB Accelerator.**

DAX is a fully managed **in-memory cache for DynamoDB**.

The slides describe:

- Up to 10x performance improvement.
- Millisecond latency can be reduced to microseconds for cached access.
- Secure.
- Highly scalable.
- Highly available.

## DAX vs ElastiCache

### DAX

```text
Application
     |
     v
   DAX
     |
     v
DynamoDB
```

DAX is specifically integrated with DynamoDB.

### ElastiCache

```text
Application
     |
     v
ElastiCache
     |
     v
Database
```

ElastiCache can be used with other databases.

### Exam memory

**DAX = cache for DynamoDB only.**

**ElastiCache = general managed Redis/Memcached cache.**

---

# 19. DynamoDB Global Tables

Global Tables make a DynamoDB table accessible with low latency across multiple Regions.

They use **active-active replication**.

This means applications can read/write in multiple AWS Regions.

### Example

```text
Users in USA
     |
     v
DynamoDB Global Table
us-east-1
     ^
     | two-way replication
     v
DynamoDB Global Table
eu-west-3
     ^
     |
Users in Europe
```

A user can access the table from a nearby Region.

### Key point

**Global Tables = multi-Region DynamoDB + active-active read/write.**

---

# 20. Amazon Redshift

Redshift is an AWS data warehouse service.

It is based on PostgreSQL but is **not used for OLTP** in the slides.

Redshift is designed for **OLAP**:

> Online Analytical Processing

and data warehousing.

## OLTP vs OLAP

### OLTP

Used for frequent transactions:

```text
Buy product
Update account
Create order
Make payment
```

### OLAP

Used for analysis:

```text
What were total sales this year?
Which product sold the most?
Which region generated the most revenue?
```

---

# 21. Redshift Features

The slides mention:

- OLAP / analytics.
- Data warehousing.
- Columnar storage.
- Massively Parallel Query Execution (MPP).
- High availability.
- SQL interface.
- Scaling to very large datasets.
- Pay-as-you-go based on provisioned instances.
- Integration with BI tools such as QuickSight and Tableau.

## Columnar storage

Traditional relational tables are commonly thought of as row-based:

```text
ID | Name | City | Sales
```

Columnar storage groups data by columns.

This is useful for analytical queries that may need only a few columns from a very large dataset.

---

# 22. Redshift Example

Suppose Amazon has billions of sales records.

A business analyst asks:

```text
SELECT region, SUM(sales)
FROM sales
GROUP BY region;
```

This is an analytical workload.

A data warehouse such as Redshift is designed for this type of query.

### Easy memory

```text
RDS / Aurora -> OLTP
Redshift     -> OLAP
```

---

# 23. Amazon Redshift Serverless

Redshift Serverless automatically provisions and scales the underlying data warehouse capacity.

You don't have to manage the data warehouse infrastructure yourself.

## Features

- Automatically provisions capacity.
- Automatically scales based on workloads.
- Pay only for what you use.
- Useful for:
  - Reporting.
  - Dashboarding.
  - Real-time analytics.

### Example

```text
Low analytics traffic
        ↓
Less capacity

Heavy analytics traffic
        ↓
More capacity
```

---

# 24. Amazon EMR

**EMR = Elastic MapReduce.**

EMR helps create Hadoop clusters for processing and analyzing very large amounts of data.

Clusters can contain hundreds of EC2 instances.

## Technologies mentioned

EMR supports:

- Apache Hadoop
- Apache Spark
- HBase
- Presto
- Flink

AWS handles provisioning and configuration.

It also supports:

- Auto scaling.
- Spot Instances.

## Use cases

- Big data processing.
- Machine learning.
- Web indexing.
- Large-scale data analysis.

### Example

Suppose a company has billions of website logs.

```text
Huge Log Data
     |
     v
    EMR
     |
     +--> Process
     +--> Analyze
     +--> Generate results
```

---

# 25. Amazon Athena

Athena is a **serverless query service** used to analyze data stored in **Amazon S3**.

It uses standard SQL.

## Supported formats in the slides

- CSV
- JSON
- ORC
- Avro
- Parquet

It is built on Presto.

## Pricing concept from the slides

The slide gives an example price of:

**$5 per TB of data scanned.**

Using compressed or columnar data can reduce the amount of data scanned and therefore reduce cost.

## Example

Suppose:

```text
S3
 |
 +-- sales.csv
 +-- customers.json
 +-- logs.parquet
```

Use Athena:

```text
S3
 |
 v
Athena
 |
 +--> SQL query
 |
 v
Results
```

### Exam tip

**Analyze data in S3 using serverless SQL -> Athena.**

---

# 26. Athena + QuickSight Architecture

A common analytics flow from the slides is:

```text
S3 Bucket
    |
    v
  Athena
    |
    v
Query & Analyze
    |
    v
QuickSight
    |
    v
Reports / Dashboards
```

Example:

```text
S3 contains sales.csv
        |
        v
Athena queries sales.csv
        |
        v
QuickSight creates dashboard
```

---

# 27. Amazon QuickSight

QuickSight is a serverless, machine-learning-powered **business intelligence (BI)** service.

It is used to create interactive dashboards.

## Use cases

- Business analytics.
- Visualizations.
- Ad-hoc analysis.
- Business insights.
- Dashboards.

## Integrations mentioned

QuickSight integrates with:

- RDS
- Aurora
- Athena
- Redshift
- S3

### Example

```text
Sales Data
    |
    v
Redshift / Athena / RDS
    |
    v
QuickSight
    |
    v
Interactive Dashboard
```

---

# 28. Amazon DocumentDB

DocumentDB is a managed database service associated with MongoDB-compatible document workloads.

The slides compare the concept to Aurora:

```text
Aurora    -> AWS implementation of PostgreSQL / MySQL
DocumentDB -> AWS service for MongoDB workloads
```

## Important points

- MongoDB is a NoSQL database.
- MongoDB stores, queries, and indexes JSON data.
- DocumentDB is fully managed.
- Highly available with replication across 3 AZs.
- Storage automatically grows in 10 GB increments.
- Designed to scale to workloads with millions of requests per second.

### Example document

```json
{
  "customer": "Sakshi",
  "orders": [
    {
      "product": "Laptop",
      "price": 50000
    },
    {
      "product": "Mouse",
      "price": 1000
    }
  ]
}
```

This kind of nested JSON/document data fits a document database model.

---

# 29. Amazon Neptune

Neptune is a **fully managed graph database**.

It is designed for highly connected datasets.

## Graph example

A social network has:

```text
User A
 |
 | friend
 v
User B
 |
 | likes
 v
Post C
 |
 | comment
 v
User D
```

These relationships are naturally represented as a graph.

## Use cases

- Social networking.
- Knowledge graphs.
- Fraud detection.
- Recommendation engines.

The slides mention:

- High availability across 3 AZs.
- Up to 15 read replicas.
- Billions of relationships.
- Millisecond-level graph queries.

### Key point

**Neptune = Graph database.**

---

# 30. Amazon Timestream

Timestream is a fully managed, fast, scalable, serverless **time-series database**.

A time-series database stores data associated with time.

## Examples

### Temperature sensor

```text
10:00 -> 25°C
10:01 -> 25.5°C
10:02 -> 26°C
```

### CPU monitoring

```text
10:00 -> 45%
10:01 -> 51%
10:02 -> 60%
```

## Features from the slides

- Automatically scales up/down.
- Handles very large numbers of events.
- Built-in time-series analytics.
- Helps identify patterns in near real time.

### Key point

**Timestream = time-series data.**

---

# 31. Amazon Managed Blockchain

Blockchain allows multiple parties to execute transactions without requiring a trusted central authority.

Amazon Managed Blockchain is a managed service to:

- Join public blockchain networks.
- Create scalable private blockchain networks.

The slides mention compatibility with:

- Hyperledger Fabric
- Ethereum

### Example

Multiple companies need to share transaction records:

```text
Company A
    |
    v
Blockchain Network
   / \
  v   v
Company B   Company C
```

No single company necessarily has to be the central trusted authority for the blockchain network.

---

# 32. AWS Glue

AWS Glue is a managed **ETL** service.

ETL means:

- **E = Extract**
- **T = Transform**
- **L = Load**

Glue is useful for preparing and transforming data for analytics.

It is serverless.

## Example

Suppose raw data exists in:

```text
S3
sales.csv
```

and another source contains customer data:

```text
RDS
Customers table
```

Glue can:

```text
S3 + RDS
   |
   v
Extract
   |
   v
Transform
(clean / format / combine)
   |
   v
Load
   |
   v
Redshift
```

---

# 33. Glue Data Catalog

Glue also provides the **Glue Data Catalog**.

The Data Catalog is a catalog of datasets.

It can be used by:

- Athena
- Redshift
- EMR

### Simple idea

Imagine a library.

The actual books are the data.

The library catalog tells you:

```text
Book name
Author
Category
Location
```

Similarly, the Data Catalog stores metadata about datasets so analytics services can understand and work with them.

### Important distinction

```text
Actual data       -> S3 / RDS / other storage
Data Catalog      -> information/metadata about the datasets
```

So, the Data Catalog is **not simply another place where your actual business data is stored**.

---

# 34. DMS — Database Migration Service

**DMS = Database Migration Service.**

DMS is used to migrate databases to AWS.

## Features from the slides

- Quick and secure database migration.
- Resilient and self-healing.
- Source database remains available during migration.

## Two migration types

### 1. Homogeneous migration

Same database technology:

```text
Oracle
  |
  v
Oracle
```

### 2. Heterogeneous migration

Different database technologies:

```text
Microsoft SQL Server
        |
        v
      Aurora
```

## Basic architecture

```text
Source Database
      |
      v
EC2 running DMS
      |
      v
Target Database
```

---

# 35. Complete AWS Database & Analytics Map

Use this table for quick revision.

| AWS Service | Main Purpose | Database / Tool Type |
|---|---|---|
| RDS | Managed relational DB | SQL / OLTP |
| Aurora | AWS cloud-optimized relational DB | MySQL/PostgreSQL compatible |
| ElastiCache | Fast in-memory cache | Redis/Memcached |
| DynamoDB | Massive-scale NoSQL | Key-value |
| DAX | Cache for DynamoDB | In-memory |
| Redshift | Data warehouse / analytics | OLAP / SQL |
| EMR | Big data processing | Hadoop/Spark/etc. |
| Athena | Query S3 data | Serverless SQL |
| QuickSight | Dashboards / BI | Visualization |
| DocumentDB | Document database | MongoDB workloads / JSON |
| Neptune | Graph database | Relationships/graphs |
| Timestream | Time-series database | Time-based data |
| Managed Blockchain | Blockchain networks | Hyperledger/Ethereum |
| Glue | ETL + Data Catalog | Data preparation/metadata |
| DMS | Database migration | Migration service |

---

# 36. Most Important Comparisons

## RDS vs DynamoDB

```text
RDS
 |
 +-- Relational
 +-- SQL
 +-- Tables
 +-- Relationships
 +-- OLTP

DynamoDB
 |
 +-- NoSQL
 +-- Key-value
 +-- Serverless/distributed
 +-- Massive scale
```

---

## RDS vs Aurora

```text
RDS
 |
 +-- Managed relational database service
 +-- Supports several database engines

Aurora
 |
 +-- AWS proprietary database technology
 +-- MySQL/PostgreSQL compatible
 +-- AWS cloud optimized
```

---

## Read Replica vs Multi-AZ

```text
Read Replica
     |
     +--> Scale READ traffic

Multi-AZ
     |
     +--> High Availability
     +--> Failover
```

---

## ElastiCache vs DAX

```text
ElastiCache
 |
 +--> Managed Redis/Memcached
 +--> Can cache data for different databases

DAX
 |
 +--> DynamoDB Accelerator
 +--> Specifically for DynamoDB
```

---

## RDS vs Redshift

```text
RDS
 |
 +--> OLTP
 +--> Transactions
 +--> Frequent reads/writes

Redshift
 |
 +--> OLAP
 +--> Analytics
 +--> Data warehouse
```

---

## Athena vs Redshift

```text
Athena
 |
 +--> Query data directly in S3
 +--> Serverless
 +--> SQL

Redshift
 |
 +--> Data warehouse
 +--> OLAP
 +--> SQL
```

---

# 37. Easy Service Selection — Exam Style

### Question: Need a managed relational database?

**Answer: RDS**

### Question: Need MySQL/PostgreSQL-compatible AWS-optimized relational database?

**Answer: Aurora**

### Question: Need a database for massive key-value workloads?

**Answer: DynamoDB**

### Question: Need a cache for DynamoDB?

**Answer: DAX**

### Question: Need a managed Redis/Memcached cache?

**Answer: ElastiCache**

### Question: Need high availability/failover for RDS?

**Answer: Multi-AZ**

### Question: Need more read capacity for RDS?

**Answer: Read Replicas**

### Question: Need database replicas in multiple Regions?

**Answer: RDS Multi-Region Read Replicas**

### Question: Need data warehouse / OLAP?

**Answer: Redshift**

### Question: Need serverless SQL queries on S3?

**Answer: Athena**

### Question: Need dashboards and BI?

**Answer: QuickSight**

### Question: Need Hadoop/large-scale big data processing?

**Answer: EMR**

### Question: Need ETL?

**Answer: Glue**

### Question: Need metadata/catalog of datasets?

**Answer: Glue Data Catalog**

### Question: Need migrate a database to AWS?

**Answer: DMS**

### Question: Need MongoDB/document/JSON workloads?

**Answer: DocumentDB**

### Question: Need graph relationships?

**Answer: Neptune**

### Question: Need time-based sensor/metric data?

**Answer: Timestream**

### Question: Need blockchain network?

**Answer: Amazon Managed Blockchain**

---

# 38. Complete Analytics Example

Suppose an online shopping company collects:

- Orders from RDS.
- Raw logs in S3.
- Customer data in RDS.
- Huge historical datasets.

A possible pipeline is:

```text
                 RDS
                  |
                  | Extract
                  v
S3 ------------> AWS Glue
                  |
                  | Transform
                  v
               Redshift
                  |
                  v
             QuickSight
                  |
                  v
          Business Dashboard
```

For raw S3 data that does not need to be loaded into a warehouse:

```text
S3
 |
 v
Athena
 |
 v
SQL Analysis
 |
 v
QuickSight
```

For database migration:

```text
Old Database
     |
     v
    DMS
     |
     v
AWS Database
```

---

# 39. Page-by-Page Topic Map

| PDF Page | Topic |
|---:|---|
| 168 | Databases Section |
| 169 | Databases Introduction |
| 170 | Relational Databases |
| 171 | NoSQL Databases |
| 172 | NoSQL JSON Example |
| 173 | Databases & Shared Responsibility |
| 174 | Amazon RDS Overview |
| 175 | RDS advantages vs EC2 |
| 176 | RDS solution architecture |
| 177 | Amazon Aurora |
| 178 | Aurora Serverless |
| 179 | RDS Read Replicas & Multi-AZ |
| 180 | RDS Multi-Region |
| 181 | ElastiCache Overview |
| 182 | ElastiCache Architecture |
| 183 | DynamoDB |
| 184 | DynamoDB key/value data |
| 185 | DAX |
| 186 | DynamoDB Global Tables |
| 187 | Redshift |
| 188 | Redshift Serverless |
| 189 | EMR |
| 190 | Athena |
| 191 | QuickSight |
| 192 | DocumentDB |
| 193 | Neptune |
| 194 | Timestream |
| 195 | Managed Blockchain |
| 196 | AWS Glue |
| 197 | DMS |
| 198 | Databases & Analytics Summary |

---

# 40. Final Exam Cheat Sheet

```text
RELATIONAL / OLTP
RDS
Aurora

IN-MEMORY
ElastiCache
DAX -> DynamoDB cache

NOSQL
DynamoDB -> Key-value
DocumentDB -> Document / JSON
Neptune -> Graph
Timestream -> Time-series

ANALYTICS / OLAP
Redshift -> Data warehouse
Athena -> SQL on S3
EMR -> Big Data / Hadoop
QuickSight -> Dashboards

DATA ENGINEERING
Glue -> ETL
Glue Data Catalog -> Dataset metadata

MIGRATION
DMS -> Database migration

BLOCKCHAIN
Managed Blockchain
```

## One-line memory trick

**RDS/Aurora = relational, DynamoDB = key-value, ElastiCache = cache, Redshift = warehouse, Athena = S3 SQL, EMR = big data, QuickSight = dashboard, Glue = ETL/catalog, DMS = migration, Neptune = graph, Timestream = time-series, DocumentDB = document, Blockchain = blockchain.**

---


