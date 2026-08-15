# IAM - Identity and Access Management

IAM stands for:

Identity and Access Management

IAM controls:

- Who can access AWS
- What they can access
- What actions they can perform

## Simple Example

Imagine a college.

There are:

- Students
- Teachers
- Principal

Students should not have access to everything.

Teachers may have different permissions.

The principal may have more permissions.

AWS IAM works in a similar way.

---

# IAM Users

A user represents a person who needs access to AWS.

Example:

Alice
Bob
John

Each person can have their own AWS user.

---

# IAM Groups

A group contains users.

Example:

Developers Group:
- Alice
- Bob
- John

Instead of giving permissions to Alice, Bob and John separately,
we can give permissions to the Developers group.

Then users in that group receive those permissions.

Important:

A group can contain users,
but a group cannot contain another group.



-----------------------------*******************------------------------------------------------


# MFA

MFA = Multi-Factor Authentication

MFA adds an extra security step.

Without MFA:

Password
   ↓
Login

With MFA:

Password
   +
Security code/device
   ↓
Login

## Simple Example

Suppose someone steals your AWS password.

If MFA is enabled, they still need the MFA device/code.

So your account has an extra layer of protection.

## Remember

MFA =

Something you know
+
Something you have


-----------------------------*******************------------------------------------------------

# AWS Console vs CLI vs SDK

## 1. AWS Management Console

You use a web browser.

Example:

Open AWS Console
→ Click EC2
→ Launch Instance

Authentication:
Password + MFA

---

## 2. AWS CLI

CLI means Command Line Interface.

You use commands in a terminal.

Example:

aws s3 ls

This command can list S3 buckets.

Authentication:
Access Keys

---

## 3. AWS SDK

SDK means Software Development Kit.

You use programming languages to work with AWS.

Example:

Python application
        ↓
AWS SDK
        ↓
AWS Service

Supported languages include:

- Python
- Java
- JavaScript
- C++
- Go
- PHP
- Ruby


-----------------------------*******************------------------------------------------------


# Access Keys

Access keys are used for programmatic access to AWS.

They are mainly used with:

- AWS CLI
- AWS SDK

There are two parts:

1. Access Key ID
2. Secret Access Key

Easy way to remember:

Access Key ID ≈ Username

Secret Access Key ≈ Password

## IMPORTANT

Never share your Secret Access Key.

Do not put real access keys in GitHub.

Do not upload them into your code.

Do not share them with anyone.

-----------------------------*******************------------------------------------------------

# IAM Roles

An IAM Role gives permissions to an AWS service.

A service can assume the role and perform allowed actions.

## Simple Example

EC2
 ↓
IAM Role
 ↓
S3

Suppose an EC2 instance needs to read files from S3.

Instead of putting access keys inside the EC2 server,
we can attach an IAM Role to the EC2 instance.

The role gives EC2 the required permissions.

## Common Examples

- EC2 Instance Role
- Lambda Function Role
- CloudFormation Role
