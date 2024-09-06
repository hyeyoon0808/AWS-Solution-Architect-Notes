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
- **Benefit** : if a password is stolen or hacked, the account is not compromised.(침해 당하지 않는다)
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

## EC2 Dedicated Instances(전용 인스턴스)

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

  - The hourly spot price varies based on offer and capacity(제공과 용량)
  - If the current spot price > your max price you can choose to **stop**(멈춤) or **terminate**(종료) your instance with a 2 minutes grace period. 

- Other strategy: **Spot Block** 

  - “block” spot instance during a specified time frame (available between 1 to 6 hours) without interruptions 
  - In rare situations, the instance may be reclaimed(회수) => but again, rarely!

- Used for batch jobs, data analysis, or workloads that are resilient to failures. 

- Not great for critical jobs or databases

- <u>How to terminate Spot Instances?</u>

  1. Cancel Spot requests (when spot requests are active, disabled or closed)
     - cancelling a spot request does not terminate instances

  2. Terminate the associated spot instances

## Spot Fleets

- Spot Fleets(무리) = set of Spot Instances + (optional) On-Demand Instances 
- The Spot Fleet will try to meet the target capacity(목표 용량) with price constraints(가격 제한) 
  - Define possible launch pools: instance type (m5.large), OS, Availability Zone 
  - Can have multiple launch pools, so that the fleet can choose 
  - Spot Fleet stops launching instances when reaching capacity or max cost 
- Strategies to allocate Spot Instances: 
  - **lowestPrice**: from the pool with the lowest price (cost optimization, short workload)
  - **diversified**(다양한): distributed across all pools - when one pool is gone, the other pool will be active (great for availability, long workloads)
  - **capacityOptimized**(용량 최적화): pool with the optimal capacity for the number of instances
  - **priceCapacityOptimized** (recommended): pools with highest capacity available, then select the pool with the lowest price (best choice for most workloads) 
- <u>Spot Fleets allow us to automatically request Spot Instances with the lowest price</u>

---

## Private VS Public IP (IPv4)

- Networking has two sorts of IPs. IPv4 and IPv6: 
- IPv4 is still the most common format used online. 
- IPv6 is newer and solves problems for the Internet of Things (IoT). 
- IPv4: [0-255].[0-255].[0-255].[0-255] -> 3.7 billion different addresses

## Private vs Public IP (IPv4) Fundamental Differences

- Public IP: 

  - The machine can be identified on the **internet** (WWW) 

  - Must be **unique** across the whole web (not two machines can have the same public IP). 
  - Can be geo-located easily 

- Private IP: 

  - The machine can only be identified(accessed) on a **private network only** 
  - The IP must be unique across the private network 
  - BUT two different private networks (two companies) can have the same private IPs. 
  - Machines connect to WWW using a NAT + internet gateway (a proxy) 
  - Only a specified range of IPs can be used as private IP

## Elastic IPs(탄력적 IPs)

- ''When you stop and then start an EC2 instance, it can **change** **its public IP.** ''
- If you need to have a **fixed public IP** for your instance, you need an Elastic IP 
- An Elastic IP is a public IPv4 IP you own as long as you don’t delete it 
- You can attach it to one instance at a time

## Elastic IP

- With an Elastic IP address, you can mask the failure of an instance or software by rapidly remapping the address to another instance in your account.
- You can only have 5 Elastic IP in your account (you can ask AWS to increase that).
- Overall, try to **avoid using Elastic IP:**
  - They often reflect poor architectural decisions
  - Instead, use a random public IP and register a **DNS name** to it->much more controls.
  - Or, as we’ll see later, use a **Load Balancer** and don’t use a public IP

## Private vs Public IP (IPv4) In AWS EC2

- By default, your EC2 machine comes with: 
  - A private IP for the internal AWS Network 
  - A public IP, for the WWW. 
- When we are doing SSH into our EC2 machines: 
  - We can’t use a private IP, because we are not in the same network 
  - We can only use the public IP. 
- If your machine is stopped and then started, the public IP can change

## Placement Groups(배치 그룹)

- Sometimes you want control over the EC2 Instance placement strategy
- When you create a placement group, there 3 types of strategies for the group:
  - Cluster—spreads instances into a low-latency(짧은 지연 시간-input to output) hardware in a single Availability Zone
  - Spread(분산)—spreads instances into another hardware (max 7 instances per group per AZ)
    - critical applications use
  - Partition(분열)—spreads instances across many different partitions (which rely on different sets of racks) within an AZ(availability zone). Scales to 100s of EC2 instances per group
    - like Hadoop, Cassandra, Kafka

## Placement Groups Cluster

- every ec2 instances are in the same availability zone.

- Pros: 
  - low latency & great network (10 Gbps bandwidth per sec between instances with Enhanced Networking(향상된 네트워킹) enabled - recommended) 
- Cons: 
  - If the AZ fails, all instances fails at the same time
- Use case: 
  - Big Data job that needs to complete **fast** 
  - Application that needs extremely low latency and high network throughput

## Placement Groups Spread

- each EC2 instances are in differenct hardwares
- Pros: 
  - Can span across(걸쳐있다) several Availability Zones (AZ) 
  - Reduced risk is simultaneous failure (동시 실패)
    - EC2 Instances are on different physical hardware
- Cons: 
  - Limited to 7 instances per AZ per placement group-the application has to have good size, not too big
- Use case:
  - Application that needs to **maximize high availability**(가용성 극대화)
  - Critical Applications where each instance must be **isolated from failure** from each other

## Placement Groups Partition

- Up to 7 partitions per AZ 
- Can span across multiple AZs in the **same region** 
- Up to 100s of EC2 instances 
- The instances in a partition do not share racks with the instances in the other partitions 
- A partition failure can affect many EC2 in the same partition but won’t affect other partitions 
- EC2 instances get access to the partition information as metadata 
- Use cases: HDFS, HBase, Cassandra, Kafka-application that can be partitioned aware to distribute the data and your servers(usually big data applications)

## Elastic Network Interfaces (ENI)

- Logical component in a VPC(virtual private cloud) that represents a virtual network card 
- The ENI can have the following attributes: 
  - Primary private IPv4, one or more secondary IPv4
  - One Elastic IP (IPv4) per private IPv4 
  - One private or Public IPv4 
  - One or more security groups 
  - A MAC address 
-  You can create ENI independently and attach them immediately (move them) on EC2 instances **for failover**(장애 조치)
- binding to a specific availability zone (AZ)
  - if ENI has created in a specific AZ, it can be binding in the specific AZ **ONLY**

## EC2 Hibernate(절전 모드)

- stop, terminate instances 

  - Stop – the data(RAM) on disk is dumped into the EBS(Elastic Block Storage) and kept intact(유지) for the next start
    - and The RAM on the disk is gone
  - Terminate – any EBS volumes (root) also set-up to be destroyed is lost 
    - if not, then it will be kept

- When restart, 

  1. the OS boots & the EC2 User Data script is run 

  2. the OS boots up 

  3. your application starts, caches get warmed up(from EBS), and that can take time!

- EC2 Hibernate: 
  - The in-memory (RAM) state is preserved(보존된다)
  - The instance boot is much faster! (the OS is not stopped / restarted) 
  - Under the hood(실제 안에서는): the RAM state is written to a file in the root EBS volume 
  - The root EBS volume must be encrypted 
- Use cases: 
  - Long-running processing and never stop
  - Saving the RAM state 
  - Services that take time to initialize(초기화)

## EC2 Hibernate – Good to know

- Supported Instance Families – C3, C4, C5, I3, M3, M4, R3, R4, T2, T3, … 
- Instance RAM Size – must be less than 150 GB. 
- Instance Size – not supported for bare metal instances. 
- AMI – Amazon Linux 2, Linux AMI, Ubuntu, RHEL, CentOS & Windows… 
- Root Volume – must be EBS, encrypted, not instance store, and large 
- Available for On-Demand, Reserved and Spot Instances 
- An instance can NOT be hibernated more than 60 days

---

# Amazon EC2 - Instance Storage

## What's an EBS Volume?

- An EBS (Elastic Block Store) Volume is a **network** **drive** you can attach to your instances while they run 
  - network drive: shared storage device on a local area network(LAN) within a business or home.
- It allows your instances to persist(지속) data, even after their termination(종료 후)
- They can only be mounted to one instance at a time (at the CCP level) 
- several EBS Volume can be attached to one instance
- They are bound to a **specific availability zone** 
- it's like a “network USB stick” 
  - one computer to the another computer USB through network
- Free tier: 30 GB of free EBS storage of type General Purpose (SSD) or Magnetic per month

## EBS Volume

-  It’s a network drive (i.e. not a physical drive) 
  - It uses the network to communicate the instance, which means there might be a bit of latency(지연)
  - It can be detached from an EC2 instance and attached to another one quickly 
- It’s locked to an Availability Zone (AZ) -specific
  - An EBS Volume in us-east-1a cannot be attached to us-east-1b 
  - if you want to move a volume across, you first need to **snapshot** it 
- Have a provisioned capacity (size in GBs, and IOPS-IO per sec) - 용량 미리 결정 필요
  - You get billed for all the provisioned capacity 
  - You can increase the capacity of the drive over time

## EBS – Delete on Termination attribute

- Controls the EBS behaviour when an EC2 instance terminates 
  - By default, the root EBS volume is deleted (attribute enabled) 
  - By default, any other attached EBS volume is not deleted (attribute disabled) 
- This can be controlled by the AWS console / AWS CLI 
- Use case: preserve root volume when instance is terminated

## EBS Snapshots

- Make a **backup** (snapshot) of your EBS volume at a point in time 
- **Not necessary to detach** volume to do snapshot, but **recommended** 
- Can copy snapshots across AZ or Region

## EBS Snapshots Features 

- EBS Snapshot Archive(보관)
  - Move a Snapshot to an ”archive tier” that is **75% cheaper** 
  - Takes within 24 to 72 hours for restoring the archive-not immediate 
- Recycle Bin for EBS Snapshots 
  - Setup rules to retain deleted(영구 삭제) snapshots so you can recover them after an accidental deletion 
  - Specify retention(보유) (from 1 day to 1 year) 
- Fast Snapshot Restore (FSR) 
  - Force full initialization(초기화) of snapshot which is already existed to have **no latency on the first use** ($$$high)

## AMI Overview

- AMI = Amazon Machine **Image** 
- AMI are a customized image of an EC2 instance 
  - You add your own software, configuration, operating system, monitoring… 
  - Faster boot / configuration time because all your software is pre-packaged 
- AMI are built for a ** ** (and **can be copied** across regions) 
- You can launch EC2 instances from different AMIs: 
  - A Public AMI: AWS provided 
  - Your own AMI: you make and maintain them yourself 
  - An AWS Marketplace AMI: an AMI someone else made (and potentially sells)

## AMI Process (from an EC2 instance)

1. Start an EC2 instance and customize it 

2. Stop the instance (for data integrity) 

3. Build an AMI – this will also **create EBS snapshots** 

4. Launch instances from other AMIs

## EC2 Instance Store

- EBS volumes are network drives with good but “limited” performance 
- <u>If you need a **high-performance hardware disk**, use EC2 **Instance Store**</u> 
  - Better **I/O performance** - goot throughput(처리량) and high quality disk performance
  - EC2 Instance Store lose their storage if they’re(instance) stopped (ephemeral 임시) 
  - Not for long term storage / EBS is for long term
  - Good for buffer / cache / scratch data / temporary content 
  - Risk of data loss if hardware fails 
  - -> **Backups and Replication**(복제) are your responsibility


## EBS Volume Types

- EBS Volumes come in 6 types 
  - **gp2** / gp3 (**SSD**): General purpose SSD volume that balances price and performance for a wide variety of workloads 
  - io1 / io2 Block Express (**SSD**): Highest-performance SSD volume for mission-critical low-latency(저지연) or high-throughput workloads (고처리량 작업)
  - st1 (**HDD**): Low cost HDD volume designed for frequently accessed, throughput- intensive workloads 
  - sc1 (**HDD**): Lowest cost HDD volume designed for less frequently accessed workloads 
- EBS Volumes are characterized in Size | Throughput | IOPS (I/O Ops Per Sec-성능 측정 단위) 
- When in doubt always consult the AWS documentation – it’s good! 
- Only gp2/gp3 and io1/io2 Block Express can be used as boot volumes - where OS's root is running

## EBS Volume Types Use cases - General Purpose SSD

- GP Solid State Drive - gp2, gp3, io1, io2
- **Cost effective storage, low-latency** 
- System boot volumes, Virtual desktops, Development and test environments 
- 1 GiB - 16 TiB 
- gp3: 
  - Baseline of 3,000 IOPS and throughput of 125 MiB/s 
  - Can increase IOPS up to 16,000 and throughput up to 1000 MiB/s **independently** 
    - **independently set the IOPS and throughput**
- **gp2**: 
  - Small gp2 volumes can burst IOPS to 3,000 
  - Size of the **volume and IOPS are linked**, max IOPS is 16,000 
    - **volume and IOPS are linked together, unlike gp3**
  - 3 IOPS per GB, means at 5,334 GB we are at the max IOPS

## EBS Volume Types Use cases - Provisioned IOPS (PIOPS) SSD 

- Critical business applications with sustained(지속적인) IOPS performance 
- Or applications that need more than 16,000 IOPS 
- Great for **databases workloads** (sensitive to storage perf and consistency) 
- io1 (4 GiB - 16 TiB): 
  - Max PIOPS: 64,000 for Nitro EC2 instances & **32,000 for other** 
  - Can increase PIOPS independently from storage size 
- io2 Block Express (4 GiB – 64 TiB): 
  - Sub-millisecond latency 
  - Max PIOPS: 256,000 with an IOPS:GiB ratio of 1,000:1 
- Supports EBS Multi-attach

## EBS Volume Types Use cases - Hard Disk Drives (HDD) 

- Hard Disk Drive
- Cannot be a boot volume 
- 125 GiB to 16 TiB 
- Throughput Optimized(처리량 최적화) HDD (st1) 
  - Big Data, Data Warehouses, Log Processing 
  - Max throughput 500 MiB/s – max IOPS 500 
- Cold HDD (sc1): 
  - For data that is infrequently accessed 
  - Scenarios where lowest cost is important 
  - Max throughput 250 MiB/s – max IOPS 250

> if you need databases, Then us general purpose SSD, Provisioned IOPS (gp2, gp3, io1, io2)
>
> if you need high throughput and low price, then HDD (st1, sc1)

> if you need over 32,000 IOPS, you need EC2 Nitro and (Io1 or Io2)

## EBS Multi-Attach – io1/io2 family

- **Attach** the same EBS volume **to multiple EC2 instances** in the **same AZ** 
- Each instance has full read & write permissions to the high-performance volume 
- Use case: 
  - Achieve **higher application availability** in clustered Linux applications (ex: Teradata)
  - Applications must manage concurrent write operations (동시 쓰기 작업)
- Up to **16 EC2 Instances at a time** 
- Must use a file system that’s **cluster-aware** (not XFS, EXT4, etc…) 

## EBS Encryption 

- When you create an encrypted EBS volume, you get the following: 
  - Data at rest is encrypted inside the volume 
  - All the data in flight moving between the instance and the volume is encrypted 
  - All snapshots are encrypted 
  - All volumes created from the snapshot 
- Encryption and decryption are handled transparently (you have nothing to do) 
  - EC2 and EBS are handling behind the scene 
- good to use - bec, Encryption has a **minimal impact on latency** 
- EBS Encryption leverages(활성화) keys from **KMS (AES-256)** 
- Copying an unencrypted snapshot allows encryption 
-  Snapshots of encrypted volumes are encrypted

## Encryption: encrypt an unencrypted EBS volume 

1. Create an EBS snapshot of the volume 
2. Encrypt the EBS snapshot ( using copy ) 
3. Create new ebs volume from the snapshot ( the volume will also be encrypted ) 
4. Now you can attach the encrypted volume to the original instance

## Amazon EFS – Elastic File System

- Managed NFS (network file system) that can be mounted on many EC2 
- EFS works with **EC2 instances in multi-AZ** 
- Highly available, scalable, expensive (3x gp2), pay per use
- multiple instances in multi-az can **connect through EFS**

## Amazon EFS – Elastic File System

- Use cases: content management, web serving, data sharing, Wordpress 
- Uses **NFSv4.1 protocol** -security group
- Uses **security group to control** **access** to EFS
- Only Compatible with **Linux based AMI** (not Windows) 
- **Encryption** at rest using KMS(Key Management Service)  
- POSIX file system (~Linux) that has a standard file API 
- File system scales automatically, pay-per-use, no capacity planning

## EFS – Performance & Storage Classes

- EFS Scale 
  - 1000s of concurrent NFS(Network File System) clients, 10 GB+ /s throughput 
  - Grow to Petabyte-scale network file system, automatically 
- Performance Mode (set at EFS creation time) 
  - General Purpose (default) – latency-sensitive use cases (web server, CMS, etc…) 
  - Max I/O – higher latency, throughput, highly parallel (big data, media processing) 
- Throughput Mode 
  - **Bursting** – 1 TB = 50MiB/s + burst of up to 100MiB/s 
  - **Provisioned** – set your throughput regardless of storage size, ex: 1 GiB/s for 1 TB storage 
  - **Elastic** – automatically scales throughput up or down based on your workloads 
    - Up to 3GiB/s for reads and 1GiB/s for writes 
    - Used for unpredictable workloads

## EFS – Storage Classes

- Storage Tiers (lifecycle management feature – move file after N days) 
  - Standard: for frequently accessed files 
  - Infrequent access (자주 액세스 하지 않는)(EFS-IA): cost to retrieve(검색) files, lower price to store files. 
  - Archive: rarely accessed data (few times each year), 50% cheaper 
  - Implement lifecycle policies to move files between storage tiers 
- Availability and durability 
  - Standard: Multi-AZ, great for product
  - One Zone: One AZ, great for dev, backup enabled by default, compatible with IA (EFS One Zone-IA) 
- Over 90% in cost savings

## EBS vs EFS – Elastic Block Storage

- EBS volumes… 
  - **one instance** (except multi-attach io1/io2-but very specific) 
  - are **locked** at the **Availability Zone (AZ) level** 
  - gp2: IO increases if the disk size increases 
  - gp3 & io1: can increase IO independently 
- To migrate an EBS volume across AZ 
  - **Take a snapshot** 
  - Restore the snapshot to another AZ 
  - EBS backups **use IO** and you **shouldn’t run them** while your application is handling a lot of traffic 
- Root EBS Volumes of instances **get terminated by default** if the EC2 instance gets terminated. (you can disable that)

## EBS vs EFS – Elastic File System

- Mounting 100s of instances **across AZ** 
- EFS **share website files** (WordPress) 
- **Only for Linux** Instances (POSIX) 
- EFS has a higher price point than EBS 
- Can leverage Storage Tiers for cost savings 
- Remember: EFS vs EBS vs Instance Store 
