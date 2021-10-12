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

- Managed network file system that can be mounted on many EC2 instances
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
- EFS Can be mounted to multiple EC2 across different Availability Zones
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
- Writer endpoint for writing data
- reader endpoint for reading data which is connected to read replicas via a load balancer
- Don't connect via the dns address - connect via the relevant endpoint

### Amazon Elasticache

- Managed Redis or Memcached
- Caches are in-memory databases with really high performance, low latency
- Reduces load off of databases for read intensive workloads
- Helps make your application stateless
- Involves heavy application code changes
- Architecture
  - App queries elasticache
  - if not available, get from rds and store in elasticache
- User session store
  - user logs in
  - app writes the sessions data into elasticache
  - the user hits another instance of the application
  - the instance retrieves the data from the cache and is already logged in
- Redis
  - Multi AZ with auto-failover
  - Read replicas to scale reads and have high availability
  - data durability
  - backup and restore features
- Memcached
  - Multi-node for partitioning (sharding)
  - No high availability
  - Non persistent
  - No backup and restore
  - Multi-threaded architecture

- Strategies
  - https://aws.amazon.com/caching/best-practices/
  - some data shouldn't be cached because it needs to correct e.g. price at checkouut
  - Lazy Loading / Cache-aside / lazy population
    - application reads from the cache
    - if not available in the cache query the database (cache miss)
    - store the data in the cache and return the data to the application
      - pros
        - only requested data is cached, memory efficient
        - node failures are not fatal
      - cons
        - If there is a cache miss there are 3 network calls (noticeable delay for the request)
        - stale data can be updated in the database and outdated in the cache
  - Write through
    - Add or update cache when database is updated
    - When a write occurs to the database write to the cache at the same time so it is available in the cache for all reads
    - Pros
      - data is never stale
      - Write penalty vs read penalty (users expect a write to take longer than a read)
    - Cons
      - Missing data until it is added/updated, cache doesn't have all the data until it is written
      - To mitigate this you can combine write through with lazy loading
      - There will be a lot data in the cache that might not be necessary
- Cache evictions and Time To Live
  - Three possibilities
    - You delete the item explicitly from the cache
    - The item is evicted because the memory is full and it's not recently used
    - You set a TTL or expiration
- Elasticache replication
  - Cluster mode disabled
    - One primary node, up to 5 replicas
    - asynchronous replication
    - primary node is read/write, the other nodes are read-only
    - one shard, all nodes have all the data
  - Cluster mode enabled
    - Data is partitioned across shards
    - Each shard has a primary and up to 5 replica nodes

## Route 53

### What is DNS?

- Domain Name System
  - Translates human friendly hostnames to IP addresses
- Uses a hierarchical naming structure
- Domain registrar
  - Where you register your domain name
- DNS Records
  - A, CNAME etc.
- Zone File
  - Contains DNS records
- Name Server
  - Resolved DNS queries
- Top Level Domain
  - e.g. .com  .us .gov .org
- Second Level Domain
  - e.g. amazon.com, google.com

### Overview

- Route 53 is a Domain Name System & Domain Registrar
- Ability to check resources health
- 100% availability

### Record Types

- A - Maps hostname to IPv4
- AAAA - Maps a hostname to IPv6
- CNAME - maps a hostname to another hostname
  - The target is a domain name which must have anA or AAAA record
  - Can't create a CNAME record for the top node of a DNS namespace
- NS - Name Servers for the hosted Zone

### Time To Live (TTL)

- Client will cache the result for the duration of the TTL
- If the client requests the same hostname a query will not be sent to the DNS
- The request will be sent straight to the web server rather than to the DNS
- TTL is mandatory for all records except ALIAS records 

### CNAME Vs ALIAS

- AWS Resouces expose an AWS hostname
- CNAME
  - Point a hostname to any other hostname
  - only for non root domain e.g.. something.mydomain.com
  - CNAME is a Canonical Name used for linking a subdomain to the apex domain
  - e.g. my.cool.domain.com => domain.com
- ALIAS
  - Points a hostnae to an AWS resource
  - works for root domain and non root domain
  - Always of type A or AAAA (ipv4/ipv6)
  - Targets
    - Load Balancers
    - CloudFront Distributions
    - API Gateway
    - Elastic beanstalk environments
    - S3 websites
    - VPC interface endpoints
    - etc.

### Routing Policies

- Defines how Route 53 respons to DNS queries
- Does not route traffic but responds to DNS queries
- Simple
  - Route traffic to a single resource
- Weighted
  - Control the % of the requests that go to each specific resource
  - DNS records must have the same name and type
  - Can be associated with health checks
  - used for load balancing between regions
- Latency
  - Redirect to the resouce that has the least latency
  - based on traffic between users and aws regions
  - Can be associated with health checks

### Health Checks

- Provides automated DNS failover
- Can monitor an app, server, aws resource or cloudwatch resource
- Calculated health checks
  - A parent health check monitors multiple child health checks combined with boolean operators

### Failover

- Associate primary DNS record with a health check
- If this is unhealthy divert the DNS request to the secondary IP

### Geolocation

- Routing based on user location

### Geoproximity

- Route traffic ro your resources based on the grographic location of users and resources
- ability to shift morer traffic to resources based on the bias
- To change the size of the geographic region, specify the bias values
- resources can be
  - aws resource
  - non aws resource (e.g. on premises servers)
- Use route 53 traffic flow

### Routing Policies

- Used when routing traffic to multiple resosurces
- Up to 8 healthy recoords are returned for each multi-value-query
- Not a substitute for an ELB (this is client side as it only queries DNS)
- Can be associated with health checks

## Virtual Private Cloud (VPC)

- Private netowrk to deployr your resources within AWS cloud
- Regional Resource
  - One VPC per region
- Subnets
  - Allow you to partition  your network inside your VPC
  - Defined at the availability zone level
  - Public subnet - accessible from the internet
  - Private subnet - private to your VPC
  - Route tables define access to the internet and between subnets
- Internet gateways & NAT gateways
  - public subnets use internet gateways to connect to the internet
  - Private subnets can use NAT gateways to taccess the internet while remaining on your private VPC
  - NAT gateways are aws managed
  - nat instances are self managed
  - This is done by deploying a NAT instance on your public subnet and connect your instance on your private VPC to the NAT on the public subnet

### Network ACL & Security Groups

- NACL
  - Firewall that controls traffic from and to the subnet
  - Uses allow and deny rules
  - Attached at the subnet level
  - Rules only include IP addresses e.g. allow from these IP addresses
- Security Groups
  - A firewall that controls traffic to and from and ENI/EC2 instance
  - Only has allow rules
  - Can include IP addresses or other security groups

### VPC Flow Logs

- Captures information about IP traffic going into your interfaces
- Used for monitoring and troubleshooting connectivity issues

### VPC Peering

- Connect two VPC, privately using AWS' network
- Behave as if they are in the same network
- Must not have overlapping CIDR (IP address range)
- Not transitive - must be established for each VPC e.g. if A connects to B and A connects to C, A is not connected to C

### VPC Endpoints

- Allow you to connect to AWS services using a private network
- Enhanced security and lower latency to access aws services

### Site to Site VPN & Direct Connect

- Site to Site
  - Connect an on-premises VPN to AWS
  - Encrypted over the public internet
- Direct Connect
  - Physical connection between on premisese and AWS
  - Private secure and fasst
  - Goes over a private network
- Both cannot access VPC endpoints

### Typical 3-Teir solution architecture

- ELB deployed on public subnet
- EC2 instances deployed on private subnet
- Amazon RDS (database) stored on a data subnet along with elasticache
  - Elasticache used to store/retrieve session data and cached data

**To Review**: Gateway endpoints and interfaces, route tables, NACL
