# 09 – Deploying and Managing Infrastructure at Scale

## Introduction

When an application becomes large, manually creating and managing AWS resources becomes difficult.

Important services in this section:

- CloudFormation
- AWS CDK
- Elastic Beanstalk
- CodeDeploy
- CodeCommit
- CodeBuild
- CodePipeline
- CodeArtifact
- Systems Manager
- Session Manager
- Parameter Store

---

# 1. AWS CloudFormation

## What is CloudFormation?

AWS CloudFormation is an Infrastructure as Code (IaC) service.

Instead of manually creating AWS resources from the AWS Console, we describe the infrastructure in a template. CloudFormation then creates the required resources.

Example:

```text
CloudFormation Template
        ↓
CloudFormation Stack
        ↓
EC2 + S3 + Security Group + Load Balancer
```

The template can describe:

- Security Group
- EC2 instances
- S3 bucket
- Load Balancer
- Other AWS resources

CloudFormation creates resources in the required order with the configuration specified in the template.

---

# 2. Declarative Infrastructure

CloudFormation is declarative.

Declarative means:

> We describe WHAT infrastructure we want rather than manually describing every step of HOW to create it.

Example:

```text
I want:
- 1 Security Group
- 2 EC2 instances
- 1 S3 bucket
- 1 Load Balancer
```

CloudFormation handles the creation and orchestration.

---

# 3. CloudFormation Template

CloudFormation templates can be written using YAML or JSON.

Simple example:

```yaml
Resources:

  MyBucket:
    Type: AWS::S3::Bucket

  MySecurityGroup:
    Type: AWS::EC2::SecurityGroup

  MyInstance:
    Type: AWS::EC2::Instance
```

The `Resources` section describes the AWS resources that should be created.

---

# 4. CloudFormation Stack

A CloudFormation Stack is the collection of AWS resources created and managed together.

```text
CloudFormation Template
        ↓
       Stack
        ↓
 ┌──────┼─────────┐
 EC2    S3    Security Group
```

### Easy difference

**Template = Blueprint**

```text
"Create 2 EC2 + 1 S3 + Load Balancer"
```

**Stack = Actual deployed infrastructure**

```text
EC2-1
EC2-2
S3
Load Balancer
```

---

# 5. CloudFormation Example – Student Leave Management System

Suppose our application needs:

```text
Student Leave Management
        |
        ├── EC2
        ├── Security Group
        ├── Load Balancer
        └── Database
```

Without CloudFormation:

```text
Step 1 → Create Security Group
Step 2 → Create EC2
Step 3 → Create another EC2
Step 4 → Create Load Balancer
Step 5 → Configure Target Group
Step 6 → Create database
Step 7 → Connect everything
```

With CloudFormation:

```text
CloudFormation Template
        ↓
CloudFormation Stack
        ↓
AWS creates the resources
```

---

# 6. Benefits of CloudFormation

## 6.1 Infrastructure as Code

Infrastructure is stored as code.

Instead of manually clicking through the AWS Console:

```text
cloudformation.yaml
```

can describe the infrastructure.

Benefits:

- Easy to review
- Easy to version-control
- Repeatable
- Less manual work

---

## 6.2 Cost Management

Resources in a CloudFormation stack can be identified together.

This makes it easier to understand the cost associated with a stack.

Example:

```text
Student-App-Stack
 ├── EC2
 ├── Load Balancer
 ├── Database
 └── Storage
```

The course also gives a development-environment saving strategy: automatically delete development resources at the end of the day and recreate them in the morning.

---

## 6.3 Productivity

CloudFormation allows infrastructure to be destroyed and recreated.

Example:

```text
Template
   ↓
Stack
   ↓
Infrastructure
```

Delete the stack:

```text
Stack
 ↓
Resources deleted
```

Deploy the template again:

```text
Template
   ↓
New Stack
   ↓
New Infrastructure
```

This is useful for development and testing.

---

## 6.4 Automatic Diagram Generation

CloudFormation can generate a diagram showing resources and their relationships.

This helps understand complex infrastructure.

---

## 6.5 Reuse Existing Templates

Existing CloudFormation templates can be reused rather than building everything from scratch.

---

## 6.6 Support for AWS Resources

CloudFormation supports almost all AWS resources.

For resources that are not directly supported, CloudFormation can use custom resources.

---

# 7. CloudFormation Infrastructure Composer

Infrastructure Composer provides a visual way to work with CloudFormation infrastructure.

Example:

```text
       Load Balancer
             |
       -------------
       |           |
     EC2-1       EC2-2
       |           |
       -------------
             |
            RDS
```

The PDF uses a WordPress CloudFormation stack as an example.

The diagram helps us see:

- Which resources exist
- How resources are related

---

# 8. AWS CDK

CDK means:

> AWS Cloud Development Kit

CDK allows us to define cloud infrastructure using familiar programming languages.

Languages mentioned in the course:

- JavaScript / TypeScript
- Python
- Java
- .NET

Flow:

```text
Programming Language
        ↓
       CDK
        ↓
CloudFormation Template
        ↓
CloudFormation
        ↓
AWS Resources
```

Important:

> CDK generates a CloudFormation template. CloudFormation is then used to deploy the infrastructure.

---

# 9. Why Use CDK?

Instead of writing a large YAML/JSON template, developers can define infrastructure using programming languages they already know.

Example concept:

```text
Create an S3 bucket
```

CDK converts this infrastructure definition into CloudFormation.

---

# 10. CDK with Lambda

CDK is useful when infrastructure and application runtime code need to be deployed together.

Example:

```text
CDK Application
      |
      ├── Lambda
      ├── IAM Role
      ├── API Gateway
      └── Database
```

---

# 11. CDK with Docker

CDK can also be used with Docker containers.

Examples:

```text
CDK
 ↓
ECS
 ↓
Docker Container
```

and:

```text
CDK
 ↓
EKS
 ↓
Docker Containers
```

The course specifically highlights CDK for Lambda and Docker containers in ECS/EKS.

---

# 12. CloudFormation vs CDK

| CloudFormation | CDK |
|---|---|
| Infrastructure defined directly in YAML/JSON | Infrastructure defined using programming languages |
| Direct CloudFormation template | Generates CloudFormation |
| Declarative template | Programming-language based |
| YAML/JSON | TypeScript, Python, Java, .NET etc. |

Easy memory:

```text
CDK
 ↓
CloudFormation
 ↓
AWS Resources
```

---

# 13. Developer Problems on AWS

Developers often face several infrastructure problems:

- Managing infrastructure
- Deploying code
- Configuring databases
- Configuring load balancers
- Scaling applications
- Maintaining consistent environments

Many web applications use similar architecture, such as:

```text
ALB + ASG + EC2
```

Developers generally want to focus on their application code rather than manually managing every infrastructure component.

---

# 14. AWS Elastic Beanstalk

Elastic Beanstalk provides a developer-centric way to deploy applications on AWS.

It uses AWS components such as:

- EC2
- Auto Scaling
- Elastic Load Balancing
- RDS

Elastic Beanstalk is:

> Platform as a Service (PaaS)

It provides a simpler application-focused view while still allowing control over configuration.

The Beanstalk service itself is free, but the underlying AWS resources are charged.

---

# 15. Elastic Beanstalk Example

Suppose we have a Node.js application.

Without Beanstalk:

```text
Create EC2
    ↓
Install Node.js
    ↓
Configure application
    ↓
Create Load Balancer
    ↓
Configure Auto Scaling
    ↓
Deploy application
    ↓
Monitor
```

With Beanstalk:

```text
Application Code
       ↓
Elastic Beanstalk
       ↓
EC2 + ASG + ELB
       ↓
Running Application
```

---

# 16. What Elastic Beanstalk Manages

Beanstalk manages several infrastructure-related tasks:

- Instance configuration
- Operating-system-related configuration
- Deployment
- Capacity provisioning
- Load balancing
- Auto Scaling
- Application health monitoring
- Responsiveness monitoring

The developer mainly remains responsible for the application code.

---

# 17. Elastic Beanstalk Architecture Models

Beanstalk provides three architecture models.

## 17.1 Single Instance

```text
User
 ↓
EC2
 ↓
Application
```

Good for:

- Development
- Testing

---

## 17.2 Load Balancer + Auto Scaling Group

```text
             Users
               |
              ELB
          /     |     \
        EC2    EC2    EC2
          \     |     /
              ASG
```

Good for:

- Production web applications
- Pre-production web applications
- Applications requiring scaling

---

## 17.3 Auto Scaling Group Only

```text
              ASG
          /    |    \
        EC2   EC2   EC2
```

Good for non-web production applications such as:

- Workers
- Background processing

---

# 18. Elastic Beanstalk Supported Platforms

The course lists support for:

- Go
- Java SE
- Java with Tomcat
- .NET on Windows Server with IIS
- Node.js
- PHP
- Python
- Ruby
- Packer Builder
- Docker

Docker options include:

- Single Container Docker
- Multi-Container Docker
- Preconfigured Docker

---

# 19. Elastic Beanstalk Health Monitoring

Beanstalk has a health agent.

Flow:

```text
Application
     ↓
Health Agent
     ↓
CloudWatch
```

The health agent:

- Checks application health
- Publishes health events
- Sends metrics to CloudWatch

Example:

```text
Application
     ↓
Health Agent
     ↓
CloudWatch
```

---

# 20. AWS CodeDeploy

CodeDeploy is used to automatically deploy applications.

It works with:

- EC2 instances
- On-premises servers

Therefore CodeDeploy is a:

> Hybrid service

Important:

> Servers must already be provisioned and configured before CodeDeploy can deploy to them.

---

# 21. CodeDeploy Agent

Servers must have the CodeDeploy Agent installed/configured.

Flow:

```text
CodeDeploy
     ↓
CodeDeploy Agent
     ↓
EC2 / On-Premises Server
     ↓
Application
```

---

# 22. CodeDeploy Example

Suppose three EC2 instances currently run version 1:

```text
EC2-1 → v1
EC2-2 → v1
EC2-3 → v1
```

We want to deploy version 2.

```text
v1
 ↓
CodeDeploy
 ↓
v2
```

Result:

```text
EC2-1 → v2
EC2-2 → v2
EC2-3 → v2
```

CodeDeploy automates the application deployment/upgrade process.

---

# 23. AWS CodeCommit

CodeCommit is a source-control service that hosts Git-based repositories.

It can be thought of as an AWS service for storing private Git repositories.

Example:

```text
CodeCommit Repository

StudentLeaveManagement
├── server.js
├── package.json
├── public/
├── views/
└── README.md
```

---

# 24. CodeCommit Benefits

The course lists these benefits:

- Fully managed
- Scalable
- Highly available
- Private
- Secure
- Integrated with AWS
- Git-based
- Automatic versioning of code changes

---

# 25. AWS CodeBuild

CodeBuild is a cloud service for building source code.

It can:

- Compile source code
- Run tests
- Produce packages/artifacts ready for deployment

Flow:

```text
CodeCommit
     ↓
Retrieve Code
     ↓
CodeBuild
     ↓
Build + Test
     ↓
Ready-to-deploy Artifact
```

---

# 26. CodeBuild Example

Suppose a Node.js application is stored in CodeCommit.

```text
CodeCommit
     ↓
CodeBuild
     ↓
Install dependencies
     ↓
Build
     ↓
Run tests
     ↓
Create deployable artifact
```

CodeBuild is:

- Fully managed
- Serverless
- Continuously scalable
- Highly available
- Secure

The course notes pay-as-you-go pricing based on build time.

---

# 27. AWS CodePipeline

CodePipeline orchestrates the different stages required to move code to production.

The course flow is:

```text
Code
 ↓
Build
 ↓
Test
 ↓
Provision
 ↓
Deploy
```

CodePipeline is a basis for:

> Continuous Integration and Continuous Delivery (CI/CD)

---

# 28. Complete CodePipeline Example

For a Student Leave Management application:

```text
Developer
    ↓
CodeCommit
    ↓
CodePipeline
    ↓
CodeBuild
    ↓
Build + Test
    ↓
CodeDeploy
    ↓
EC2
    ↓
Application
```

CodePipeline coordinates the different stages.

---

# 29. CodePipeline Integrations

CodePipeline can work with:

- CodeCommit
- CodeBuild
- CodeDeploy
- Elastic Beanstalk
- CloudFormation
- GitHub
- Third-party services
- Custom plugins

Therefore:

> CodePipeline acts as the orchestration layer.

---

# 30. AWS CodeArtifact

Software applications depend on packages and libraries.

For example, a Node.js application may depend on:

```text
express
mysql2
dotenv
jsonwebtoken
```

These are software dependencies.

Managing and storing dependencies is called:

> Artifact management

CodeArtifact provides secure, scalable and cost-effective artifact management.

---

# 31. CodeArtifact Example

```text
Developer
    ↓
CodeArtifact
    ↓
Software Dependencies
    ↓
Application
```

CodeBuild can retrieve dependencies from CodeArtifact.

---

# 32. CodeArtifact Supported Tools

The course mentions:

- Maven
- Gradle
- npm
- yarn
- twine
- pip
- NuGet

Example:

```text
Node.js
   ↓
npm
   ↓
CodeArtifact
   ↓
Required Packages
```

---

# 33. AWS Systems Manager

AWS Systems Manager, commonly called SSM, helps manage EC2 and on-premises systems at scale.

It is another:

> Hybrid AWS service

It provides operational insights and a suite of management features.

---

# 34. Why Systems Manager?

Suppose a company has many servers:

```text
100 EC2 Instances
+
50 On-Premises Servers
```

Manually managing each server is difficult.

Systems Manager can help perform operations across the fleet.

---

# 35. Important Systems Manager Features

The course highlights:

### 1. Patching Automation

Automate server patching.

### 2. Run Commands

Run commands across an entire fleet.

Example:

```text
100 EC2 Instances
       ↓
Systems Manager
       ↓
Run Command
       ↓
All selected servers
```

### 3. Parameter Store

Store configuration and parameters.

---

# 36. Supported Operating Systems

Systems Manager works with:

- Linux
- Windows
- macOS
- Raspberry Pi OS (Raspbian)

---

# 37. How Systems Manager Works

Systems Manager uses the:

> SSM Agent

Architecture:

```text
              Systems Manager
                    |
        -------------------------
        |           |           |
      EC2         EC2       On-Prem VM
        |           |           |
    SSM Agent   SSM Agent   SSM Agent
```

The SSM Agent allows Systems Manager to communicate with the managed systems.

The course notes that the SSM Agent is installed by default on Amazon Linux AMIs and some Ubuntu AMIs.

---

# 38. Systems Manager Session Manager

Session Manager provides a secure shell/session on:

- EC2 instances
- On-premises servers

without requiring traditional SSH access.

It does not require:

- SSH keys
- Bastion hosts
- Port 22 for the Session Manager connection

It supports:

- Linux
- macOS
- Windows

Session logs can be sent to:

- Amazon S3
- CloudWatch Logs

---

# 39. SSH vs Session Manager

### Traditional SSH

```text
User
 ↓
SSH
 ↓
Port 22
 ↓
EC2
```

### Session Manager

```text
User
 ↓
IAM Permissions
 ↓
Session Manager
 ↓
SSM Agent
 ↓
EC2
```

Session Manager therefore avoids the need for a traditional SSH connection.

---

# 40. Parameter Store

Systems Manager Parameter Store is used for secure storage of:

- API keys
- Passwords
- Configuration
- Other parameters/secrets

Example:

```text
DATABASE_HOST = mydb
DATABASE_PORT = 3306
API_KEY = XXXXX
```

Instead of putting sensitive values directly into application code, an application can retrieve them from Parameter Store.

---

# 41. Parameter Store + IAM

IAM controls access to Parameter Store.

Example:

```text
Application
     ↓
IAM Role
     ↓
Permission
     ↓
Parameter Store
```

Only authorized users/applications should be allowed to access the parameters.

---

# 42. Parameter Store + KMS

Parameter Store supports encryption using AWS KMS.

Conceptual flow:

```text
Application
     ↓
Parameter Store
     ↓
Encrypted Parameter
     ↓
AWS KMS
```

Parameter Store also supports version tracking.

Example:

```text
DB_PASSWORD

Version 1 → Old value
Version 2 → New value
Version 3 → Updated value
```

---

# 43. Complete Deployment Example

Suppose we have a Student Leave Management System.

## Infrastructure

```text
CDK
 ↓
CloudFormation
 ↓
AWS Infrastructure
 ↓
EC2 + ALB + RDS + Security Groups
```

## Application Delivery

```text
Developer
   ↓
CodeCommit
   ↓
CodePipeline
   ↓
CodeBuild
   ↓
CodeDeploy
   ↓
EC2
   ↓
Application
```

## Configuration

```text
Application
     ↓
Parameter Store
     ↓
Configuration / Secrets
```

## Server Management

```text
Systems Manager
     ↓
EC2 / On-Premises Servers
```

---

# 44. CloudFormation vs Elastic Beanstalk

### CloudFormation

Focus:

> Infrastructure

Example:

```text
Create:
- EC2
- S3
- RDS
- VPC
- ALB
- IAM resources
```

### Elastic Beanstalk

Focus:

> Application deployment/platform

Example:

```text
Node.js Application
       ↓
Elastic Beanstalk
       ↓
EC2 + ALB + ASG
       ↓
Running Application
```

Easy memory:

```text
CloudFormation
= "Build my AWS infrastructure"

Elastic Beanstalk
= "Deploy/run my application"
```

---

# 45. CloudFormation vs CDK

CloudFormation:

```text
YAML / JSON
     ↓
CloudFormation
     ↓
AWS
```

CDK:

```text
Python / TypeScript / Java / .NET
              ↓
             CDK
              ↓
      CloudFormation Template
              ↓
             AWS
```

---

# 46. CodeCommit vs CodeBuild vs CodeDeploy

Remember:

```text
CodeCommit
    ↓
STORE CODE

CodeBuild
    ↓
BUILD + TEST CODE

CodeDeploy
    ↓
DEPLOY CODE
```

Complete flow:

```text
Developer
   ↓
CodeCommit
   ↓
CodeBuild
   ↓
CodeDeploy
   ↓
EC2 / Servers
```

---

# 47. CodePipeline

CodePipeline connects and orchestrates the stages:

```text
Code
 ↓
Build
 ↓
Test
 ↓
Provision
 ↓
Deploy
```

Therefore:

> CodePipeline = CI/CD orchestration layer

---

# 48. CodeArtifact vs CodeCommit

### CodeCommit

Stores:

```text
Source Code
```

Example:

```text
server.js
index.html
style.css
```

### CodeArtifact

Stores/manages:

```text
Software Packages / Dependencies
```

Example:

```text
npm packages
Maven packages
Python packages
NuGet packages
```

---

# 49. CodeDeploy vs Systems Manager

### CodeDeploy

Main purpose:

```text
Application Deployment
```

Example:

```text
Deploy application version 2
```

### Systems Manager

Main purpose:

```text
Server Management
```

Examples:

```text
Run commands
Patch servers
Configure servers
Start Session Manager sessions
Store parameters
```

---

# 50. Service Decision Guide

### Need Infrastructure as Code?

```text
CloudFormation
```

### Want to write infrastructure using Python/TypeScript/etc.?

```text
CDK
```

### Want a managed application platform?

```text
Elastic Beanstalk
```

### Want a Git repository?

```text
CodeCommit
```

### Want to build and test code?

```text
CodeBuild
```

### Want to deploy application code?

```text
CodeDeploy
```

### Want to orchestrate CI/CD?

```text
CodePipeline
```

### Want to manage software packages/dependencies?

```text
CodeArtifact
```

### Want to manage servers at scale?

```text
Systems Manager
```

### Want a secure shell without traditional SSH?

```text
Session Manager
```

### Want to store configuration/secrets?

```text
Parameter Store
```

---

# 51. Final Revision Table

| Service | Main Purpose |
|---|---|
| CloudFormation | Infrastructure as Code |
| CDK | Define infrastructure using programming languages |
| Elastic Beanstalk | PaaS application deployment |
| CodeCommit | Store source code in Git repositories |
| CodeBuild | Build and test code |
| CodeDeploy | Deploy applications to servers |
| CodePipeline | Orchestrate CI/CD |
| CodeArtifact | Store/manage software packages and dependencies |
| Systems Manager | Manage EC2 and on-premises systems at scale |
| Session Manager | Secure server session without traditional SSH |
| Parameter Store | Store configuration and secrets |

---

# 52. One-Line Memory Trick

```text
CloudFormation → Create Infrastructure

CDK → Write Infrastructure Using Programming Language

Elastic Beanstalk → Deploy Application Easily

CodeCommit → Store Code

CodeBuild → Build + Test

CodeDeploy → Deploy

CodePipeline → Connect/Orchestrate CI/CD

CodeArtifact → Store Dependencies

Systems Manager → Manage Servers

Session Manager → Secure Server Session

Parameter Store → Store Configuration and Secrets
```

---

# 53. Important Exam Points

1. CloudFormation = Infrastructure as Code.
2. CloudFormation templates describe AWS infrastructure.
3. CloudFormation manages resources through stacks.
4. CloudFormation is declarative.
5. CDK lets developers define infrastructure using programming languages.
6. CDK generates CloudFormation templates.
7. Elastic Beanstalk is a Platform as a Service (PaaS).
8. Elastic Beanstalk uses services such as EC2, ASG, ELB and RDS.
9. The Beanstalk service itself is free, but underlying AWS resources are charged.
10. CodeCommit = Git-based source repository.
11. CodeBuild = build and test source code.
12. CodeDeploy = deploy applications.
13. CodePipeline = orchestrate CI/CD.
14. CodeArtifact = manage software packages/dependencies.
15. Systems Manager manages EC2 and on-premises systems at scale.
16. SSM Agent enables Systems Manager management.
17. Session Manager provides secure sessions without traditional SSH access.
18. Parameter Store stores configuration and secrets.
19. Parameter Store can use IAM for access control and KMS for encryption.
20. CodeDeploy and Systems Manager can work with AWS and on-premises servers.
