# 01 – What is Cloud Computing



## 1. How Websites Work

When you open a website, two main sides are involved:

- **Client** – your laptop, phone, or browser that requests the website.
- **Server** – a computer that receives the request and sends back the website/data.
- Both clients and servers have **IP addresses**.
- A **network** connects these systems.

### Simple example

You open `www.shop.com`.

1. Your browser acts as the client.
2. It sends a request through the network.
3. The server receives the request.
4. The server sends the website response back to your browser.

The slides compare this to sending post mail: the sender and receiver need addresses so the message can reach the correct destination.

## 2. What Is a Server Made Of?

A server needs several important components:

| Component | Simple meaning |
|---|---|
| CPU | Performs calculations and runs instructions |
| RAM | Temporary working memory |
| Storage | Stores files and data |
| Database | Stores structured data |
| Network | Connects the server to other systems |

### Networking terms

- **Network:** Connected computers, servers, cables, routers, and other networking devices.
- **Router:** Forwards packets between networks and helps decide where packets should go.
- **Switch:** Sends packets to the correct server/client inside a network.

## 3. Traditional IT Infrastructure

Before cloud computing, an organization normally had to build and maintain its own infrastructure.

For example, a company might need:

- A data center
- Physical servers
- Storage
- Networking equipment
- Power supply
- Cooling
- Maintenance
- Staff to monitor the infrastructure

### Problems with traditional IT

1. Data-center rent is expensive.
2. Power, cooling, and maintenance cost money.
3. Buying or replacing hardware takes time.
4. Scaling is limited.
5. A team may be needed to monitor infrastructure 24/7.
6. Disasters such as fire, power failure, or earthquakes can damage infrastructure.

Cloud computing helps move much of this infrastructure work to a cloud provider.

## 4. What Is Cloud Computing?

**Cloud computing is the on-demand delivery of compute power, database storage, applications, and other IT resources through a cloud platform, using pay-as-you-go pricing.**

In simple words:

> Instead of buying and maintaining all physical servers yourself, you rent the IT resources you need from a cloud provider.

With cloud computing:

- You can choose the type and size of resources you need.
- Resources can be provisioned quickly.
- You can access servers, storage, databases, and application services.
- AWS owns and maintains the network-connected hardware used for these services.
- You pay according to the services/resources you use.

### Real-life example

Imagine opening a small online shop.

**Traditional way:**
You buy servers, storage, networking equipment, provide electricity/cooling, and maintain everything.

**Cloud way:**
You use AWS services and create the required resources when you need them. If traffic increases, you can increase resources.

## 5. Cloud Services You Already Use

The slides give familiar examples:

### Gmail
Gmail is a cloud email service. You use email without managing the physical email servers yourself.

### Dropbox
Dropbox is a cloud storage service.

### Netflix
Netflix provides video-on-demand and is an example of a large application using cloud infrastructure.

## 6. Cloud Deployment Models

There are three important deployment models:

### 6.1 Private Cloud

A cloud environment used by a **single organization** and not exposed to the public.

Benefits:

- More control
- Useful for sensitive applications
- Can meet specific business requirements

**Example:** A company keeps sensitive internal systems in a private cloud.

### 6.2 Public Cloud

Cloud resources are owned and operated by a **third-party cloud provider** and delivered over the Internet.

**Example:** Using AWS to create an EC2 server.

### 6.3 Hybrid Cloud

A combination of on-premises/private infrastructure and public cloud.

**Example:**

- Sensitive data stays in the company's own infrastructure.
- The company uses AWS for additional application capacity.

### Easy comparison

| Model | Main idea |
|---|---|
| Private | One organization |
| Public | Third-party provider over the Internet |
| Hybrid | Private/on-premises + public cloud |

## 7. Five Characteristics of Cloud Computing

### 1. On-demand self-service

Users can provision resources themselves without waiting for human interaction from the provider.

**Example:** Create an EC2 instance from the AWS Console when needed.

### 2. Broad network access

Cloud resources are available over a network and can be accessed from different client platforms.

**Example:** Access an application from a laptop or phone.

### 3. Multi-tenancy and resource pooling

Multiple customers can use shared physical infrastructure while maintaining security and privacy.

**Example:** Different AWS customers use resources hosted on AWS infrastructure.

### 4. Rapid elasticity and scalability

Resources can be quickly added or removed according to demand.

**Example:** A shopping website needs more computing resources during a sale and fewer resources after the sale.

### 5. Measured service

Cloud usage is measured, so customers can be charged according to their usage.

**Example:** You pay for the compute time or storage you use.

## 8. Six Advantages of Cloud Computing

### 1. Trade CAPEX for OPEX

- **CAPEX (Capital Expense):** Buying physical infrastructure.
- **OPEX (Operational Expense):** Paying for resources as operating costs.

Cloud computing lets organizations avoid large upfront hardware purchases.

### 2. Pay on demand

You do not need to own all the hardware. You use resources when required.

### 3. Reduced TCO and OPEX

Cloud can reduce the total cost of ownership and operational expenses because you do not have to maintain your own data center infrastructure.

### 4. Massive economies of scale

Large cloud providers operate at huge scale. Their efficiency can help reduce prices.

### 5. Stop guessing capacity

You do not have to buy hardware for a future maximum load. You can scale according to actual usage.

### 6. Increase speed and agility

You can create infrastructure quickly, test applications, and launch products faster.

Cloud infrastructure also helps organizations reach users globally using AWS global infrastructure.

## 9. Problems Solved by Cloud

### Flexibility
Change resource types when requirements change.

### Cost-effectiveness
Pay for what you use.

### Scalability
Handle larger workloads by using stronger hardware or adding more resources.

### Elasticity
Scale out and scale in when needed.

- **Scale out:** Add more resources/nodes.
- **Scale in:** Remove resources.

### High availability and fault tolerance
Applications can be designed across multiple data centers so that one failure does not necessarily stop the whole application.

### Agility
Develop, test, and launch applications quickly.

## 10. Types of Cloud Computing

There are three major service models:

### 10.1 IaaS – Infrastructure as a Service

IaaS provides basic building blocks for cloud IT, such as:

- Networking
- Computing
- Storage

It gives a high level of flexibility and is similar to traditional IT infrastructure.

**AWS example:** Amazon EC2.

### 10.2 PaaS – Platform as a Service

PaaS removes the need for the organization to manage the underlying infrastructure.

You mainly focus on deploying and managing your application.

**AWS example:** Elastic Beanstalk.

### 10.3 SaaS – Software as a Service

SaaS is a complete product that is run and managed by the service provider.

**Examples:** Gmail, Dropbox, Zoom. The slides also give Amazon Rekognition as an AWS service example.

### Easy way to remember

> **IaaS = manage more infrastructure**  
> **PaaS = focus on your application**  
> **SaaS = use a finished product**

### Responsibility idea

Moving from On-Premises → IaaS → PaaS → SaaS means more of the underlying stack is managed by the provider and less is managed by you.

## 11. Cloud Computing Examples

### IaaS
- Amazon EC2
- Google Cloud
- Microsoft Azure
- DigitalOcean
- Linode

### PaaS
- AWS Elastic Beanstalk
- Heroku
- Google App Engine

### SaaS
- Gmail
- Dropbox
- Zoom
- Amazon Rekognition is listed in the slides as an AWS service example.

## 12. AWS Cloud Pricing – Basic Idea

The slides describe AWS pricing as **pay-as-you-go**.

Three basic areas are:

### Compute
Pay for compute time.

### Storage
Pay for the data stored in the cloud.

### Data transfer
The slides state that data transfer **IN** is free, while data transfer **OUT** can be charged.

## 13. AWS Cloud History

The slide timeline highlights:

- **2002:** AWS was internally launched.
- **2003:** Amazon identified its infrastructure as a core strength and considered marketing it.
- **2004:** Public launch with SQS.
- **2006:** Public re-launch with SQS, S3, and EC2.
- **2007:** AWS launched in Europe.

## 14. AWS Cloud Use Cases

AWS can be used to build sophisticated and scalable applications across many industries.

Examples from the slides:

- Enterprise IT
- Backup and storage
- Big data analytics
- Website hosting
- Mobile and social applications
- Gaming

## 15. AWS Global Infrastructure

Important terms:

- **AWS Regions**
- **Availability Zones (AZs)**
- **Data Centers**
- **Edge Locations / Points of Presence**

These are important for understanding where AWS resources and services operate.

## 16. AWS Regions

An **AWS Region** is a geographical area containing a cluster of data centers.

Examples of Region names include:

- `us-east-1`
- `eu-west-3`

Most AWS services are **Region-scoped**, meaning the resource is created/used in a selected Region.

### How to choose a Region?

Consider:

1. **Compliance and legal requirements** – data governance requirements matter.
2. **Customer proximity** – choosing a nearby Region can reduce latency.
3. **Available services** – not every AWS service or feature is available in every Region.
4. **Pricing** – prices can vary by Region.

### Example

If most customers are in India, you may prefer a nearby AWS Region to reduce network latency, subject to service availability, compliance, and pricing.

## 17. Availability Zones

An **Availability Zone (AZ)** is one or more discrete data centers within an AWS Region.

The slides describe AZs as having:

- Redundant power
- Redundant networking
- Redundant connectivity
- Isolation from other AZs
- High-bandwidth, ultra-low-latency connections between AZs

Example Region:

```text
AWS Region
├── Availability Zone A
├── Availability Zone B
└── Availability Zone C
```

### Why multiple AZs?

Suppose your application runs in one AZ and that AZ has a problem. If you designed the application across multiple AZs, another AZ can continue serving the application.

## 18. Points of Presence / Edge Locations

AWS has Points of Presence, including Edge Locations and Regional Caches.

Their purpose is to help deliver content closer to end users, which can reduce latency.

This is especially important for content delivery services such as Amazon CloudFront.

## 19. Global Services vs Region-Scoped Services

Some AWS services are considered **global services**, while many others are Region-scoped.

### Global services shown in the slides

- IAM
- Route 53 – DNS service
- CloudFront – Content Delivery Network
- WAF – Web Application Firewall

### Region-scoped services shown in the slides

- EC2
- Elastic Beanstalk
- Lambda
- Rekognition

### Easy exam idea

> If a service is Region-scoped, check/select the Region where you want to create the resource.

## 20. AWS Shared Responsibility Model

The basic idea is:

> **AWS = Security OF the Cloud**  
> **Customer = Security IN the Cloud**

AWS is responsible for the security of the underlying cloud infrastructure.

The customer is responsible for security/configuration inside the cloud according to the service being used.

### Simple example

AWS protects the underlying physical cloud infrastructure.

You are responsible for properly configuring your AWS resources and securing what you put in the cloud.

## 21. AWS Acceptable Use Policy

The slides highlight that AWS must not be used for:

- Illegal, harmful, or offensive use/content
- Security violations
- Network abuse
- Email or other message abuse

## 22. Quick Revision

### Cloud computing
On-demand IT resources with pay-as-you-go pricing.

### Deployment models
- Private
- Public
- Hybrid

### Five characteristics
- On-demand self-service
- Broad network access
- Multi-tenancy/resource pooling
- Rapid elasticity/scalability
- Measured service

### Six advantages
- CAPEX → OPEX
- Pay on demand
- Reduced TCO/OPEX
- Economies of scale
- No capacity guessing
- Speed and agility

### Service models
- IaaS
- PaaS
- SaaS

### AWS infrastructure
- Region
- Availability Zone
- Data Center
- Edge Location / Point of Presence

### Security
- AWS: security **of** the cloud
- Customer: security **in** the cloud

## 23. Easy Real-Life Example

Imagine you run an online clothing store.

### Without cloud

You buy:

- Servers
- Storage
- Network devices
- Power/cooling infrastructure

You maintain everything yourself.

### With AWS

You can use cloud resources when needed.

During a festival sale:

```text
Normal traffic
      ↓
Small amount of resources

Festival sale
      ↓
High traffic
      ↓
More resources

Sale finished
      ↓
Reduce resources
```

This demonstrates **scalability, elasticity, pay-as-you-go, and agility**.

---

