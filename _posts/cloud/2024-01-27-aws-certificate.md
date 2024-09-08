---
title:  "AWS Certificate Notes"
date:   2024-01-27 -0500
categories: cloud 
tags: AWS
header:
    teaser: /assets/img/training-guidence.png
---

Notes for AWS Online Course

## Introdution

### Cloud Computing

Deployment models:
- On-premises: company host and maintain hardware and infrastructure
- Cloud: delivery IT service via Internet
- Hybrid: On-premises + Cloud

Advantages of Cloud:
1. Pay-as-you-go: only pay for resource you need
2. Benefit from massive economies of scale: achieve lower cost, aggregated custormer in the cloud
3. Stop guessing capacity: no capacity limit
4. Increase speed and agility: reduce the time to make resource available
5. Realize cost savings: No maintainence fee for infrastructure
6. Go global in minutes: lower latency



### Infrastructure

Region -> Availability Zone -> Data Center

Choosing the region: 

- Latency
- Price
- Service Availability
- Data Compliance: regulation requirement 

Edge locations: global locations where content is cached.

### Interaction with AWS

API

- Management Console

- CLI
- Download locally
  - Cloud shell

- SDK



## Security

Customer + AWS



AWS Responsibility: 

|         **Category**         |         **Examples of AWS Services in the Category**         |                    **AWS Responsibility**                    |
| :--------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| **Infrastructure services ** | Compute services, such as Amazon Elastic Compute Cloud (Amazon EC2) | AWS manages the underlying infrastructure and foundation services. |
|   **Abstracted services **   | Services that require very little management from the customer, such as Amazon Simple Storage Service (Amazon S3) | AWS operates the infrastructure layer, operating system, and platforms, in addition to server-side encryption and data protection. |



Customer Responsibility:

|         **Category**         |         **Examples of AWS Services in the Category**         |                 **Customer Responsibility**                  |
| :--------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| **Infrastructure services ** | Compute services, such as Amazon Elastic Compute Cloud (Amazon EC2) | Customers' control the operating system and application platform, in addition to encrypting, protecting, and managing customer data. |
|   **Abstracted services **   | Services that require very little management from the customer, such as Amazon Simple Storage Service (Amazon S3) | Customers' are responsible for customer data, encrypting the data, and protecting it through network firewalls and backups. |

Example: 

- Choosing a Region for AWS resources in accordance with data **sovereignty regulations**
- Implementing **data-protection mechanisms**, such as encryption and scheduled backups
- Using **access control** to limit who can access your data and AWS resources



### Root User

2 sets credentials 

- email + password: management console

- access key: programmatic request (CLI + API)

  - Access key ID
  - Secret access ID

  > Delete access key for root to keep safety



Safety Practice:

- strong password
- <u>Multi-factor authentication (MFA)</u>
  - Something you know: username, password, PIN number
  - Something you have: one-time passcode from hardware/mobile app
  - Something you are: fingerprint, face scanning
- No share
- Disable / delete access key associated with root
- Create Identity and Access Management (IAM) user for task



### Supported MFA devices

|         Device          |                         Description                          |                 ***\*Supported Devices\****                  |
| :---------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|     **Virtual MFA**     | A software app that runs on a phone or other device that provides a one-time passcode. These applications can run on unsecured mobile devices, and because of that, they might not provide the same level of security as hardware or FIDO security keys. | Twilio Authy Authenticator, Duo Mobile, LastPass Authenticator, Microsoft Authenticator, Google Authenticator, Symantec VIP |
| **Hardware TOTP token** | A hardware device, generally a key fob or display card device, that generates a one-time, six-digit numeric code based on the time-based one-time password (TOTP) algorithm. |                    Key fob, display card                     |
| **FIDO security keys**  | FIDO-certified hardware security keys are provided by third-party providers such as Yubico. You can plug your FIDO security key into a USB port on your computer and enable it using the instructions that follow. | [FIDO Certified products](https://fidoalliance.org/certification/fido-certified-products) |



### Identity and Access Management (IAM)

> Authentication: verify identity
>
> Authorization: give premission to access resource and service

- manage access to your AWS account and resources
- provides a centralized view of who and what are **allowed** inside your AWS account (authentication), and who and what have **permissions** to use and work with your AWS resources (authorization).

#### Features

- Global: any region is fine
- Integrated with AWS service
- Shared access: without having to share your password and key.
- MFA
- identity federation: allows users with passwords elsewhere—like your corporate network or internet identity provider—to get temporary access to your AWS account
- Free to use

#### IAM Group

Access management in scalable way

- User-group: n-n mapping
- Group-group: uninheritiable

#### IAM Policy

Evaluate in both group-level and user-level

`JSON` document for access management

Example 1: 

```json
{
	"Version": "2012-10-17",  // Version
	"Statement": [{
		"Effect": "Allow",  		// Effect
		"Action": "*", 					// Action
		"Resource": "*"  				// Resource
	}]
}
```

**4 Elements:**

- ***Version***: defines the version of the policy language

- ***Effect***: specifies whether the policy will **allow** or **deny** access
- ***Action***: describes the type of action
- ***Resource***:  specifies the object or objects that the policy statement covers

Example 2:

```JSON
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyS3AccessOutsideMyBoundary",
      "Effect": "Deny",
      "Action": [
        "s3:*"
      ],
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "aws:ResourceAccount": [
            "222222222222"
          ]
        }
      }
    }
  ]
}
// Comments:
// Block the access to S3 resource unless the resource account is "222222222222"
```

#### IAM Role

Temporary access to AWS

- No static login credentials
- assumed programmatically
- Temporary for configurable time amount
- credentials expire and are rotated

Use Case Example: Federated Roles for Identity Provider (IdP)

### Security Best Practices

- Lock down root
- Follow the principle of least privilege (Only necessary permission)

- Use IAM appropriately
- Use IAM roles
- Considering IdP usage (one employee with access to multiple AWS account)

- Regularly review and remove unused security configuration



## Computation

Server: handle HTTP requests, send response

Example: 

- Windows option: such as Internet Information Services
- Linux option: Apache HTTP server, Nginx, Apache Tomcat

AWS computation service:

- Virtual machines (VMs): emulate physical server, e.g. Elastic Compute Cloud (EC2)

  >  Install hypervisor on host to run VM

- Container services

- Serverless



### Elastic Compute Cloud, EC2

web service that provides secure, **resizable** capacity in the cloud

definition for EC2 instance:

- Hardware specification: CPU, memory, network, storage
- Logical configuration: Networking location, firewall rules, authentication, OS



#### Amazon Machine Image (AMI)

OS, storage mapping, architecture type, launch permission. Preinstalled software application 

> AMI and EC2 instance: class & object, cake recipe & cake

Categories:

- Quick Start AMI
- Marketplace AMI: popular open-source and commercial software form 3rd-party vendors
- My AMIs: create from instance
- Community AMI: AWS user community
- Custom image: image builder



#### Instance

Type example: `c5n.xlarge`

- `c`: instance family, compute optimized family
- `5`: generation
- `n`: additional attribute
- `xlarge`: instance size

instance family

|    **Instance family**     |                       **Description**                        |                        **Use Cases**                         |
| :------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|    **General purpose **    | General purpose instances provide a balance of compute, memory, and networking resources, and can be used for a variety of workloads. | Ideal for applications that use these resources in equal proportions, such as web servers and code repositories |
|   **Compute optimized **   | Compute optimized instances are ideal for compute-bound applications that benefit from high-performance processors. | Well-suited for batch processing workloads, media transcoding, high performance web servers, high performance computing (HPC), scientific modeling, dedicated gaming servers and ad server engines, machine learning inference, and other compute intensive applications |
|   **Memory optimized **    | Memory optimized instances are designed to deliver fast performance for workloads that process large datasets in memory. | Memory-intensive applications, such as high-performance databases, distributed web-scale in-memory caches, mid-size in-memory databases, real-time big-data analytics, and other enterprise applications |
| **Accelerated computing ** | Accelerated computing instances use hardware accelerators or co-processors to perform functions such as floating-point number calculations, graphics processing, or data pattern matching more efficiently than is possible in software running on CPUs. | Machine learning, HPC, computational fluid dynamics, computational finance, seismic analysis, speech recognition, autonomous vehicles, and drug discovery |
|   **Storage optimized **   | Storage optimized instances are designed for workloads that require high sequential read and write access to large datasets on local storage. They are optimized to deliver tens of thousands of low-latency random I/O operations per second (IOPS) to applications that replicate their data across different instances. | NoSQL databases (Cassandra, MongoDB and Redis), in-memory databases, scale-out transactional databases, data warehousing, Elasticsearch, and analytics |
|     **HPC optimized **     | High performance computing (HPC) instances are purpose built to offer the best price performance for running HPC workloads at scale on AWS. | Ideal for applications that benefit from high-performance processors, such as large, complex simulations and deep learning workloads |



Architecting for high availability: 2 instances in different Availability Zone



#### Lifecycle 

- Pending
- Running
- Rebooting: rebooting OS, keep DNS name and IPv4
- Stopping
- Stopped: shutdown PC
- Shutting-down
- Terminated

Stop and stop-hibernate: 

- Stop loss data in RAM
- Stop-hibernate store data in RAM to EBS(Elastic Block Store)



#### Price

- On-demand instance: pay for hours and seconds, no upfront payment or long-term commitments   
- Spot instance: flexible start and end times  
- Saving plans: long-term commitment, consistent amount of usage
- Reserved instance: steady state usage that might require reserved capacity
- Dedicated host: physical, use your existing server-bound software licenses



### Container

Usage: web applications, lift and shift migrations, distributed applications, and streamlining of development, test, and production environments.

A container is a standardized unit that packages your **code** and its **dependencies**.

> Difference with VM: containers share OS

#### Orchestrating containers

many containers on many instances

Large-scale computing:

- How to place your containers on your instances
- What happens if your container fails
- What happens if your instance fails
- How to monitor deployments of your containers

Services: 

- Amazon Elastic Container Service (Amazon ECS) 

- Amazon Elastic Kubernetes Service (Amazon EKS

|                 ECS                 |             EKS             |      |
| :---------------------------------: | :-------------------------: | :--: |
| install agent on container instance | run containers on work node |      |
|        Container calls task         |     Container calls pod     |      |
|           AWS Native Tech           |         Kubernates          |      |



### Serverless

Features: 

- There are no servers to provision or manage.
- It scales with usage.
- You never pay for idle resources.
- Availability and fault tolerance are built in.

#### Fargate

AWS Fargate is a purpose-built serverless compute engine for **containers**. 

#### Lambda

Lambda runs your **code** on a high availability compute infrastructure and requires no administration from the user. 

Components:

- Function: resource for invokation
- Tigger: when to run function
- Event: JSON-formatted document, data for processing
- Application Environment: secure and isolated runtime environment, manages the processes and resources
- deployment package: deploy code
  - `.zip` file archive
  - container image
- Runtime: language-specific environment 
- Lambda function handler: method in function

Charge for invoke times and running time

## Networking 

IP address

IPv4: 4*8 bits(octets)



Classless Inter-Domain Routing (CIDR): specify network size

Example: 192.168.1.0/24 (first 24 bits **FIXED**)

largest range: /16



#### Virtual Private Cloud, VPC

Factors:

- Name
- Region, spans all Availability Zone
- IP range in CIDR notation



Subnet specification:

- VPC
- Availability Zone
- CIDR block for subnet



Reserved IPs:

5 IP addresses

Example: 10.0.0.0/22 VPC divided into 4 equal-sized subnets

| IP address |           Usage           |
| :--------: | :-----------------------: |
|  10.0.0.0  |      Network address      |
|  10.0.0.1  |     VPC local router      |
|  10.0.0.2  |        DNS server         |
|  10.0.03   |        Future use         |
| 10.0.2.255 | Network broadcast address |



Gateways:

- Internet gateway: connect VPC to Internet (like modem)
- virtual private gateway: connect VPC to another private network (custom gateway)



AWS direct connect: secure physical connection between on-premise data center and VPC



Routing:

- main route table: allow traffic between all subnets in the local network
- custom route table: **override** main route table



VPC security: 

- Network Access Control List (ACL): virtual firewall at subnet level

- security groups: EC2 

  Default: Block inbound and allow outbound

## Storage

Categories:

- File storage

  File storage is ideal when you require centralized access to files that must be easily shared and managed by multiple host computers

  Require file locking and integration with communication protocols

  *Usecase*: web serving, analytics, media and entertainment, home directory

- Block storage: splits files into **fixed-size** chunks of data called blocks that have their own addresses

  retrieve efficiently, fast, use less bandwidth, low-latency

  *Usecase*: transactional workloads, containers, virtual machine

- Object storage: objects are stored in a bucket using a flat structure, meaning there are no folders, directories, or complex hierarchies.

  When you want to change one character in an object, the entire object must be updated.

  Store large and unstructured datasets

  *Usecase*: Data archiving, backup and recovery, rich media



#### Elastic File System, EFS

A set-and-forget file system that automatically grows and shrinks as you add and remove files

Standard: multi-AZ



#### FSx

- NetApp ONTAP: drop-in replacement for existing ONTAP deployments
- OpenZFS: move data residing in on-premises ZFS or other Linux-based file servers to AWS
- Windows File Server: accessible over the Service Message Block (SMB) protocol / drop-in replacement for Windows file server deployments.
- Lustre: designed for applications that require fast storage



#### Elastic Block Store, EBS

Located on disks which are physically attached to the host computer

EBS is block-level storage (EBS volume) you can attach to EC2 instance (attach external drive to laptop)

Features:

- Detachable
- Distinct: sperate from computer
- Size-limited: limited to external drive size
- 1-to-1 connection: Most EBS only attach to 1 instance



Scaling-up: 

- Increase volume size
- attach multiple volumes

Usecase: 

- operating system
- database
- enterprise application
- big data analytics engines



Type: SSD (Solid State Drive) / HDD (Hard Disk Drive)



Benefits:

- High availability
- Data persistence: persists when instance doesn't
- Data encryption 
- Flexibility
- Backup



Snapshot: incremental backups that only save the blocks on the volume that have changed after your most recent snapshot



#### Simple Storage Service, S3

##### Basics

object storage service, need to create bucket to store objects

> object = file + metadata

ID: bucket name, key, version ID



bucket name must be unique across all AWS accounts in all AWS Regions within a partition. 

> Partition is a grouping of Regions: Standard Regions, China Regions, and AWS GovCloud (US)

Flat structure, but could use prefixes and delimiters in key to imply logical hierarchy 

Example: http://testbucket.s3.amazonaws.com/2022-03-01/cat.jpg

- `testbucket`: bucket name
- `2022-03-01`: prefix
- `cat.jpg`: object key



Usecase:

- Backup and storage
- media hosting
- software hosting
- Software delivery
- data lake
- Static websites
- static content 

##### Security

- IAM policies
- S3 bucket policies
- encryption

##### Class

|                 **Storage Class**                  |                       **Description**                        |
| :------------------------------------------------: | :----------------------------------------------------------: |
|                  **S3 Standard**                   | This is considered general-purpose storage for cloud applications, dynamic websites, content distribution, mobile and gaming applications, and big data analytics. |
|             **S3 Intelligent-Tiering**             | This tier is useful if your data has unknown or changing access patters. S3 Intelligent-Tiering stores objects in three tiers: a frequent access tier, an infrequent access tier, and an archive instance access tier. Amazon S3 monitors access patterns of your data and automatically moves your data to the most cost-effective storage tier based on frequency of access. |
| **S3 Standard-Infrequent Access (S3 Standard-IA)** | This tier is for data that is accessed less frequently but requires rapid access when needed. S3 Standard-IA offers the high durability, high throughput, and low latency of S3 Standard, with a low per-GB storage price and per-GB retrieval fee. This storage tier is ideal if you want to store long-term backups, disaster recovery files, and so on. |
| **S3 One Zone-Infrequent Access (S3 One Zone-IA)** | Unlike other S3 storage classes that store data in a minimum of three Availability Zones, S3 One Zone-IA stores data in a single Availability Zone, which makes it less expensive than S3 Standard-IA. S3 One Zone-IA is ideal for customers who want a lower-cost option for infrequently accessed data, but do not require the availability and resilience of S3 Standard or S3 Standard-IA. It's a good choice for storing secondary backup copies of on-premises data or easily recreatable data. |
|          **S3 Glacier Instant Retrieval**          | Use S3 Glacier Instant Retrieval for archiving data that is rarely accessed and requires millisecond retrieval. Data stored in this storage class offers a cost savings of up to 68 percent compared to the S3 Standard-IA storage class, with the same latency and throughput performance. |
|         **S3 Glacier Flexible Retrieval**          | S3 Glacier Flexible Retrieval offers low-cost storage for archived data that is accessed 1–2 times per year. With S3 Glacier Flexible Retrieval, your data can be accessed in as little as 1–5 minutes using an expedited retrieval. You can also request free bulk retrievals in up to 5–12 hours. It is an ideal solution for backup, disaster recovery, offsite data storage needs, and for when some data occasionally must be retrieved in minutes. |
|            **S3 Glacier Deep Archive**             | S3 Glacier Deep Archive is the lowest-cost Amazon S3 storage class. It supports long-term retention and digital preservation for data that might be accessed once or twice a year. Data stored in the S3 Glacier Deep Archive storage class has a default retrieval time of 12 hours. It is designed for customers that retain data sets for 7–10 years or longer, to meet regulatory compliance requirements. Examples include those in highly regulated industries, such as the financial services, healthcare, and public sectors. |
|                 **S3 on Outposts**                 | Amazon S3 on Outposts delivers object storage to your on-premises AWS Outposts environment using S3 API's and features. For workloads that require satisfying local data residency requirements or need to keep data close to on premises applications for performance reasons, the S3 Outposts storage class is the ideal option. |

##### Versioning

version ID for object

recover objects from accidental deletion or overwrite

States:

- unversioned (default)
- Versioning-enabled
- Versioning-suspended: new object no version, old object keep version



##### Lifecycle

- **Transition actions** define when objects should transition to another storage class.
- **Expiration actions** define when objects expire and should be permanently deleted.



## Database

Service:

|                      **AWS Service(s)**                      | Database Type |                        **Use Cases**                         |
| :----------------------------------------------------------: | :-----------: | :----------------------------------------------------------: |
|           **Amazon RDS, Aurora, Amazon Redshift**            |  Relational   |        Traditional applications, ERP, CRM, ecommerce         |
|                         **DynamoDB**                         |   Key-value   | High-traffic web applications, ecommerce systems, gaming applications |
| **Amazon ElastiCache for Memcached, Amazon ElastiCache for Redis** |   In-memory   | Caching, session management, gaming leaderboards, geospatial applications |
|                    **Amazon DocumentDB**                     |   Document    |         Content management, catalogs, user profiles          |
|                     **Amazon Keyspaces**                     |  Wide column  | High-scale industrial applications for equipment maintenance, fleet management, route optimization |
|                         **Neptune**                          |     Graph     |  Fraud detection, social networking, recommendation engines  |
|                        **Timestream**                        |  Time series  | IoT applications, Development Operations (DevOps), industrial telemetry |
|                       **Amazon QLDB**                        |    Ledger     | Systems of record, supply chain, registrations, banking transactions |



#### RDS

- Commercial: Oracle, SQL Server
- Open-source: MySQL, PostgreSQL, MariaDB
- Cloud native: Aurora



RDS = compute (instance) + storage (EBS, cluster volume for Aurora)

instance type: 

- Standard (`m`) : balance of compute, memory, network
- Memory optimized (`r`/`x`) : accelerate workload
- Burstable (`t`) : basic CPU + ability to burst

Storage type:

- General Purpose SSD (`gp2`): development and testing environments
- Provisioned SSD (`io1`): production environments, I/O-intensive workloads
- Magnetic (`standard`): backward compatibility



RDS in VPC: private, no route to internet gateway, only achievable for backend



Backup

- automated: DB instance + transaction log
  - retaining backups: 0~35 days
  - Point-in-time recovery: restored from specific time point
- Manual



Redundancy: 2 instances in different AZ

- primary
- standby

Failover: DNS name



Security: 

- IAM
- Security group
- encryption
- (Security Socket Layer) SSL/ (Transport Layer Security) TLS



#### Purpose-built Database

##### DynamoDB

NoSQL

High-scale application and serverless application

**Core Components:**

- table: collection of items
- item: collection of attributes
- attribute: fundamental data element

**Security: **

- Redundancy
- encryption (AWS Key Management Service, KMS)
- IAM



##### ElasticCache

In-memory caching solution

support for Redis, Memcached

##### MemoryDB for Redis

In-memory, Ultra-fast

##### DocumentDB (MongoDB campatibility)

store and query rich documents

##### Keyspaces for Apache Cassandra

high-volume applications with straightforward access patterns

##### Neptune

 highly connected data with a rich variety of relationships

##### Timestream

time series database service

##### Quantum Ledger Database (Amazon QLDB)

purpose-built ledger database that provides a complete and cryptographically verifiable history of all changes made to your application data.



### Monitoring

The act of **collecting**, **analyzing**, and **using** data to **make decisions** or **answer questions** about your IT resources and systems is called monitoring.



Metrics: CPU utilization, network utilization, disk performance, memory utilization, logs

Service-speicific metrics type: 

- S3 Bucket:
  - size of object
  - number of object
  - number of HTTP request

- RDS
  - DB connection
  - CPU utilization
  - Disk space consumption
- EC2
  - CPU utilization
  - network utilization
  - disk performance
  - status check



Benefits of monitoring:

- Respond proactively
- Improve performance and reliability
- Recognize security threats and events: create baseline and detect abnormality
- Make data-driven decisions
- create cost-effective solution



#### CloudWatch

CloudWatch is a monitoring and observability service that collects your resource data and provides actionable insights into your applications.

Features:

- Detect anomalous behavior in your environments.
- Set alarms to alert you when something is not right.
- Visualize logs and metrics with the AWS Management Console.
- Take automated actions like scaling.
- Troubleshoot issues.
- Discover insights to keep your applications healthy.



Collect, Monitor, Act, Analyze



Basic monitoring (Free): automatically send metrics to CloudWatch for free at a rate of 1 data point per metric per 5-minute interval.

Detailed monitoring (Charged): shrink interval



Custom metrics: webpage load time, request error rates, #process/thread, work amount



Log terminology:

- event: record of activity, timestamp + event message
- stream: log events belonging to same resource
- Group: log streams sharing retention and permission settings



Alarm setting: threshold, time period, action

State of alarm: OK/ALARM/INSUFFICIENT DATA(just start)



#### Availability Solution

**Redundancy** is important for availability

Challenges:

- Replication process
- Customer redirection: DNS name(update issue)
- Types of high availability
  - Active-passive
  - Active-active: load scalability

#### Elastic Load Balancer, ELB

Traffic redirect algorithm: round robin

Features: 

- Hybrid mode
- High availability
- Scalability



Health Check: 

- Establishing a connection to a backend EC2 instance using TCP and marking the instance as available if the connection is successful.
- Making an HTTP or HTTPS request to a webpage that you specify and validating that an HTTP response code is returned.



Components:

- Rules: source IP + target group
- listeners: client side, port + protocol
- target group: backend server



Type:

- Application LB (7th layer of OSI)
  - routes traffic based on requested data
  - send response directly to the client
  - Use TLS offloading
  - Authenticate users
  - Secures traffic
  - Support sticky session (request must be sent to same backend server)

- Network LB (4th layer of OSI)
  - Sticky session
  - Low latency
  - Source IP address
  - Static IP support
  - Elastic IP address support
  - DNS failover
- Gateway LB (3rd/4th layer of OSI)
  - High availability
  - Monitoring
  - Streamlined deployment
  - Private connectivity



#### Auto Scaling

- Vertical Scaling: increase instance size

  active-passive system

  - Stop passive instance
  - Change instance size / type
  - Shift traffic to passive instance
  - Stop, change size and restart

- Horizontal Scaling: add additional instance

  Active-active system



Features:

- Automatic scaling
- Scheduled scaling
- Fleet management: auto replace unhealthy EC2 instance
- Predictive scaling
- Purchase options
- Amazon EC2 availability



Configure EC2 Auto Scaling Components:

- Launch template or configuration

  - Amazon Machine Image (AMI) ID
  - instance type
  - security group
  - additional EBS volume

  > support versioning for rolling back
  >
  > Recommend: launch template > launch configuration (you cannot use a previously created launch configuration as a template)

- Auto scaling group

  Capacity settings:

  - Minimum capacity
  - Desired capacity
  - Maximum capacity

- Scaling policy

  - Simple scaling policy

    > add an EC2 instance if the CPU utilization across all instances is above 65 percent
    >
    > above 85% -> step policy

  - step scaling policy

    > add two more instances when CPU utilization is at 85 percent and four more instances when it’s at 95 percent.

  - Target tracking scaling policy

    > Alarm based -> average metric based









