# 02 – AWS Identity and Access Management (IAM)

## 1. What Is IAM?

**IAM = Identity and Access Management.**

IAM is an AWS **global service** used to control who can access AWS resources and what they are allowed to do.

Think of IAM like the security system of an office:

- **Users** = employees
- **Groups** = departments
- **Policies** = permission rules
- **Roles** = temporary/access permissions given to AWS services
- **MFA** = extra security check

## 2. AWS Root User

When an AWS account is created, a **root account/user** is created by default.

Important rule:

> **Do not use or share the root account for normal AWS work.**

Use it only for tasks that specifically require root access, especially account setup/management tasks.

## 3. IAM Users

An IAM user represents a person within an organization.

Example:

```text
Company
├── Alice
├── Bob
├── Charles
└── David
```

Each person should normally have their own IAM user.

### Best practice

> One physical person = One AWS IAM user.

Do not create one shared IAM user for several people.

## 4. IAM Groups

A **group** is a collection of IAM users.

Example:

```text
Developers
├── Alice
├── Bob
└── Charles

Operations
├── David
└── Edward
```

Important points:

- A group contains **users only**.
- Groups cannot contain other groups.
- A user does not have to belong to a group.
- One user can belong to multiple groups.

### Why groups are useful

Instead of giving permissions individually to 50 developers, create a Developers group and attach the required policies to that group.

## 5. IAM Policies

A **policy** is a JSON document that defines permissions.

A policy answers:

> Who can do what, on which resource, and under which conditions?

Policies can be assigned to users or groups.

### Least Privilege Principle

AWS follows the principle of **least privilege**:

> Give a user only the permissions they actually need.

### Example

If a developer only needs to view EC2 information, do not give the developer full permission to delete EC2 instances.

## 6. IAM Policy Example

A simplified policy looks like this:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "ec2:Describe*",
      "Resource": "*"
    }
  ]
}
```

### Meaning

- `Version` = policy language version.
- `Statement` = permission statement.
- `Effect: Allow` = permission is allowed.
- `Action` = operation the user can perform.
- `Resource` = resource on which the action applies.
- `*` = wildcard.

This example allows EC2 `Describe` actions on all resources represented by `*`.

## 7. IAM Policy Structure

Important elements:

### Version

Defines the policy language version.

The slides use:

```text
2012-10-17
```

### Id

Optional identifier for the policy.

### Statement

Required. Contains one or more permission statements.

Inside a Statement:

### Sid
Optional identifier for a statement.

### Effect

Controls whether access is:

- `Allow`
- `Deny`

### Principal

The account, user, or role to which a resource-based policy applies.

### Action

The AWS operation allowed or denied.

Examples:

```text
ec2:DescribeInstances
s3:GetObject
```

### Resource

The AWS resource to which the action applies.

### Condition

Optional rules that specify when the policy should apply.

## 8. Policy Inheritance

Permissions can come from policies attached to:

- A user
- A group

Example:

```text
Alice
  ↓
Developers Group
  ↓
EC2 Read Policy
```

Alice receives the permissions provided by the group policy.

A user can also have permissions directly attached to the user.

## 9. IAM Password Policy

AWS allows you to configure password requirements for IAM users.

Possible requirements include:

- Minimum password length
- Uppercase letters
- Lowercase letters
- Numbers
- Non-alphanumeric characters
- Allow users to change their own passwords
- Password expiration
- Prevent password reuse

### Example

A company might require:

```text
Minimum length: 12
Uppercase: Yes
Lowercase: Yes
Number: Yes
Special character: Yes
Password expiration: Enabled
Password reuse: Prevented
```

The exact settings depend on the organization's security requirements.

## 10. Multi-Factor Authentication (MFA)

**MFA = Multi-Factor Authentication.**

MFA adds an additional security factor.

Basic idea:

```text
Something you know
        +
Something you own
        =
Stronger authentication
```

For AWS, this commonly means:

```text
Password + MFA device/code
```

### Why MFA is important

If someone steals your password, they may still not be able to access the account without the second factor.

### Where should MFA be used?

The slides emphasize protecting:

- Root account
- IAM users

## 11. MFA Device Options

The slides mention several MFA options:

### Virtual MFA

Examples:

- Google Authenticator
- Authy

These use a phone to generate MFA codes.

### Security key

Example:

- YubiKey

A physical security key can be used as an additional authentication factor.

### Hardware MFA devices

The slides also mention hardware key-fob devices.

## 12. How Users Access AWS

There are three main ways shown in the slides:

### 1. AWS Management Console

A web interface.

Authentication can use:

```text
Username/password + MFA
```

Useful when you want to work through a browser.

### 2. AWS CLI

**CLI = Command Line Interface**

You interact with AWS using commands in a terminal.

Example:

```bash
aws s3 ls
```

This can list S3 buckets accessible to the authenticated identity.

### 3. AWS SDK

**SDK = Software Development Kit**

It allows an application to interact with AWS programmatically using a programming language.

Examples of supported languages include:

- Python
- JavaScript
- Java
- C++
- Go
- Ruby
- PHP
- .NET
- Node.js

## 13. Access Keys

Access keys are used for programmatic access such as:

- AWS CLI
- AWS SDK

They contain:

- **Access Key ID**
- **Secret Access Key**

Easy analogy:

```text
Access Key ID    ≈ username
Secret Access Key ≈ password
```

### Security rule

> Treat access keys like passwords. Never share them.

Do not put real access keys in GitHub repositories or public code.

## 14. AWS CLI

The AWS CLI is a command-line tool for interacting with AWS services.

It provides direct access to AWS service APIs through commands.

Example:

```bash
aws sts get-caller-identity
```

You can also automate AWS operations with scripts.

### Console vs CLI

| Method | How you use it |
|---|---|
| Console | Browser/GUI |
| CLI | Terminal commands |
| SDK | Application code |

## 15. AWS SDK

AWS SDKs are language-specific libraries/APIs that allow applications to access and manage AWS services programmatically.

Example idea:

```text
Your Application
       ↓
AWS SDK
       ↓
AWS Service
```

The slides mention SDK support for languages/platforms such as:

- JavaScript
- Python
- PHP
- .NET
- Ruby
- Java
- Go
- C++
- Mobile SDKs
- IoT Device SDKs

## 16. IAM Roles for AWS Services

Sometimes an AWS service needs permission to access another AWS service on your behalf.

Instead of storing a user's access key inside the service, use an **IAM Role**.

Common examples from the slides:

- EC2 Instance Role
- Lambda Function Role
- CloudFormation Role

### Example: EC2 + S3

Suppose an EC2 instance needs to read files from S3.

A good design is:

```text
EC2
 ↓
IAM Role
 ↓
Policy
 ↓
S3 permission
```

The EC2 instance uses the role's permissions.

### Why roles are useful

The service can receive the required permissions without you putting a personal access key directly inside the application/server.

## 17. IAM Security Tools

### IAM Credentials Report

This is an **account-level** report.

It lists IAM users and information about the status of their credentials.

It can help an administrator audit user credentials.

### IAM Access Advisor

This is a **user-level** tool.

It shows:

- Permissions granted to a user
- When services were last accessed

You can use this information to review and reduce unnecessary permissions.

## 18. IAM Best Practices

### 1. Do not use the root account for normal work

Use it only when required.

### 2. One physical user = one IAM user

Do not share IAM users.

### 3. Use groups

Put users into groups and assign permissions to groups where appropriate.

### 4. Use a strong password policy

Require suitable password complexity and lifecycle controls.

### 5. Use MFA

Protect root and IAM users with MFA.

### 6. Use IAM roles for AWS services

For example, give an EC2 instance a role when it needs AWS permissions.

### 7. Use access keys for programmatic access

Use them for CLI/SDK when required.

### 8. Audit permissions

Use:

- IAM Credentials Report
- IAM Access Advisor

### 9. Never share IAM users or access keys

Credentials are sensitive.

## 19. IAM Shared Responsibility Model

The IAM shared responsibility model divides responsibilities between AWS and the customer.

### AWS responsibilities

The slides include:

- Infrastructure/global network security
- Configuration and vulnerability analysis
- Compliance validation

### Customer responsibilities

The customer manages:

- Users
- Groups
- Roles
- Policies
- MFA
- Access keys
- Permission reviews
- Access-pattern analysis

### Simple example

AWS secures the underlying cloud infrastructure.

You must configure IAM correctly:

```text
AWS
 ↓
Secure cloud infrastructure

Customer
 ↓
Users + Groups + Policies + Roles + MFA
```

## 20. Complete IAM Example

Imagine a company has three teams:

```text
Company
│
├── Developers
│   ├── Alice
│   └── Bob
│
├── Operations
│   ├── Charles
│   └── David
│
└── Audit
    └── Emma
```

### Step 1: Create users

Create one IAM user for each employee.

### Step 2: Create groups

Create:

- Developers
- Operations
- Audit

### Step 3: Attach policies

For example:

```text
Developers → required development permissions
Operations → required operational permissions
Audit → required read/audit permissions
```

### Step 4: Enable MFA

Protect IAM users.

### Step 5: Use roles for AWS services

If an EC2 server needs S3 access:

```text
EC2 → IAM Role → S3 permissions
```

### Step 6: Review permissions

Use IAM Access Advisor and the Credentials Report.

This is much safer than giving everyone Administrator-level access.

## 21. Console vs CLI vs SDK – Easy Example

Suppose you want to work with an S3 bucket.

### Console

You open the AWS website and click through the S3 interface.

### CLI

You use a command such as:

```bash
aws s3 ls
```

### SDK

Your Python/JavaScript application calls AWS using the appropriate SDK.

So:

> **Console = human + browser**  
> **CLI = human/script + terminal**  
> **SDK = application code + AWS**

## 22. IAM Important Terms

| Term | Meaning |
|---|---|
| IAM | Identity and Access Management |
| Root user | Original account identity |
| User | Person/identity in an organization |
| Group | Collection of users |
| Policy | JSON permission document |
| Role | Permissions that can be assumed/used by AWS identities/services |
| MFA | Extra authentication factor |
| Access Key ID | Programmatic access identifier |
| Secret Access Key | Secret credential for programmatic access |
| CLI | Command-line access to AWS |
| SDK | Programmatic access through code |
| Credentials Report | Account-level credential audit report |
| Access Advisor | User-level service access review |

## 23. Exam-Focused Differences

### User vs Group

- User = individual identity
- Group = collection of users

### Group vs Role

- Group organizes users and can have policies attached.
- Role is used to provide permissions to an AWS identity/service that assumes or uses the role.

### Password vs Access Keys

- Password → commonly used for Console sign-in.
- Access keys → used for CLI/SDK programmatic access.

### MFA vs Password

- Password is one authentication factor.
- MFA adds another factor.

### CLI vs SDK

- CLI → commands from terminal.
- SDK → libraries used inside application code.

### Credentials Report vs Access Advisor

- Credentials Report → account-level credential status.
- Access Advisor → user-level permissions and service access history.

## 24. Quick Revision

### IAM
Global AWS service for identity and access management.

### Users
Represent individual people.

### Groups
Contain users only.

### Policies
JSON documents defining permissions.

### Least privilege
Give only the permissions required.

### MFA
Password + additional security factor.

### Access keys
Used for CLI/SDK programmatic access.

### Roles
Used to give AWS services permissions.

### CLI
Manage AWS through terminal commands.

### SDK
Manage AWS through application code.

### Security tools
- IAM Credentials Report
- IAM Access Advisor

### Best practices
- Avoid root user for normal work.
- One physical person = one IAM user.
- Use groups.
- Use strong passwords.
- Enable MFA.
- Use roles for AWS services.
- Protect/rotate credentials appropriately.
- Review permissions regularly.
- Never share access keys.

---

# Final Exam Memory Trick

```text
IAM
│
├── Users      → People
├── Groups     → Collection of users
├── Policies   → JSON permissions
├── Roles      → Permissions for AWS services/identities
├── MFA        → Extra security
├── Password   → Console authentication
├── Access Keys→ CLI / SDK
├── CLI        → Terminal
├── SDK        → Code
└── Audit
    ├── Credentials Report
    └── Access Advisor
```

> **Most important idea:** IAM controls **who can access AWS and what they can do**.
