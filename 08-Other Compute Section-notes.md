# 1. Docker

## What is Docker?

**Docker is a software development platform used to package and deploy applications in containers.**

A **container** contains the application and the things the application needs to run.

For example, suppose we create a Node.js application.

Normally, the application may need:

- Node.js
- Some libraries
- Configuration
- Application code

If we move the application to another computer, we may face compatibility problems.

With Docker, we package the application and its required environment into a **Docker image** and run it as a container.

### Simple idea

```text
Application
     +
Required libraries/configuration
     ↓
Docker Image
     ↓
Docker Container
     ↓
Runs on a machine
```

## Why use Docker?

Docker gives:

- Same application behavior in different environments
- Fewer compatibility problems
- Predictable behavior
- Easier deployment
- Easier maintenance
- Fast scaling of containers

Docker can be used with different:

- Programming languages
- Operating systems
- Technologies

### Real-life example

Imagine you developed a Node.js website on your laptop.

It works on your laptop, but when you move it to another server, some package or Node.js version is different.

Docker packages the application environment so that the application can run consistently.

### AWS example

You can create a Docker image for your application and run that container using:

- Amazon ECS
- AWS Fargate
- Amazon EKS

---

# 2. Docker Image

A **Docker image** is a package/template used to create a container.

Example:

```text
Node.js Application
        +
Node.js
        +
Required libraries
        ↓
   Docker Image
        ↓
   Container
```

Think of:

- **Docker Image = Blueprint**
- **Docker Container = Running application created from the blueprint**

### Example

Suppose you have:

```text
my-node-app
```

You create a Docker image:

```text
my-node-app-image
```

When you run that image:

```text
my-node-app-image
        ↓
Docker Container
```

The container runs your application.

---

# 3. Where are Docker Images Stored?

Docker images are stored in **Docker repositories**.

There are two important examples:

## Public Repository — Docker Hub

**Docker Hub** is a public Docker image repository.

It contains images for technologies such as:

- Ubuntu
- MySQL
- Node.js
- Java

Example:

```text
Docker Hub
   ↓
MySQL image
   ↓
Download image
   ↓
Run MySQL container
```

## Private Repository — Amazon ECR

**Amazon ECR = Elastic Container Registry**

ECR is a **private Docker image repository on AWS**.

You can store your organization's Docker images in ECR.

Example:

```text
Developer
   ↓
Build Docker Image
   ↓
Amazon ECR
   ↓
ECS / Fargate
   ↓
Run Container
```

---

# 4. Docker vs Virtual Machine

Docker containers and Virtual Machines are not the same.

## Virtual Machine

A VM normally has:

```text
Physical Infrastructure
        ↓
     Host OS
        ↓
    Hypervisor
        ↓
   Guest OS
   ┌─────────────┐
   │ Application │
   └─────────────┘
```

Each VM has its own guest operating system.

## Docker Container

Containers share resources with the host operating system.

```text
Infrastructure
      ↓
Host OS / EC2
      ↓
Docker Daemon
      ↓
Container
Container
Container
Container
```

Therefore, many containers can run on one server.

## Easy difference

| Docker Container | Virtual Machine |
|---|---|
| Lightweight | Heavier |
| Shares host resources | Has a guest OS |
| Starts quickly | Usually takes more time |
| Many containers can run on one server | VMs need more resources |
| Good for packaging applications | Good for complete isolated machines |

### Easy example

Suppose one EC2 server has enough resources.

You can run:

```text
EC2
 ├── Container 1 → Node.js app
 ├── Container 2 → Python app
 └── Container 3 → another service
```

---

# 5. Amazon ECS

## Full form

**ECS = Elastic Container Service**

Amazon ECS is an AWS service used to **run Docker containers on AWS**.

According to the slides, with ECS:

- You run Docker containers on AWS.
- You must provision and maintain the infrastructure.
- The infrastructure can be EC2 instances.
- AWS takes care of starting and stopping containers.
- ECS integrates with an Application Load Balancer.

### Important point

With ECS + EC2:

**You manage the EC2 infrastructure.**

### AWS example

Suppose you have an online shopping application.

```text
Users
  ↓
Application Load Balancer
  ↓
ECS Service
  ↓
EC2      EC2      EC2
 ↓        ↓        ↓
Container Container Container
```

ECS can manage the containers running on those EC2 instances.

### Simple meaning

**ECS = AWS service for running Docker containers, where with the EC2 launch type you manage the EC2 infrastructure.**

---

# 6. AWS Fargate

Fargate also runs Docker containers on AWS.

The important difference is:

**You do not need to provision or manage EC2 instances.**

Fargate is a **serverless offering for containers**.

AWS runs the containers for you based on the CPU and RAM you specify.

### ECS vs Fargate

```text
ECS + EC2

You
 ↓
Manage EC2
 ↓
ECS
 ↓
Containers
```

```text
ECS + Fargate

You
 ↓
Choose CPU + RAM
 ↓
Fargate
 ↓
Containers
```

### Easy example

Suppose you have a Node.js Docker application.

With ECS + EC2:

```text
You create EC2
      ↓
Maintain EC2
      ↓
Run container using ECS
```

With Fargate:

```text
Docker image
      ↓
Fargate
      ↓
Container runs
```

You don't manage the underlying EC2 instances.

### Exam point

**Fargate = Run containers without managing servers/EC2 infrastructure.**

---

# 7. Amazon ECR

## Full form

**ECR = Elastic Container Registry**

ECR is a **private Docker image repository on AWS**.

Its main purpose is:

**Store Docker images.**

Those images can then be used by:

- ECS
- Fargate

### Example

```text
Developer
   ↓
Docker Image
   ↓
Amazon ECR
   ↓
ECS / Fargate
   ↓
Docker Container
```

### Easy memory trick

**ECR = Store Docker Images**

**ECS = Run Docker Containers**

**Fargate = Run Containers without managing EC2**

---

# 8. Amazon EKS

## Full form

**EKS = Elastic Kubernetes Service**

EKS is AWS's managed Kubernetes service.

### What is Kubernetes?

Kubernetes is an open-source system used for:

- Managing containers
- Deploying containerized applications
- Scaling containerized applications

Kubernetes is **cloud-agnostic**, meaning it can be used with different cloud providers such as:

- AWS
- Azure
- Google Cloud

### EKS

Amazon EKS lets you launch **managed Kubernetes clusters on AWS**.

Containers in EKS can run on:

- EC2 instances
- Fargate

### AWS example

Suppose a company has hundreds of containers.

Kubernetes can help manage those containers.

```text
Amazon EKS
    ↓
Kubernetes Cluster
    ↓
Pods
    ↓
Containers
```

The containers can run on:

```text
EC2 Nodes
OR
Fargate
```

### Exam point

**EKS = Managed Kubernetes on AWS**

---

# 9. ECS vs EKS

This is an important exam comparison.

| ECS | EKS |
|---|---|
| AWS container orchestration service | Managed Kubernetes service |
| Uses AWS ECS technology | Uses Kubernetes |
| Runs Docker containers | Manages Kubernetes workloads |
| AWS-specific service | Kubernetes is cloud-agnostic |
| Can use EC2/Fargate | Can use EC2/Fargate |

### Easy memory

**ECS → AWS's own container orchestration service**

**EKS → Kubernetes on AWS**

---

# 10. What is Serverless?

Serverless is a computing model where **developers do not have to manage servers**.

Important:

**Serverless does NOT mean there are no servers.**

Servers still exist.

AWS manages the underlying servers for you.

You mainly:

```text
Write code
   ↓
Deploy code
   ↓
AWS manages infrastructure
```

The slides explain that serverless was pioneered by AWS Lambda and now the idea can include managed services such as databases, messaging and storage.

### Examples of serverless services from the course

- AWS Lambda
- Amazon S3
- DynamoDB
- Fargate

### Easy real-life example

Normally:

```text
You
 ↓
Buy/manage server
 ↓
Install OS
 ↓
Install application
 ↓
Maintain server
```

Serverless:

```text
You
 ↓
Deploy code
 ↓
AWS manages servers
```

---

# 11. AWS Lambda

## What is AWS Lambda?

**AWS Lambda is a serverless Function as a Service (FaaS).**

You upload your function/code.

AWS runs it when needed.

You don't manage the server.

### EC2 vs Lambda

## EC2

```text
EC2
 ↓
Virtual Server
 ↓
You manage server
 ↓
Application runs
```

EC2 instances can continuously run.

Scaling may require adding or removing servers.

## Lambda

```text
Lambda
 ↓
Function
 ↓
Run when needed
 ↓
AWS manages servers
```

Scaling is automated.

### Easy difference

| EC2 | Lambda |
|---|---|
| Virtual server | Function |
| You manage the server | AWS manages infrastructure |
| Can run continuously | Runs when invoked |
| Scaling needs more management | Scaling is automatic |
| Pay for running compute capacity | Pay based on invocations and execution time |

---

# 12. Lambda is Event-Driven

Lambda functions are often **event-driven**.

This means an event happens and Lambda is invoked.

Example:

```text
New image uploaded to S3
          ↓
       Event
          ↓
    AWS Lambda
          ↓
Create thumbnail
```

You don't need to continuously run a server waiting for the image.

Lambda runs when the event happens.

---

# 13. Lambda Benefits

According to the slides, important benefits include:

### 1. Pay for what you use

Lambda pricing is based on:

- Number of requests/invocations
- Compute time

### 2. Serverless

You don't manage servers.

### 3. Event-driven

AWS services can invoke Lambda when events occur.

### 4. AWS integration

Lambda integrates with many AWS services.

### 5. Monitoring

Lambda can be monitored using **Amazon CloudWatch**.

### 6. Many programming languages

The slides list support for:

- Node.js / JavaScript
- Python
- Java
- C# / .NET
- PowerShell
- Ruby
- Custom Runtime API

Lambda also supports container images when they implement the Lambda Runtime API.

For arbitrary Docker workloads, the slides recommend ECS/Fargate instead.

---

# 14. Lambda Example — S3 Thumbnail Creation

This is one of the most important examples.

Suppose a user uploads:

```text
photo.jpg
```

to an S3 bucket.

### Flow

```text
User
 ↓
Upload image
 ↓
Amazon S3
 ↓
Trigger
 ↓
AWS Lambda
 ↓
Create Thumbnail
 ↓
Store thumbnail in S3
```

The slides also show storing metadata in DynamoDB.

For example:

```text
DynamoDB

Image name: photo.jpg
Image size: 2 MB
Creation date: ...
```

### Real-life use case

A social media website allows users to upload profile pictures.

Instead of manually creating smaller versions, Lambda automatically creates a thumbnail.

---

# 15. Lambda Example — Serverless CRON Job

A scheduled task can also invoke Lambda.

Example:

**Run a task every 1 hour.**

Flow:

```text
Amazon EventBridge
       ↓
Every 1 hour
       ↓
AWS Lambda
       ↓
Perform task
```

### Example task

Every hour, Lambda can:

```text
Check something
     ↓
Process data
     ↓
Send result
```

No server needs to be continuously running just for this task.

---

# 16. Lambda Pricing

The slides explain two main parts of Lambda pricing.

## 1. Number of requests

You pay based on the number of Lambda requests/invocations.

The course slides give an example where the first **1,000,000 requests are free**, followed by a per-request charge.

## 2. Duration

You also pay based on:

```text
Execution time × Memory allocated
```

The slides express this as compute time in **GB-seconds**.

### Easy example

Suppose:

```text
Lambda memory = 1 GB
Execution time = 2 seconds
```

Then compute usage is:

```text
1 GB × 2 seconds
= 2 GB-seconds
```

If the same function uses:

```text
2 GB
```

for:

```text
2 seconds
```

then:

```text
2 GB × 2 seconds
= 4 GB-seconds
```

### Important exam idea

**Lambda billing depends on how many times the function runs and how much compute time it uses.**

---

# 17. Lambda Maximum Execution Time

For the Cloud Practitioner material:

**A Lambda function can run for up to 15 minutes per invocation.**

This is important when comparing Lambda with AWS Batch.

### Example

If your job takes:

```text
2 minutes
```

Lambda can be suitable.

If your job needs:

```text
2 hours
```

Lambda is not suitable because of the invocation time limit.

For long-running batch processing, AWS Batch can be a better choice.

---

# 18. Amazon API Gateway

## What is API Gateway?

**Amazon API Gateway is a fully managed service used to create, publish, maintain, monitor and secure APIs.**

It can be used to build a **serverless API**.

It supports:

- REST APIs
- WebSocket APIs

It also provides features such as:

- Authentication
- Security
- API keys
- API throttling
- Monitoring

---

# 19. API Gateway + Lambda + DynamoDB Example

Suppose we create a student management application.

The client sends:

```text
GET /students
```

Flow:

```text
Client
  ↓
API Gateway
  ↓
Lambda
  ↓
DynamoDB
```

Lambda gets the student data from DynamoDB and returns the result.

### CRUD example

CRUD means:

- Create
- Read
- Update
- Delete

For example:

```text
Client
  ↓
API Gateway
  ↓
Lambda
  ↓
DynamoDB
```

The API can perform:

```text
POST   → Create student
GET    → Read student
PUT    → Update student
DELETE → Delete student
```

### Why API Gateway?

API Gateway acts as the HTTP/API entry point for clients.

---

# 20. AWS Batch

## What is AWS Batch?

**AWS Batch is a fully managed service for running batch processing jobs at scale.**

A **batch job** has:

```text
Start
 ↓
Process
 ↓
End
```

It is different from a continuously running application.

### Example

Suppose a company has 100,000 images that must be processed.

Instead of manually creating many servers:

```text
100,000 images
      ↓
AWS Batch
      ↓
Compute resources
      ↓
Process images
      ↓
Results
```

AWS Batch can dynamically launch:

- EC2 instances
- Spot Instances

It provisions the appropriate compute resources for the jobs.

---

# 21. How AWS Batch Works

The slides show that batch jobs are defined as **Docker images** and run using ECS.

Simplified flow:

```text
Job submitted
     ↓
AWS Batch
     ↓
EC2 / Spot Instances
     ↓
ECS
     ↓
Docker container
     ↓
Process job
     ↓
Result
```

### S3 example

Suppose raw files are stored in S3.

```text
S3
 ↓
AWS Batch
 ↓
Process files
 ↓
S3
```

The processed object can be inserted back into S3.

---

# 22. AWS Batch Real-Life Example

Suppose a company receives 1 million images every night.

It needs to:

- Resize images
- Compress images
- Analyze images
- Save processed images

This is a batch workload because the job:

```text
Starts
 ↓
Processes many files
 ↓
Finishes
```

AWS Batch can automatically provide the required compute capacity.

---

# 23. Batch vs Lambda

This is an important exam comparison.

| Lambda | AWS Batch |
|---|---|
| Serverless | Managed batch processing |
| Function-based | Job-based |
| Has execution time limit | No Lambda-style 15-minute limit |
| Limited runtimes | Any runtime packaged as Docker image |
| Limited temporary disk space | Can rely on EBS / instance store |
| AWS manages servers | AWS Batch can manage the underlying EC2 capacity |
| Good for short event-driven tasks | Good for large/long-running batch jobs |

### Easy example

**Lambda:**

```text
S3 upload
 ↓
Lambda
 ↓
Create thumbnail
```

Short task.

**Batch:**

```text
1 million files
 ↓
AWS Batch
 ↓
Process all files
 ↓
Finish
```

Large batch job.

### Memory trick

**Lambda = short function**

**Batch = large/long batch job**

---

# 24. Amazon Lightsail

## What is Lightsail?

Amazon Lightsail provides:

- Virtual servers
- Storage
- Databases
- Networking

It has **simple and predictable pricing**.

It is designed as a simpler alternative to services such as:

- EC2
- RDS
- ELB
- EBS
- Route 53

### Who should use Lightsail?

It is useful for people with **little cloud experience**.

---

# 25. Lightsail Use Cases

The slides list use cases such as:

### Simple web applications

Templates are available for:

- LAMP
- Nginx
- MEAN
- Node.js

### Websites

Templates include:

- WordPress
- Magento
- Plesk
- Joomla

### Development / Testing

Lightsail can be used to quickly create a simple development or test environment.

---

# 26. Lightsail Important Limitation

Lightsail has:

- High availability
- No auto-scaling
- Limited AWS integrations

So Lightsail is good for **simple applications**, but it is not intended to provide the same flexibility as using many individual AWS services.

### Easy example

A beginner wants to create a simple WordPress website.

Instead of configuring:

```text
EC2
EBS
RDS
Networking
Load Balancer
etc.
```

they can use:

```text
Amazon Lightsail
       ↓
Simple website
```

---

# 27. Important AWS Compute Services — One Table

| Service | Simple meaning | Main use |
|---|---|---|
| Docker | Container technology | Package applications |
| ECS | Run/manage Docker containers | Container orchestration on AWS |
| Fargate | Run containers without managing EC2 | Serverless containers |
| ECR | Store Docker images | Private container image repository |
| EKS | Managed Kubernetes | Kubernetes workloads |
| Lambda | Run functions without managing servers | Event-driven/serverless tasks |
| API Gateway | Create/expose/manage APIs | Serverless APIs |
| AWS Batch | Run batch jobs | Large/long-running jobs |
| Lightsail | Simple cloud application stack | Beginner/simple applications |

---

# 28. ECR vs ECS vs Fargate

This is very important.

Think about a Docker application:

```text
1. Store image
       ↓
     ECR

2. Run/manage container
       ↓
     ECS

3. If you don't want to manage EC2
       ↓
    Fargate
```

### Example

```text
Docker Image
     ↓
Amazon ECR
     ↓
ECS
     ↓
Fargate
     ↓
Running Container
```

**ECR stores the image.**

**ECS manages/runs containers.**

**Fargate provides serverless compute for containers.**

---

# 29. ECS vs Fargate

| ECS | Fargate |
|---|---|
| Container orchestration service | Serverless container compute |
| Can use EC2 | Does not require you to manage EC2 |
| With ECS + EC2, you manage infrastructure | AWS manages underlying infrastructure |
| Starts/stops containers | Runs containers for you |
| Can integrate with ALB | Can be used with ECS |

### Easy sentence

**ECS tells AWS how to manage containers.**

**Fargate lets you run those containers without managing servers.**

---

# 30. EKS vs ECS

```text
ECS
 ↓
AWS container orchestration

EKS
 ↓
Kubernetes container orchestration
```

Use **EKS** when your application/team is using Kubernetes.

Use **ECS** when you want AWS's native container orchestration service.

---

# 31. Serverless vs Server-Based

## Server-based example

```text
EC2
 ↓
You manage server
 ↓
Application
```

You may need to think about:

- Server
- OS
- Updates
- Scaling

## Serverless example

```text
Lambda
 ↓
Your function
 ↓
AWS manages infrastructure
```

You focus mainly on your code.

### Important

Serverless does **not** mean:

```text
No physical servers exist
```

It means:

```text
You don't manage/provision the servers.
```

---

# 32. Common Architecture Examples

## Example 1 — Serverless Image Processing

```text
User
 ↓
S3
 ↓
Event
 ↓
Lambda
 ↓
Create Thumbnail
 ↓
S3

Lambda
 ↓
DynamoDB
 ↓
Store image metadata
```

Use this when an event should automatically trigger a short function.

---

## Example 2 — Serverless API

```text
Client
 ↓
API Gateway
 ↓
Lambda
 ↓
DynamoDB
```

Use this to build a serverless HTTP API.

---

## Example 3 — Docker Application

```text
Developer
 ↓
Docker Image
 ↓
ECR
 ↓
ECS
 ↓
Container
```

If you use Fargate:

```text
Developer
 ↓
Docker Image
 ↓
ECR
 ↓
ECS + Fargate
 ↓
Container
```

No EC2 management is required with Fargate.

---

## Example 4 — Kubernetes

```text
Application
 ↓
Docker Container
 ↓
EKS
 ↓
Kubernetes Cluster
 ↓
EC2 / Fargate
```

Use EKS when Kubernetes is required.

---

## Example 5 — Large Batch Processing

```text
Input files
    ↓
   S3
    ↓
AWS Batch
    ↓
EC2 / Spot
    ↓
Docker / ECS
    ↓
Processed files
    ↓
   S3
```

Use Batch for large batch workloads.

---

## Example 6 — Beginner Website

```text
Beginner
   ↓
Lightsail
   ↓
Website
```

Use Lightsail when you want a simple and predictable cloud setup.

---

# 33. Exam-Focused Points

Remember these points for the Cloud Practitioner exam:

### Docker

**Docker = container technology used to package and run applications.**

### ECR

**ECR = private Docker image repository.**

### ECS

**ECS = run Docker containers on AWS.**

### Fargate

**Fargate = run containers without provisioning/managing EC2 infrastructure.**

### EKS

**EKS = managed Kubernetes service.**

### Serverless

**Serverless does not mean no servers. It means you don't manage the servers.**

### Lambda

**Lambda = serverless Function as a Service.**

### Lambda trigger

Lambda is commonly triggered by events.

Example:

```text
S3 upload → Lambda
```

### Lambda time

**Lambda invocation can run up to 15 minutes.**

### Lambda pricing

Main ideas:

```text
Number of invocations
+
Execution time × memory
```

### API Gateway

**API Gateway = create, publish, secure and monitor APIs.**

### AWS Batch

**Batch = run large/long-running batch jobs using managed compute.**

### Lightsail

**Lightsail = simple, predictable cloud service for beginners and simple applications.**

---

# 34. Super Short Revision

```text
Docker
→ Package application in containers

ECR
→ Store Docker images

ECS
→ Run/manage Docker containers

Fargate
→ Run containers without managing EC2

EKS
→ Managed Kubernetes

Serverless
→ AWS manages servers for you

Lambda
→ Run short functions on demand

API Gateway
→ Expose/manage APIs

Batch
→ Run large batch jobs

Lightsail
→ Simple cloud setup for beginners
```

---

# 35. One-Line Memory Trick

```text
ECR = Store Image
ECS = Manage Containers
Fargate = Run Containers without EC2 Management
EKS = Kubernetes
Lambda = Function
API Gateway = API
Batch = Batch Jobs
Lightsail = Simple Cloud
Docker = Containers
```

