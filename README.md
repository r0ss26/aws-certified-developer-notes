# AWS Certified Developer

[toc]	

## Regions

- AWS has regions all around the world represented by codes e.g. us-east-1

- A **region** is a cluster of data centers

- Most AWS services are scoped to a region.

**Exam Q**: How do you choose an AWS region?

**Answer**:

**Compliance** with data governance and legal requirements, e.g. data never leaves a region without your explicit permission

**Proximity:** The region closes to your users to limit latency

**Available Services:** Not all regions have all services available

**Pricing:** Pricing varies from region to region

### Availability Zones

- Each region has many availability zones, usually 3
- Each availability zone is one or more discrete data centers with redundant power, networking and connectivity
- Connected with high bandwidth networking, which together form a region

Point of presence (edge locations)

- Deliver content to end users with low latency

## IAM

- Identity and access management, global service
- Root account created by default, shouldn't be used or shared
- Users are people within your organization that can be grouped
- Group only contain users
- Users don't have to belong to a group, and can belong to multiple groups

### Permissions

- Users our groups can be assigned JSON documents called policies, these policies define the permissions of the users
- Apply the least privilege principle - don't give a user more permissions than they need

### Inheritance

- users in groups inherit the groups policy
- users can have inline policies if they are not part of a group
- Structure
  - Version (language version)
  - unique id for the policy
  - statement
    - sid, unique id for the statement
    - effect: allow or deny access
    - principal
      - Which account user or role the policy is applied to
    - action
      - list of api call that are denied or allowed
    - resource
      - resources the actions are applied to
    - condition (optional) condition for when the policy is applied

### Multi Factor Authentication

- Virtual MFA device
  - google authentication (phone only)
  - authy (multi-device)
- Universal 2nd factor (u2f) security key
  - physical device
  - multipl root and iam users on a single key
- hardware key fob mfa

## Accessing Aws

- management console (password + mfa) in browser
- AWS CLI (protected by access keys)
- WS SDK: for code, calling API's win code, protected by access keys
  - Language specific
  - embed in your application
- Access key id is like username
- secret access key is like a password (don't share with others)

## Cloudshell

- cloud terminal

## IAM Roles

- Some AWS service will need to perform actions on your behalf
- To do so, we will assign permission to AWS services with iam roles
- used to grant permissions to AWS services
- Common roles
  - ec2 instance roles
  - lambda function roles
  - roles for cloudformation

## IAM Security Tools

- IAM credentials report (account-level)
  - a report that lists all your account susers and the status of their various credentials

## IAM Best practices

- Don't use the root account except for the AWS account setup
- one physical user = one iam user
- assign users to groups and assign permission to groups
- use and enforce the use of MFA
- create a strong password policy
- create and use roles for giving permissions to AWS services
- use access keys for programmatic access
- audit permissions of  your account with the iam credentials report
- never share iam users & access keys

## EC2

- Most popular AWS service
- elastic compute cloud
- infrastructure as a service
- not just one service, consists of
  - renting virtual machines
  - store data on virtual drives
  - distribute load across machines
  - scaling the services using an auto-scaling group
- fundamental to understand how the cloud works
- linux, windows, mac
- computer power & cores
- RAM
- storage space
  - network
  - hardware
- network card
- public ip
- firewall rules
- bootstrap script (configure at first launch)

### Instance types

- m5.2xlarge
  - m: instance class
  - 5: generation
  - 2xlarge :size e.g. ram, cpu
- general purpose
  - diversity of workloads
- compute optimized
  - batch processing data
  - media transcoding
  - high performance web server
  - machine learning
  - dedicated gaming server
- Memory optimized
  - fast performance for large data in memory
  - relational/non relational database
  - distributed web scale cache store
  - real time data processing
- storage optimized

### Security Groups

- consist of inbound and outbound rules for allowing traffic through given ports using various protocols

- ssh port 22
- ftp port 21
- sftp 22 (upload file using ssh)
- 80 http, unsecured website
- 443 https, secured website
- 3389 remote desktop protocol (rdp) log into a windows instance

**Exam question:** which ec2 launch type fits a scenario?

### EBS Volumes

- Elastic Block Store
- network drive you can attach to instances
- your isntance can persist data even after they are terminated
- can only be mounted to onoe instance a a time
- can be detached and attached to other ec2 instances
- locked to an availability zone

### Snapshots

- snapshots are a backup
- recommended to detach the volume to do a snapshot
- can copy snapshots across availability zones or regions

### AMI

- amazon machine imae
- customization of an ec2 image
  - ass your own software, configuration, OS, monitoring
  - faster boot/configuration time because all your software is pre-packaged
  - AMI's are  built for a specific region
- you can launch ec2 instance from
  - a public AMI (AWS provided)
  - your own AMI: you make and maintain them yourself
  - AWS marketplaces: an AMI someone else made (and potentially sells)

### EC2 Instance Store

- physically attached to the ec2
- lost i ec2 instance is shutdown
- not a durable long term data store
- good for buffer/cache/scratch data/temporary content
- risk of data loss if hardware fails
- backups are your responsibility

### EBS Volume Types

- gp2/gp2 SSD general purpose
- io1/io2: high performance SSD
- st 1 low cost HDD 
- sc 1 lowest cost HDD

### EBS Multi Attach

- Attach the same EBS volume to multiple EC2 instances in the same AZ
- Achieve higher application availability in clustered linux applications
- application must manage concurrent write operations
- must use a file system that is cluster aware

### Elastic File System

- Managed network file system that can be mounted on many EC@
- EFS works with EC@ instances in multi-az
- highly available
- expensive
- Can be mounted on many EC2 instances
- works across multiple EC2 instances
- Use cases: content management, web servers, data sharing, wordpress
- use security group to control access to EFS
- Only compatible with linux based Amazon Machine Images

### EBS Vs. EFS

- EBS volumes can only be attached to one instance at a time and are locked to a single AZ
- Root EBS volumes of instances get terminated by default
- Can be mounted to multiple EC2 across different Availability Zones
- EFS share website files
- EFS is only for linux instances
- EFS is more expensive (3x)
- Can use EFS Infrequent Access for infrequently accessed files for cost savings

## Elastic Load Balancing

### Scalability & High Availability

Scalability means the application / system can handle greater loads by adapting

There are two kinds of scalability

- Vertical scalability
  - Increase the size of your instance
  - e.g. go from a t2.micro to a t2.large
  - common in non distributed systems for example a database
  - Hardware limits

- Horizontal scalability (elasticity)
  - Increase the number of instances / systems for your application
  - distributed systems
  - Cloud makes this easy

High availability

- Usually goes hand in hand with high availability
- Means running your app in at least 2 data centers (Availability Zones)
- The goal is to survive a data center loss / outage

### Load Balancing

- Servers that forward traffic to multiple services (e.g. EC2 instances)
- Spread load across multiple downstream instances
- expose a single point of access (DNS to your application)
- Seamlessly handle failures of downstream instances
- Provide SSL termination for your websites
- Enforce stickiness with cookies
- High availability across zones
- Separate public traffic from private traffic

### Elastic Load Balancer

- managed load balancer
- aws provides configuration
- integrated with many aws services
- uses health check to port and route /health to check health and route traffic to healthy instances

### Types of Load balancers on AWS

#### Classic CLB 

- uses TCP
- HTTP HTTPS TCP SSL
- Deprecated
- Fixed hostname

#### Application Load Balancer (ALB)

- HTTP HTTPS Websocket
- Uses HTTP
- load balancing to multiple http applications
- routing based on different target groups
  - e.g. routing based on URL
  - hostname
- Good for micro-services and container based application (e.g. docker & amazon ecs)
- Has port mapping to redirect to a dynamic port in ECS
- comes with a fixed hostname
- app servers dont see ip of the client directly
  - The true ip of the client is inserted in the header x-forwarded-for
  - Add the port x-forwarder-port
  - protocol x-forwarded-proto

#### Network Load Balancer (NLB)

#### Gateway Load Balancer

- Operates at layer 3 (network layer)
- IP Protocol

### Load Balancer Security

- Any incoming http/s requests on security groups
- Then EC2 instances should then only allow incoming HTTP traffic from your load balancer

### Target Groups

- EC2 instances
- ECS tasks
- Lambda functions
- IP addresses (must be private)

### Sticky Sessions

- Also called session affinity
-  The client that does two requests to the load balancer will always be redirected to the same instance behind the load balancer
- Works for classic load balancers and application load balancers
- uses a cookie to determine stickiness, has an expiry date
- used to make sure a user doesn't loose session data
- application based cookie
  - custom cookie
    - generated by the target (the application)
    - can include custom attributes
    - must be specififed indivbidually for each target group
  - application cookie
    - generated by the load balancer
    - AWSALBAPP
  - duration based cookie
    - genereated by the load balancer

### Cross Zone Load Balancing

- Load balancers in each AZ distrubute traffic to all instances across all of the AZ's
- Application Load Balancer
  - always on and can't be disables
- Network Load Balancer
  - Disables by default but can be enabled
  - pay for data transferred between availability zones
- Classic Load Balancer
  - Through console = enables by default
  - thrroug CLI/API disabled by default
  - no charges for inter-AZ data transfer

### SSL Certificates

- Allows traffic between clients and load balancer to be encrypted while in transit
- Secure Socket Layer
- TLS - Transport Layer Security
- TLS certificates are mainly used as they are newer than SSL but people still say SSL
- SSL certificates are issused by certificate authories
- Have an expiration date
- must be renewed regularly
- Users connect over https to the load balancer
- load balancer performs ssl certificate termination
- load balancer directs to the ec2 instance over http
- You can manage certificates using ACM
- HTTP listener
  - you must specify a default certificate
  - clients can use SNI (Server name indication) to specify the hostname they reach

#### SNI

- Solves the problem of loading multiple SSL certificates onto one web server
- It's a newer protocol and requres the client to indicate the hostname of the targer server in the initial SSL hahndshake
- The server will then find the correct certificate or return the default one

- Classic load balancer supports only one SSL cert
- ALB supports multiple listeners with multiple SSL certs and uses SNI for this to work
- NLB same as ALB

### Connection Draining

- Give some time for instances to complete the active request while the in stance is being marked as unhealthy
- once drained the load balancer will stop sending request to the new ec2 instance which is being marked unhealthy
- between 1 and 3600 seconds
- can be disables (set to 0 seconds)
- use shorter times for shorter requests

### Auto Scaling Group

- Scales EC2 instances to match an increased load
- removed EC2 instances to match decreased load
- Can define min and max number of instances running in an ASG
- Automatically register new instances to a load balancer
- load balancer can direct traffic to asg
- asgs have
  - launch confniguration
    - ami + instance type
    - user data (launch script)
    - ebs volumes 
    - security groups
    - ssh key pair
  - min size / max size / initial capacity
  - network information
  - triggers for load
- use cloud watch alarms to scale
- can be any metric such as average cpu

## Relational Database Service (RDS)

- Allows you to create databases in the cloud that are managed by AWS
  - Postgres
  - MySQL
  - MariaDB
  - Oracle 
  - Microsoft SQL Server
  - Aurora (AWS Proprietary  database)
- Automated provisioning, OS patchhing
- Continuous backups and restore to specifictimestamp
- Monitoring dashboard
- Read replicas for improved read performance
- Multi AZ setup for Disaster Recovery
- Maintenance upgrades
- Scaling
- Storage backed by BS (gp2 or io1)

### Backups

- Daily full backup of the database during the maintenance window
- transaction logs are backed-up by RDS every 5 minutes
- 7 day retention for automated backups by default
- DB Snapshots
  - Manually triggered by the user
  - Retention of backup for as long as you want

### RDS Storage - Auto Scaling

- Increase your storage on your RDS DB instance dynamically

### Read Replicas

- For scaling database reads
- Within AZ,
- Cross AZ
- Cross region
- Connection string must be updated
- Use cases
  - prod database is taking on normal load
  - new app needs to access database that will put increased load on database
  - create a read replica and use that in the new app
  - can only be used for select statements
- Network costs
  - Cost when data is transferred when data goes from one AZ to another
  - If read replica is within the same region, you don't pay a fee
  - If the read replica is in another region, you will incur a replication fee

### Multi AZ (Disaster Recovery)

- Create a new database instance in a different availabily zone
- replication is done syncrhonously
  - replicate every change in master at the same time as the standby
- You get one DNS that automatically points to the fallback database if you master fails
- No manual intervention i.e no need to update connection strings
- Read replicas can be set up as multi AZ

### Convert single-AZ RDS to Multi-AZ RDS

- No downtime - dont need to stop the database
- Click modify on the database
  - A snapshot is taken
  - A new db is restored from the snapshot in a new AZ
  - synchronization is established between the two databases

### RDS Security

- At rest encryption
  - encrypt master & read replicas using AWS KMS - AES-256 encryption
  - Encryption has to be defined at launch time
  - master has to be encrypted for the read replicas to be encrypted
- In-flight encryption
  - SSL certificates to encrypt data to RDS in flight
  - Provide SSL options with trust certificate when connection to database
- snapshots are encrypted based on the settings of the database
- Network security
  - deployed within a private subnet
  - uses security groups (the same concept as for EC2)
- Access manaement
  - IAM policies control who can manage aws rds
  - usename and password to log in to the database
  - IAM based authentication for MySql and PostGres
- IAM authentication
  - authentication token obtained using authentication token via api call
  - auth token has lifetime of 15 minutes

### Amazon Aurora

- Postgres and MySql are supported
- Cloud optimized 5x performances improvements over mysql, 3x postgres
- Storage automatically grows in increments of 10GB up to 64TB
- No need to monitor disk
- 15 read replicas
- Failover is instantaneous - faster than multi-az
- costs more than RDS
- Stores 6 copies of your data across 3 az
- striped across 100's of volumes
