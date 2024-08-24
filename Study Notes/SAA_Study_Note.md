# Aws Solutions Architect Associate Note

### Practice Course Link

https://www.udemy.com/course/best-aws-certified-solutions-architect-associate/learn/lecture/29388690#content



### Table of Contents



| No.  | Contents          |      |      |      |      |
| ---- | ----------------- | ---- | ---- | ---- | ---- |
|      | **IAM & AWS CLI** |      |      |      |      |
| 1    | [IAM]()           |      |      |      |      |
| 2    | [AWS CLI]()       |      |      |      |      |
|      | **EC2**           |      |      |      |      |
| 1    | [EC2]()           |      |      |      |      |
| 2    | [AWS CLI]()       |      |      |      |      |
|      |                   |      |      |      |      |
|      |                   |      |      |      |      |
|      |                   |      |      |      |      |

-------

# IAM

= Identity and Access Management

= **Global** service: you can use this console(IAM) in every region.

- **Root**: Default account, shouldn't be used or shared
- **Users**: within your organization, can be grouped
- **Groups**: contain users, not other groups
- Users don't have to be in a group and can be in a multiple groups

## IAM: Permissions

- Users or Groups can be assigned JSON doc called **Policies**
  - define the permissions of the users
- apply the least privilege principle
  - Do not give more permissions than a user actually needs

## IAM: Policies Structure

```
/* An example of using multiple Statements are add in one Policy */
{
  "Version": "2012-10-17", // policy language version, mostly it's “2012–10–17”.
  "Id": "S3 Account-Permissions", // an identifier for the policy
  "Statement": [  //one or more
    {
      "Sid": "FirstStatement", // identifier for the statement
      "Effect": "Allow", // whether the statement allows or denies access (Allow, Deny)
      "Principal": { //account/user/role to which this policy applied to
      	"AWS":["arn:aws:iam::1234:root"]
      }
      "Action": [ //list of actions this policy allows or denies
      	"s3:List*",
        "s3:Get*"
      ], 
      "Resource": [ // resources to which the actions applied to
        "arn:aws:s3:::confidential-data",
        "arn:aws:s3:::confidential-data/*"
      ], 
      "Condition": {
      	"Bool": {"aws:MultiFactorAuthPresent": "true"}
      } // for when this policy is in effect.
    }
  ]
}
```

- IAM > Policies : you can edit Policy in JSON format.

## IAM: MFA

- **MFA** (Multi Factor Authentication): password you know + security device you own
  - Password + MFA  => success
- very recommended to use in AWS
-  Users have access to your account and can possibly change configurations or delete resources in your AWS account.
- Protect Root Accounts and IAM users.
- **Benefit** : if a password is stolen or hacked, the account is not compromised.
  - since security device you own should be needed to login
- **Options**: 
  - virtual MFA device: support for multiple tokens(accounts') on a single device.
  - Universal 2nd Factor(U2F) Security Key: Support for multiple root and IAM users, physical device (Proviced by YubiKey by Yubico)
  - Hardware Key Fob MFA device: Proviced by Gemalto
  - Hardware Key Fob MFA device for AWS GovCloud(US): Provided by SurePassID

## IAM: Accesss Options

- 3 options to access AWS
  - AWS Management Comsole (protected by password + MFA)
  - AWS Command Line Interface (CLI): protected by access keys
  - AWS Software Developter Kit (SDK)-for code: protected by access keys
- CLI
  - Access Keys are generated through the AWS Console.
  - Users manage their own access keys
  - Access Keys are secret, just like a password, do not share them

## IAM: CLI

- using commands in your command-line shell
- direct access to the public APIs of AWS services
- you can develop scripts to manage your resources
- alternative to using AWS Management Console.

## IAM: SDK

- Software Development Kit
- Language-specific APIs (set of libraries)
- enables you to access and manage AWS services programmatically
- Embedded within your application
- Supports: SDKS(JavaScript, Python, PHP...), Mobile SDKs(Android, iOS,..), IoT Device SDKs (Embedded C, ...)

## IAM: Role

- not assign to users!!! -> assign permissions to AWS services with IAM roles
- Common roles:
  - EC2 Instance Roles
  - Lambda function roles
  - Roles for CloudFormation

## IAM: Security Tools

- **IAM Credentials Report** (account-level): a report that lists all your account's users and the status of their various credentials.
- **IAM Access Advisor** (User-level): Access advisor shows the service permissions granted to a user and when those services were last accessed

## IAM: Guidelines

- one physical user = one AWS user
- Assign users to groups and assign permissions to groups
- create a strong password policy
- use and enforce the use of Multi Factor Authentication
- create and use roles for giving permissions to AWS services
- use Access Keys for Programmatic Access
- Audit permissions of your account using IAM credentials Report & IAM access Advisor



-----

# EC2

= Elastic Compute Cloud = infrastructure as a Service

= Identity and Access Management

= **Global** service: you can use this console(IAM) in every region.

- Capability
  - Renting virtual machines (EC2)
  - Storing data on virtual drives (EBS)
  - distributing load across machines (ELB)
  - Scaling the services using an auto-scaling group (ASG)

## EC2 sizing & Configuration Options

- operating system: Linux, windows or mac OS
- how much compute power & cores (CPU)
- how much random-access memory (RAM)
- how much storage space:
  - Network-attached (EBS & EFS)
  - Hardware (EC2 Instance Store)
- Network Card: Speed of the card, Public IP address
- Firewall rules: security group
- Bootstrap Script: EC2 User Data

## EC2 User Data

- it is possible to bootstrap our instances using an EC2 User data script
- **bootstrapping**(자력) means launching commands when a machine starts
- that script is **only run once** at the instance first start
- EC2 user data is used to automate boot tasks such as:
  - installing updates
  - installing software
  - downloading common files from the internet
  - anything you can think of
- User Data script runs with the root user

## EC2 Instance Types - Overview

Example: m5.2xlarge

- m: instance class
- 5: generation (AWS improves them over time)
- 2xlarge: size within the instance class

## EC2 Instance Types - General Purpose

- Greate for a diversity of workloads such as web servers or code repositiories
- Balance between:
  - compute
  - memory
  - networking

## EC2 Instance Types - **Compute** Optimized

- great for compute-intensive tasks that require **high performance processors**:
  - batch processing workloads
  - media transcoding
  - High performance web servers
  - high performance computing
  - scientific modeling & machine learning
  - dedicated gaming servers

## EC2 Instance Types - Memory Optimized

- fast performance for workloads that process l**arge data sets in memory**:
- Use cases:
  - high performance, relational/non-relational databases
  - distributed web scale cache stores
  - In-memory databases optimized for BI (Business Intelligence)
  - applications performing real-time processing of big unstructured data

## EC2 Instance Types - Storage Optimized

- Greate for storage-intensive tasks that require high, sequential read and write access to large data sets on local storage
- use cases:
  - high frequency online transaction processing(OLTP) systems
  - relational & NoSQL databases
  - Cache for in-memory databases (eg. Redid)
  - data warehousing applications
  - distributed filed systems

 ## Security Groups?

- the fundamental of network security in AWS
- They control how traffic is allowed into or out of our EC2 instances.
- only contail **allow** rules
- security groups rules can reference by IP or by security group

## More about Security Groups 

- Security groups are acting as a "firewall" on EC2 instances.
- They regulate:
  - access to **Ports**
  - Authorised **IP ranges** - IPv4 and IPv6
  - Control of **inbound network** (from other to the instance-when others are trying to access the instance)
  - control of **outbound network** (from the instance to other-when the instance trying to access others)

## Security Groups good to know

- Can be attached to multiple instances <-> instance can attach multiple security groups
- limited to a specific region / VPC(virtual private cloud) combination
  - if needed, create new one to the region
-  seperate with EC2 -> if SG's traffic is blocked the EC2 instance won't see it
- it's good to maintain one separate security group for SSH access
  - since SSH access is difficult to manage.
- if your application is not accessible(**"time out"**). then it's mostly a security group issue.
- if you receive a **"Connection refused"** error, then SG worked and traffic  went throw but  it's an application error or it's not launched.
- all inbound traffic is **blocked** by default
- all outbound traffic is **authorised** by default

## Basic Ports

- 22 = ssh(Secure Shell) - log into a **Linux** instance
- 21 = FTP (File Transfer Protocol) - upload files into a file share
- 22 = SFTP (Secure File Transfer Protocol) - upload files using SSH
- 80 = HTTP - access unsecured websites
- 443 = HTTPS - access secured websites
- 3389 = RDP (Remote Desktop Protocol) - log into a **Windows** instance

## EC2 Instances Purchasing Options

- On-Demand(주문형) Instances - short workload, predictable pricing, pay by second
  - Allows to run by the needs(demands)
-  Reserved (1 & 3 years)
  - reserved instances - long workloads => for using db for log time
  - convertible reserved instances - log workloads with flexible instances => 
- Savings Plans  (1 & 3 years) - commitment to an amount of usage, long workload
- spot instances - short workloads, cheap, can lose instances (less reliable)
- dedicated(전용) hosts - book an entire physical server, control instance placement
- dedicated(전용) instances - no other customers will share your hardware
- capacity reservations(용량 예약) - reserve capacity in a specific AZ for any **duration**.

## EC2 on Demand

- pay for what you use:
  - Linux or Windows - billing per second, after the first minute
  - All other operating systems - billing per hour
- has the highest cost but no upfront payment(먼저 지불할 금액)
- No long-term commitment
- => Recommend for **short-term** and u**n-interrupted workloads**(중단 없는 워크로드), where you can't predict how the application will behave

## EC2 Reserved instances

- up to 72% discount compared to On-demand
- reserve a specific instance attributes (**Instance Type, Region, Tenancy, OS**)
- Reservation Period - 1 year(+discount) or 3 years(+++discount)
- Payment Options - No Upfront(+), Partial upfront(++), All Upfront(+++)
- reserved instance's Scope - regional or zonal (reserve capacity in an AZ)
- recommended for steady-state usage applications (eg. Database)
- you can **buy and sell** in the reserved instance Marketplace
- **Convertible Reserved Instance**(전환형 예약 인스턴스)
  - can change the EC2 instance type, instance type, instance family, OS, scope and tenancy
  - because more flexibility => up to 66% discount

## EC2 Savings Plans

- Get a discount based on long-term usage (up to 72% - same as RIs)
- Commit to a certain type of usage ($10/hour for 1 to 3 years)
- Usage beyond EC2 Savings Plancs is billed at the On-Demand price
- locked to a specific instance family & AWS region (e.g., M5 in us-east-I)
- flexible across:
  - instance size
  - OS
  - Tenancy(host, dedicated, default)

## EC2 Spot Instances

- Can get a discount of up to 90% compared to On-demand
- Instances that you can "**lose**" at any point of time if **your max price is less than the current spot price**
- The **MOST cost-efficient** instances in AWS
- Useful for workloads that are resilient to failure(고장에 대한 회복력)
  - batch jobs
  - data analysis
  - image processing
  - any distributed workloads(분산형 작업)
  - workloads with a flexible start and end time
- not suitable for critical job or databases

## EC2 Dedicated Hosts

- a physical server with EC2 instance capacity fully dedicated to your use
- allows you address compliance requirements(법규 준수 요건) and use your existing server-bound software license (per-socket, per-core, per-VM software licenses)
- Purchasing Options:
  - On-demand - pay per second for active dedicated host(전용 호스트)
  - Reserved - 1 or 3 years 
- the most expensive option
- Useful for software that have complicated licensing model (BYOL - bring your own license)
- Or for companies that have strong regulatory or compliance needs

## EC2 Dedicated Instances

- instances run on hardware that's dedicated to you (!= physical server)
- may share hardware with other instances in same account
- No control over instance placement (can move hardware after Stop/Start)

***Dedicated Hosts = you get an access to physical servers itself and it gives you visibility into a lower level hardware

***Dedicated Instances = you have your on instances in your own hardware

## EC2 Capacity Reservations

- Reserve On-demand instances capacity in a specific AZ for any duration
- always have access to EC2 apacity when you need it
- no time commitment (create/cancel anytime), no billing discounts
- combine with **resional reserved instances** and **savings plans** to benefit from billing discounts
- you're charged at On-demand rate whether you run instances or not
- Suitable for short-term, uninterrupted workloads that needs to be in a specific AZ

## which purchaing options is suitable?

- **On demand**: coming and staying in resort whenever we like, we pay the full price 

- **Reserved**: like planning ahead and if we plan to stay for a long time, we may get a good discount. 
- **Savings Plans**: pay a certain amount per hour for certain period and stay in any room type (e.g., King, Suite, Sea View, …) 
- **Spot instances**: the hotel allows people to bid for the empty rooms and the highest bidder keeps the rooms. You can get kicked out at any time 
- **Dedicated Hosts**: We book an entire building of the resort 
- **Capacity Reservations**: you book a room for a period with full price even you don’t stay in it

## EC2 Spot Instance Requests

- Can get a discount of up to 90% compared to On-demand 
- Define max spot price and get the instance while current spot price < max 
  - The hourly spot price varies based on offer and capacity 
  - If the current spot price > your max price you can choose to stop or terminate your instance with a 2 minutes grace period. 
- Other strategy: Spot Block 
  - “block” spot instance during a specified time frame (1 to 6 hours) without interruptions 
  - In rare situations, the instance may be reclaimed 
- Used for batch jobs, data analysis, or workloads that are resilient to failures. 
- Not great for critical jobs or databases
