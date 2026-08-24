# 05 - Elastic Load Balancing & Auto Scaling Groups


This section explains how AWS applications handle changing traffic and remain available.

Main topics:
- Scalability
- Vertical scaling
- Horizontal scaling
- High Availability
- Scalability vs Elasticity vs Agility
- Load Balancing
- Elastic Load Balancing (ELB)
- Application Load Balancer (ALB)
- Network Load Balancer (NLB)
- Gateway Load Balancer (GWLB)
- Auto Scaling Group (ASG)
- ASG minimum, desired and maximum capacity
- ASG with a Load Balancer
- Scaling strategies
- Predictive Scaling
- ELB + ASG summary

---

## 2. Scalability

**Scalability** means an application can handle a larger workload by adapting its resources.

There are two main approaches:

1. **Vertical Scalability (Scale Up / Scale Down)**
2. **Horizontal Scalability (Scale Out / Scale In)**

### Simple example

Suppose a college website is running on one EC2 instance.

- More students start using the website.
- The server becomes busy.
- We need more capacity.

We can either make the existing server bigger (**vertical**) or add more servers (**horizontal**).

---

## 3. Vertical Scalability

Vertical scalability means **increasing or decreasing the size of one EC2 instance**.

Example:

```text
Small EC2
t2.micro
   |
   | Scale Up
   v
Larger EC2
t2.large
```

If one server has too little CPU or RAM, we move to a bigger instance.

### Example

A database is running on a small EC2 instance:

```text
Before:
1 EC2 = t2.micro

After:
1 EC2 = t2.large
```

The number of instances stays the same. The instance becomes more powerful.

### Important points

- Also called **scale up / scale down**.
- Useful for systems that are not easily distributed, such as many database workloads.
- There is a hardware limit: one machine cannot become infinitely large.

---

## 4. Horizontal Scalability

Horizontal scalability means **increasing or decreasing the number of EC2 instances**.

Example:

```text
Before:
Users ---> EC2

After:
             +--> EC2
Users --> LB +--> EC2
             +--> EC2
```

Instead of making one server bigger, we add more servers.

### Example

A shopping website normally has:

```text
2 EC2 instances
```

During a sale:

```text
2 EC2 -> 5 EC2 -> 10 EC2
```

After the sale:

```text
10 EC2 -> 5 EC2 -> 2 EC2
```

This is horizontal scaling.

### Important points

- Also called **scale out / scale in**.
- Common for web applications and modern distributed applications.
- AWS EC2 makes it easier to add or remove instances.

---

## 5. High Availability (HA)

**High Availability** means designing an application so that it continues working even if part of the infrastructure fails.

The course explains HA as running the application in **at least two Availability Zones (AZs)**.

### Example

```text
                 Load Balancer
                 /           \
              AZ-A           AZ-B
               |              |
             EC2            EC2
```

If AZ-A has a problem, the application can continue using AZ-B.

### Goal

The goal is to survive a data-center or Availability Zone failure.

### HA vs Scalability

They are related, but they are not the same.

- **Scalability:** handle more workload.
- **High Availability:** keep the application available during failures.

---

## 6. High Availability + Scalability for EC2

AWS commonly combines:

- **Auto Scaling Group (ASG)** for changing the number of instances.
- **Load Balancer** for distributing traffic.
- **Multiple Availability Zones** for high availability.

Example:

```text
                  Users
                    |
                    v
              Load Balancer
               /          \
            AZ-A          AZ-B
             |              |
          EC2 + EC2      EC2 + EC2
             \              /
                ASG
```

This architecture can provide both scalability and high availability.

---

## 7. Scalability vs Elasticity vs Agility

### Scalability

The ability to handle a larger load by:

- making hardware stronger, or
- adding more instances.

### Elasticity

Elasticity means **automatically adjusting resources according to demand**.

Example:

```text
Low traffic  -> 2 EC2
High traffic -> 8 EC2
Low traffic  -> 2 EC2
```

This can reduce unnecessary cost because resources match demand.

### Agility

Agility means AWS resources can be made available quickly.

Example:

```text
Traditional:
Weeks to arrange infrastructure

Cloud:
Minutes to create resources
```

### Easy difference

| Concept | Simple meaning |
|---|---|
| Scalability | Can handle more load |
| Elasticity | Automatically adjusts to load |
| Agility | Get resources quickly |

---

# 8. What is Load Balancing?

A **Load Balancer** receives traffic from users and forwards it to multiple backend servers, such as EC2 instances.

Without a load balancer:

```text
Users ---> One EC2
```

If that EC2 becomes overloaded, the application may become slow or unavailable.

With a load balancer:

```text
                 +--> EC2 #1
Users --> LB ----+--> EC2 #2
                 +--> EC2 #3
```

The load balancer distributes requests across the backend instances.

---

## 9. Why Use a Load Balancer?

According to the course, a load balancer provides several benefits:

### 1. Spread the load

Traffic is distributed across multiple EC2 instances.

### 2. Single point of access

Users access the load balancer instead of directly accessing individual EC2 instances.

The load balancer provides a DNS endpoint for the application.

### 3. Handle failures

If a backend instance fails, the load balancer can avoid sending traffic to that unhealthy instance.

### 4. Health checks

The load balancer regularly checks whether backend instances are healthy.

### 5. SSL termination

For HTTPS applications, the load balancer can handle SSL/TLS termination.

### 6. High Availability

A load balancer can operate across Availability Zones.

---

# 10. Elastic Load Balancing (ELB)

**Elastic Load Balancing (ELB)** is AWS's managed load-balancing service.

AWS manages important infrastructure tasks such as:

- maintenance
- upgrades
- high availability

You configure the load balancer instead of managing the underlying load-balancing servers yourself.

### Why managed is useful

If you build your own load balancer, you must handle:

```text
Setup
Maintenance
Upgrades
Failure handling
Scaling
Integration
```

With ELB, AWS manages much of this infrastructure work.

---

# 11. Types of AWS Load Balancers

The supplied slides discuss four types:

1. Application Load Balancer (ALB)
2. Network Load Balancer (NLB)
3. Gateway Load Balancer (GWLB)
4. Classic Load Balancer (CLB) - old/retired

> For current AWS designs, ALB, NLB and GWLB are the important load balancer types. The course slide notes Classic Load Balancer as retired in 2023.

---

# 12. Application Load Balancer (ALB)

**ALB works at Layer 7** and is designed for application-level traffic.

The supplied course lists:

- HTTP
- HTTPS
- gRPC

### Main idea

ALB understands HTTP-level information and supports HTTP routing features.

Example:

```text
                  ALB
                   |
        +----------+----------+
        |                     |
   /students               /admin
        |                     |
 Student Servers        Admin Servers
```

A request can be routed based on application-level information.

### Example

Suppose:

```text
www.college.com/students
```

goes to student application servers.

And:

```text
www.college.com/admin
```

goes to admin application servers.

This type of HTTP-aware routing is a major use case for ALB.

---

# 13. Network Load Balancer (NLB)

**NLB works at Layer 4.**

The supplied slides mention:

- TCP
- UDP
- very high performance
- millions of requests per second
- static IP through Elastic IP

### Example

Suppose a high-performance application needs TCP traffic:

```text
Client
  |
  v
 NLB
 / \
EC2 EC2
```

NLB is suitable when very high network performance and Layer 4 traffic handling are required.

### Easy memory trick

```text
ALB -> Application -> HTTP/HTTPS/gRPC -> Layer 7
NLB -> Network     -> TCP/UDP          -> Layer 4
```

---

# 14. Gateway Load Balancer (GWLB)

GWLB is used for **third-party virtual network/security appliances**.

The supplied slides describe:

- GENEVE protocol
- IP packet traffic
- routing traffic to firewalls managed on EC2
- intrusion detection

### Example

```text
Users
  |
  v
GWLB
  |
  v
Security Appliance / Firewall
  |
  v
Application
```

A company can use security appliances such as firewalls or intrusion-detection systems.

### Easy idea

```text
ALB -> Web/application traffic
NLB -> Network traffic
GWLB -> Security/network appliances
```

---

# 15. Classic Load Balancer (CLB)

Classic Load Balancer is the older generation of AWS load balancing.

The supplied course identifies it as:

- Layer 4 and Layer 7
- retired in 2023

For learning the current architecture, focus mainly on:

```text
ALB
NLB
GWLB
```

---

# 16. What is an Auto Scaling Group (ASG)?

An **Auto Scaling Group (ASG)** automatically manages a group of EC2 instances according to application demand.

The main goals are:

- **Scale out** when demand increases.
- **Scale in** when demand decreases.
- Maintain minimum capacity.
- Maintain maximum capacity.
- Automatically register new instances with a load balancer.
- Replace unhealthy instances.
- Save cost by running an appropriate number of instances.

---

# 17. ASG Capacity

An ASG commonly uses three important capacity values:

### Minimum capacity

The minimum number of EC2 instances that should run.

Example:

```text
Minimum = 2
```

The ASG tries to keep at least 2 instances.

### Desired capacity

The number of instances the ASG wants to have normally.

Example:

```text
Desired = 3
```

Normally, the ASG tries to maintain 3 instances.

### Maximum capacity

The maximum number of instances the ASG can launch.

Example:

```text
Maximum = 10
```

The ASG will not scale beyond this configured maximum.

### Example

```text
Minimum = 2
Desired = 3
Maximum = 10

Normal:
3 EC2 instances

High traffic:
3 -> 5 -> 8 EC2

Very high traffic:
8 -> 10 EC2

Traffic decreases:
10 -> 8 -> 3 EC2
```

---

# 18. ASG Without Load Balancer

An ASG can manage EC2 instances even without an ELB.

```text
             Auto Scaling Group
          /       |       \
       EC2      EC2      EC2
```

The ASG controls the number of EC2 instances.

---

# 19. ASG With Load Balancer

ASG and ELB are commonly used together.

```text
                  Users
                    |
                    v
             Load Balancer
                    |
          +---------+---------+
          |         |         |
        EC2       EC2       EC2
          \         |         /
              Auto Scaling
                 Group
```

### How it works

1. Users send requests.
2. Load Balancer receives the requests.
3. Load Balancer sends traffic to healthy EC2 instances.
4. ASG monitors demand and capacity.
5. ASG launches new instances when required.
6. New instances are automatically registered with the load balancer.
7. When demand falls, ASG can terminate extra instances.

---

# 20. ASG Can Replace Unhealthy Instances

One important ASG feature is replacing unhealthy EC2 instances.

Example:

```text
Before:
EC2-1  Healthy
EC2-2  Healthy
EC2-3  Unhealthy
```

ASG can remove/replace the unhealthy instance:

```text
After:
EC2-1  Healthy
EC2-2  Healthy
EC2-4  New + Healthy
```

This helps maintain the desired capacity.

---

# 21. ASG Scaling Strategies

The supplied slides cover four important strategies:

1. Manual Scaling
2. Dynamic Scaling
3. Scheduled Scaling
4. Predictive Scaling

---

## 22. Manual Scaling

In manual scaling, you manually change the ASG size.

Example:

```text
Normal:
Desired capacity = 2

Exam/college admission day:
Desired capacity = 8
```

The administrator changes the ASG capacity manually.

### Disadvantage

It requires human action and may not react quickly to unexpected traffic.

---

# 23. Dynamic Scaling

Dynamic scaling responds to changing demand.

The course gives these examples:

### Simple / Step Scaling

A CloudWatch alarm can trigger an action.

Example:

```text
CPU > 70%
     |
CloudWatch Alarm
     |
Add 2 EC2 instances
```

When CPU becomes low:

```text
CPU < 30%
     |
CloudWatch Alarm
     |
Remove 1 EC2 instance
```

The exact thresholds are examples; the important idea is that a monitoring alarm can trigger scaling actions.

---

# 24. Target Tracking Scaling

Target tracking tries to keep a metric around a target value.

Example from the course:

```text
Target average CPU = 40%
```

If average CPU becomes too high, ASG can add instances.

If average CPU becomes too low, ASG can remove instances.

### Easy example

```text
Target CPU = 40%

CPU = 70% -> Add instances
CPU = 40% -> Keep current capacity
CPU = 20% -> Remove instances
```

The purpose is to maintain the selected target automatically.

---

# 25. Scheduled Scaling

Scheduled scaling is useful when you know the traffic pattern in advance.

Example:

A college website gets many users every Friday at 5 PM.

You can schedule scaling before that period:

```text
Before 5 PM:
Minimum capacity = 2

At 5 PM:
Minimum capacity = 10
```

This is useful for predictable time-based traffic.

---

# 26. Predictive Scaling

Predictive Scaling uses **Machine Learning** to predict future traffic.

It can provision EC2 instances in advance.

### Example

Suppose a shopping website normally receives high traffic every evening.

Historical data shows:

```text
6 PM -> traffic increases
7 PM -> very high traffic
8 PM -> traffic decreases
```

Predictive Scaling can use this pattern to prepare capacity ahead of time.

### Best use case

Predictable, time-based traffic patterns.

---

# 27. Complete Real-Life Example

Imagine an online college admission website.

### Normal traffic

```text
Students
   |
   v
Load Balancer
   |
   +--> EC2 #1
   +--> EC2 #2

ASG:
Minimum = 2
Desired = 2
Maximum = 10
```

### Admission form opens

Thousands of students start using the website.

```text
Traffic increases
      |
      v
CloudWatch metric/alarm
      |
      v
ASG scales out
      |
      v
2 EC2 -> 5 EC2 -> 8 EC2
```

The load balancer distributes traffic:

```text
                 Load Balancer
                /  /  |  \  \
               /  /   |   \  \
             EC1 EC2 EC3 EC4 EC5
```

### Traffic becomes normal

```text
8 EC2 -> 5 EC2 -> 2 EC2
```

This saves cost because unnecessary instances are removed.

### One EC2 fails

```text
EC2 #3 -> Unhealthy
          |
          v
        ASG
          |
          v
New EC2 instance launched
```

The new instance can be registered with the load balancer.

---

# 28. ELB + ASG + Multi-AZ Example

A strong architecture can look like this:

```text
                    Internet
                       |
                       v
                Load Balancer
                 /           \
                /             \
             AZ-A             AZ-B
              |                 |
        +-----+-----+       +---+-----+
        |           |       |         |
      EC2         EC2     EC2       EC2
        \           /       \         /
         \         /         \       /
          +-------+-----------+-----+
                    ASG
```

### What each component does

**Load Balancer**
- Receives user traffic.
- Distributes traffic.
- Performs health checks.

**ASG**
- Adds/removes EC2 instances.
- Maintains capacity.
- Replaces unhealthy instances.

**Multiple AZs**
- Improve availability.
- Help the application survive an AZ/data-center failure.

---

# 29. ELB vs ASG

| Feature | ELB | ASG |
|---|---|---|
| Main job | Distribute traffic | Manage EC2 capacity |
| Adds EC2? | No | Yes |
| Removes EC2? | No | Yes |
| Health checks | Yes | Can use health information for replacement |
| Handles traffic | Yes | No |
| Replaces unhealthy EC2 | No, it stops routing traffic to unhealthy targets | Yes |
| Works with Multi-AZ | Yes | Yes |
| Common together | Yes | Yes |

### Easy memory

```text
ELB = "Where should the request go?"

ASG = "How many servers should I run?"
```

---

# 30. Important Exam Points

### Scalability
Ability to handle a larger load.

### Vertical Scaling
Increase the size of one instance.

```text
1 small EC2 -> 1 large EC2
```

### Horizontal Scaling
Increase the number of instances.

```text
2 EC2 -> 5 EC2
```

### High Availability
Run application resources across at least two AZs to survive an AZ/data-center problem.

### Elasticity
Automatically scale resources according to demand.

### Load Balancer
Distributes traffic to backend instances.

### ELB
AWS-managed load balancing.

### ALB
Layer 7; HTTP/HTTPS/gRPC.

### NLB
Layer 4; TCP/UDP; very high performance.

### GWLB
Used for network/security appliances.

### ASG
Automatically adjusts EC2 capacity.

### Minimum
Smallest desired capacity.

### Desired
Normal target number of instances.

### Maximum
Largest allowed capacity.

### Predictive Scaling
Uses ML to predict future traffic.

---

# 31. Quick Revision

```text
SCALABILITY
|
+-- Vertical = Bigger machine
|
+-- Horizontal = More machines
|
+-- High Availability = Multiple AZs
|
+-- Elasticity = Automatically adjust capacity
|
+-- Agility = Get resources quickly

LOAD BALANCER
|
+-- ALB = Layer 7 = HTTP/HTTPS/gRPC
+-- NLB = Layer 4 = TCP/UDP
+-- GWLB = Security/network appliances
+-- CLB = Old/retired

AUTO SCALING GROUP
|
+-- Scale Out
+-- Scale In
+-- Minimum Capacity
+-- Desired Capacity
+-- Maximum Capacity
+-- Replace unhealthy instances
+-- Register new instances with ELB
|
+-- Strategies
    +-- Manual
    +-- Dynamic
    +-- Target Tracking
    +-- Scheduled
    +-- Predictive
```

---

## 32. One-Line Memory Trick

**ELB distributes traffic, ASG controls the number of EC2 instances, and Multi-AZ improves availability.**

---


