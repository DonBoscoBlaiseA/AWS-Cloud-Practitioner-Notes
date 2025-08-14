# AWS Certified Cloud Practitioner Certification Course (CLF-C02) Notes
### July 25

IAM(Identity and Access Management): add users, groups, policies etc (Amazon Linux 2 + t2 micro cpu are free tier)
Amazon Route 53: DNS

Geographical Regions: US
Regions: US-EAST, US-WEST
Availability Zones(AZ): US-EAST-a, US-EAST-b, US-EAST-c (atleast 3 AZ for good availability)

Elastic Beanstalk: PaaS (to just host code, if set to 'highly available', automatically adds load balancing, makes more than 3 instances)
EC2: IaaS (Need to manually set up load balancers etc)

Multi-AZ: runs data in another AZ to make it highly available
Scale Up: Increase resource of a server
Scale Out: Add more servers

AWS API is HTTP/S API, use postman to send HTTPS request and talk with it
AWS API with Authorization Header and Action as payload

Cloud9 to create Virtual Env

IaC: AWS CloudFormation(CFN), AWS Cloud Development Kit(CDK)
CloudFormation stack is a template (group of resources inside)
CloudFormation - declarative, Explicit, more verbose, written in json or yaml
Cloud Development Kit - imperative, Implicit, less verbose, can be written in programming lang like py, java

Difference between CDK and SDK, is that CDK is idempotent of your infrastructure
idempotent: You can apply the same operation multiple times, and the result will be the same after the first time
CDK generates CloudFormation 'templates' (state aware) | SDK sends direct 'API calls' to AWS
CDK has state awareness
Eg: If you define an S3 bucket in CDK, and it's already created, nothing will happen on the next deploy unless you
change something
CDK uses CFN underneath
CloudFront: It is AWS Content Delivery Network (CDN) service
Caches copies of content in multiple edge locations around the world (for lower latency and faster delivery to user)

Shared Responsibility Model:
AWS: Hardware / Global infrastructure like Regions, AZ, Edge Loc, Physical Security
     Software, i.e., Compute, Storage, Database, Networking
Customer: Configuration of managed services or third party services like Platforms(OS), Applications(Prog Lang), IAM
Configuration of Virtual infrastructure and Systems like Operating Sys, Network, Firewall
Security Configuration of Data: Client-Side Data Encryption(moving from s3 to local),
Server-Side Encryption(turn on encryption on s3), Networking Traffic Protection(turn on VPC flowlogs or GuardDuty),
Customer Data(Using Amazon Macie)
Customer is responsible for Security In the Cloud, AWS is responsible for Security Of the Cloud
![Diff between IaaS, PaaS, SaaS](image-1.png)

IaaS: EC2 BareMetal Instance (Customer with Hardware access, Host os config, Hypervisor, AWS with Phy Machine)
EC2 Dedicated Instance (Customer with Guest OS config, Container Runtime, AWS with Hypervisor, Phy Machine)
ECS (Customer with ability to run container apps like docker, AWS with OS, Hypervisor, Container Runtime)
*Runtime: The environment that runs your application/code

PaaS: Elastic Beanstalk (Customer can upload code, manage config of env, AWS manages servers, OS, Networking, Storage, Security)

SaaS: Amazon WorkDocs (Customer manages Content of doc, Config of sharing access controls, AWS manages OS, Servers, Networks, Storage, Security)

FaaS: AWS Lambda (Customer just uploads code, AWS manages Deployment, Container Runtime, Networking, Storage, Security)

Apart from this, there is AWS Fargate (Serverless Container as a Service)
![Responsiblity of IaaS, PaaS, SaaS](image-2.png)
![Responsiblity of VM, Container, Function](image.png)

EC2 allows you to launch VM (highly configurable: cpu, ram, network bandwidth, OS(AMI), attach multiple virtual hard drives(EBS))
*AMI: Amazon Machine Image, EBS: Elastic Block Store
Lambda, S3, RDS, DynamoDB etc., also use EC2 as underlying backbone
AWS Network is the backbone for global infrastructure, EC2 is backbone for services

VM: AWS LightSail is a managed virtual server service, easy version of EC2 VM

Containers: Virtualizing an OS, to run multiple workloads on a single OS instance, used in microservice arch
ECS is a container orchestration service. Launches a cluster of servers on EC2 instances with Docker installed (When you need Docker as a Service, or need to run containers)
ECR(Elastic Container Registry) is a repository for container images. To launch a container, you need an image(a saved copy). Repository is just a storage with version control.
ECS Fargate is a serverless container orchestration service. Same as ECS except you pay-on-demand per running container (with ECS, you have to keep a EC2 server running even if you have no containers running)
EKS(Elastic Kubernetes Service) is a fully managed Kubernetes Service. Kubernetes is an open source orchestration software. Used when you need to run Kubernetes as a Service
*Instance: A virtual env instance

Serverless: AWS Lambda: serverless functions service. Can run code without provisioning or managing servers. Charged based on runtime of the function rounded to the nearest 100ms

ECS Creation:
i. Create Cluster (Cluster is a container playground, where you can add tasks, services etc. Its like a template)
ii. Make a task definition (blueprint  that tells specific containers to run, how much CPU/memory each gets)
iii. Add a task or service in the created cluster, which uses the created task definition.
Task: Run this program once
Service: Keep this program running forever, with X copies, and restart it if it crashes
Security group: virtual firewall for each cluster

The Nitro System: Combo of Dedicated Hardware and Lightweight Hypervisor
Bottlerocket: linux-based OS by AWS, for running containers on VM or bare metal hosts
HPC (High Performance Computing): Many servers with fast connection between them to increase computing capacity
HPC is for ML or very complicate scientific computing stuff
ParallelCluster: AWS-supported open source cluster management tool, for HPC clusters on AWS

Edge Computing: When you push your workloads outside of your network to run close to destination. eg: pushing computing to run on external servers, phone, iot not within your network in premise
Hybrid Computing: When you run workloads on premise and in AWS VPC
AWS S3 Outposts: Physical rack of servers that we can get and put in our own datacenter. Allows access to AWS services like EC2 within premise
AWS Wavelength: Allows to build and launch application in telecom datacenter. Ultra low latency, since they are on 5G network, closest to end user
VMWare Cloud on AWS: Allows you to manage on-premise VMs which are using VMWare for virtualization, by making them into EC2 instances
AWS Local Zones: Edge datacenters outside of AWS region. Allows compute closer to destination. When you need fast computing, storage and databases in populated areas outside AWS Regions

Cost Mangement: How to save money?
Capacity Management: How do we meet the demand of traffic and usages by adding or upgrading servers
EC2 Spot Instances (up to 90% off): Buying spare EC2 capacity for a discount, capacity can be taken back anytime if AWS needs it
EC2 Reserved Instances (up to 75% off): Reserve to use specific instance type in specific region for 1 or 3 years. Payment can be Full upfront, Partial upfront or Monthly
EC2 Savings Plan: Commit to spend a fixed amount of $ per hour on compute for 1 or 3 years. Can change region, instance (flexible)
AWS Batch: To run batch computing jobs that dont require user interaction. Can utilize Spot instances or On-Demand
AWS Compute Optimizer: ML is used to suggest how to save money and improve performance, by analyzing previous usage history
EC2 Autoscaling Groups(ASG): Automatically adds or removes EC2 capacity as per demand
ELB and EB also comes under Cost and Capacity Management

Storage: Types are Block, File, Object
(Block) EBS - Can be used by only one EC2 Instance. Manually increase storage size. Stored in only one AZ. Supports only a single write volume(One user can attach it one instance and write, others cant write at the same time). Protocol: FC, iSCSI
(File) EFS(Elatisc File System) - Can be used my many EC2 Instances at the same time. Automatically grows/shrinks as you add or remove files. Data is replicated across multiple AZ. Allows multiple reads, writes, but locks the file (Allows multiple users to read and write, but while one is writing, lock is put, so that others cant write at the same time). Protocl: SMB, NFS
(Object) S3 - Scales with no limit. Multiple read, write with no locks. Protocl: HTTP/S, API

S3 Object: They are like files
Key: name of the object, Value: data as sequence of bytes, VersionID: version of object, Metadata: additional information attached to the object
Each S3 object can be between 0 bytes and 5 terrabytes
S3 Bucket holds many objects. Each bucket must have unique name

S3 Storage Classes: Cheap to Cheapest
S3 standard(replicated across 3 AZ), S3 Intelligent Tiering(uses ml to choose best storage class), S3 Standard-IA (Infrequent Access) (Cheaper if you access files less than once a month, additional retreival fee is applied), S3 One-Zone-IA (stored in only one AZ), S3 Glacier (for long term cold storage, retrieval can take minutes to hours), S3 Glacier Deep Archive (takes 12 hours to retrieve data)
S3 Outposts dont fit in this

AWS Snow Family: It is a set of physical devices used for moving large amounts of data in and out of AWS, as well as for edge computing. It includes devices like Snowcone, Snowball, and Snowmobile(100 PB of storage per device). Date is delivered to S3

Storage Services: S3(serverles object storage, auto scaling, unlimited storage), S3 Glacier(archiving and long-term backup, uses prev gen HDD to get that low cost), EBS(persistant cloud storage, its a virtual hard drive in the cloud you attach to EC2 instances), EFS(cloud-native NFS file system, mount to multiple EC2 at same time), Storage Gateway(hybrid cloud storage, which extends on-premise storage to the cloud), AWS Snow Family(storage devices used to physically migrate large amounts of data), AWS Backup(managed backup service, centralize and automate backup of data across multiple AWS services like EC2, RDS, EBS), CloudEndure Disaster Recovery(continously replicates your machine into low cost staging area for recovery), Amazon FSx(feature rich and highly-performant file system)

S3 Config: After turning on encryption of objects in S3 Bucket. If any hacker steals hard drive from AWS, they cant decrypt our files. Can set lifecycle rule like "move all objects to glacier after 30 days". Can have upto 100 Buckets per AWS account(Can be seen in Service Quotas)

EBS is under EC2 service. Can be added when creating EC2
EFS can be set to regional or one-Zone. Can be added when creating EC2

Data Warehouse: Performs aggregation on million of rows of data, to get analytical reports. Aggregation is grouping data. They are "HOT, meaning they can return queries fast, but are only uses once or twice a day to get analytical reports. Data warehouse is built for reporting, trend analysis, and decision-making. It needs to be consume relational data on a regular basis, or else the data will become outdated for reports

Key-value database: NoSQL database, fast but lacks relationship, indexes, aggregation. Value can be represented as dictionary. This can resemble a tabular database, but not every row will have every column
![Key-value DB](image-3.png)

Document Store: A sub type of Key-value stores, but stores docuemnts as primary. Could be XML, but mostly it is JSON or JSON-like

Non Relational Database Services in AWS:
DynamoDB is AWSs serverless NoSQL key-value and document database. Has auto scaling, cost effective and fast (Used when we want a massively scalable database)
DocumentDB: NoSQL document database (Used when you want to use MongoDB like database)
Amazon Keyspaces: NoSQL database (When you want to use Apache Cassandra database)

Relational Database Services in AWS:
RDS: Relational Database Service. Supports several SQL engines like MySQL, MariaDB(fork of MySQL), Potgres, Oracle, MsSQL, Aurora
Aurora: Fully managed database of either MySQL or Postgres(When you need highly available, scalable, durable, secure relational db for MySQL or Postgres)
Aurora Serverless: Serverless on-demand version of Aurora(autoscaling, can pause when not in use and save money). Startup latency when resuming from pause
RDS on VMware: When you want to deploy RDS supported SQL engines on-premise. On-premise datacenter must be using VMware for server virtualization

Redshift: Data Warehouse in AWS (petabyte-size data warehouse)
ElasticCache: Managed database of the in-memory and caching databases like Redis or Memcached (Improve performance of application by adding a caching layer in-front of web-server or database)
Neptune: Managed graph database. Data is represented as interconnected nodes (When you need to understand connection between data)
Amazon Timestream: Time series database. Eg. IOT devices send time sensitive information
Amazon Quantum Ledger Database: Ledger database (Transparent, immutable, cryptographically variable logs)(Used for financial information that can be trusted)
DMS(Database Migration Service): Move from on-premise to AWS, SQL to NoSQL, etc

Cloud Native Networking Services:
![Cloud Network](image-4.png)

Enterprise/Hybrid Networking:
![On-Premise Network](image-5.png)

VPC and Subnets:
![VPC and Subnets](image-6.png)

NACL(Netowrk Access Control List) and Security Groups:
![NACL and Security Groups](image-7.png)

Network Config: Create a VPC with CIDR of /16, then create a subnet inside it with CIDR /24, now there is only one route in the subnet i.e to local, so create internet gateway and add it in route table with CIDR /0, now you can create an EC2 with the created VPC and subnet. You can add security groups in EC2 to allow SSH, TCP etc., You can also use NACL in VPC to block particular ip or allow connections etc., in security groups and NACL, type means to block SSH or ALL TCP connections coming from specific ip. Set rule number in NACL as multiples of 10
*Security group can allow connections to an instance, NACL allow or deny connections (block ip etc.,)

CloudFront Config: Can set origin(source of content for CDN. Eg. S3 Bucket), Can allow or block Regions, Can create invalidation(If a content in cache is stale, you can delete by entering path of it (can use wildcards '*'))

Ec2 Instance Families: Different combinations of CPU, memory, storage and network capacity. General purpose: T2, T3, Mac(Mac server), A1 etc., Compute optimized(for Scientific modelling, dedicated gaming servers, high performance processor tasks): C5, C4 etc., Memory optimized(for in-memory cache, in-memory database, real time analytics): R4, R5, X1 etc., Accelerated optimized(for ML, Hardware accelerator or co-processors): P2, P3, P4, F1 etc., Storage optimized(data warehousing, high read and write): I3, D2, D3 etc.,

Ec2 Instance Types(meaning the size and instance family): Eg. nano, micro, small, medium, large, xlarge, 2xlarge etc.,

Tenants Types:
Dedicated Hosts: Single tenant EC2 instances, physical server isolation, billed per host, can view sockets, cores, host ID(this allows to Bring-Your-Own-License(BYOL), where these informations are needed). Eg. Microsoft SQL Server needs you to enter the amount of cores etc., so we can use dedicated hosts to use the information
*It provides you the hardware transparency needed for softwares

Dedicated Instance: A single slot in a server which is shares across other customers. The slot will always be the same

Default: Your instance lives in a slot until reboot, so it changes everytime
![Tenancy](image-8.png)

EC2 Instance Creation Config:
AWS Elastic Inference (EI) is a service that allows you to attach low-cost GPU-powered acceleration to Amazon EC2
Add SSM policy to IAM role and add it to EC2 IAM role. This allows you to securely connect to EC2 without storing SSH keys
User data: You can add commands to install and run softwares. Eg. install apache (named as httpd)
![apache install](image-9.png)
Storage: Turn on encryption
Tag: You can add a name tag to identify instance
Security group: You can use old or add new security group(Security group name cant be changed later). Add HTTP port 80 to access Apache server (source is set to anywhere, inorder to be access it from anywhere)
Launch and you can create key pair and download it(.pem file)
Search the public IPV4 address of the instance on browser, will return apache webpage
Can use CloudShell and upload the .pem private key, chmod it to 400(read only), and use it to ssh into the created EC2 instance(can use public ip or dns to ssh)
By default, we are ec2-user (name might vary for different AMI)
Apache index.html file is in /var/www/html
*Instead of ssh, you can use the SSM by clicking connect on instance. Will be logged in as ssm-user. Need to switch to ec2-user

Elastic IP address: static ip
Public IPV4 of EC2 instance wont change if it is rebooted, but will change if the instance is stopped and started
Then we can associate the elastic ip address to the EC2 instance. Now ip wont change even if stopped and started

Create AMI and launch templates: Creating an AMI is like making a copy of an EC2 instance(with its OS, config, code, data). Template is used to save EC2 launch settings (AMI, instance type, key pair, security groups, EBS settings, tags, etc.) in one reusable template.
*We can create an AMI of the EC2 with apache server and use that as the AMI for a template. Template allows us to quickly create an instance, instead of creating one from scratch. Thus, the apache server EC2 instance is saved by making it into an AMI. Future changes in the instance can be saved as AMI and updated as new version of the template

Autoscaling Group(ASG) to EC2: Add 3 AZ for availability and you can set it to scale based on different parameters like CPU utilization at 50%

Load Balancer(ALB): It will check the target group(list of instances). Create target group and set health check. It will check the website and if it doesnt get 200 OK signal, it will send traffic to another healthy instance. The number of times it can fail to consider the site unhealthy and the number of times it can succeed to consider the site healthy can be set. Add Load Balancer to ASG. Use DNS of load balancer to access the website instead of ip of instance. Add load balancer dns to route 53, to use load balancer when accessing website
*ASG always used together with ALB, because ALB acts as the entry point

Connecting ASG and ALB:
ASG working: ASG uses template to create the number of EC2 instances we want. If min number of instances is 1, max number of instances is 3 and desired number of instances is 2. It will launch 2 instances at the start. Each instance will have different IPV4. ASG can scale based on parameters set like CPU utilization at 50%. By default, ASG does "EC2" health check(checks if EC2 instance is running). If one instance fails, ASG terminates and creates a new one with new IPV4

ALB working: Since every instance has different IPV4, we need a common point, so we use DNS of ALB(also distributes traffic across instances). A target group should be created with health check parameters. Select the instances for target group(Or you can also add the ALB inside ASG). Add the target group to ALB. You can enable ELB health check in the ASG after adding ALB. Point route 53 to ALB dns

EC2 Pricing: On-Demand: Default EC2 pricing. Mostly charged by the second(but cost is shown as hourly), rarely by the hour. Used for short term, unpredictable experiments. After you know how much resourse it takes, you can buy reserved

Reserved Instances(RI): Used when you have a steady or predictable usage. Reduced pricing is based on Term, Class, RI Attributes and Payment options
Term: Commit to 1 or 3 year contract, when term expires, instance will be turn back to on-demand with no interruptions
Class: Standard(up to 75% lower price compared to on-demand, can modify RI attributes). Convertible(up to 54% lower price comapred to on-demand, can exchange RI based on RI attributes)
Payment options: All upfront, partial upfront, no upfront
*RIs can be shared between multiple accounts withing AWS organization. Unused RIs can be sold in Reserved Instance Marketplace

RI Attributes or Instance Attributes:
![RI Attributes](image-10.png)

Regional and Zonal RI: Scope of the RI
![Regional and Zonal RI](image-11.png)

RI Limits: There is a limit to the number of RI instances you can purchase per month
20 Regional Reserved Instances per Region, 20 Zonal Reserved Instances per AZ
Regional Limits: You cannot exceed the running on-demand instance limit by purchasing Regional Reserved Instances. The limit is by 20
Zonal Limits: You can exceed the running on-demand instance limit by purchasing Zonal Reserved Instances. Eg. If you have 20 running on-demand instances and you purchase 20 Zonal Reserved Instances for an AZ, you can purchase 20 more on-demand instances for that AZ with same specification of Zonal Reserved Instances

Difference between Regional and Zonal: Purchasing Regional RI just means, you know the capacity you need in the region, so you get a discount. If you dont know the capacity you need, you can go on-demand and get no discount. Regional RI doesnt allot a slot for you in the region(Zonal RI allots slot for you in one AZ). You dont get guarantee of slot in the region, you just get discount because you know the capacity you need. Whereas, Zonal RI guarantees a slot in the specific AZ, so you can run more instances in that slot, even if the limit of 20 is reached

Capacity Reservation: Since EC2 instances are backed by hardware, they can sometimes run out of capacity to launch your new EC2 instance(only can have finite amount of servers)
By using capacity reservation, you can reserve a capacity ahead of time(AWS will remove your reserved capacity from shared instance pool and put your name on it). You will be charged on-demand instance price even if no instances are running in the reserved space

Standard RI vs Convertible RI:
![Standard vs Convertible RI](image-12.png)

RI Marketplace:
![RI Marketplace](image-13.png)

Dedicated Instances: Multi-Tenant(shared hardware with software isolation) and Single Tenant(physical isolation. Can be used for BYOL softwares)
![Dedicated Instances](image-14.png)

AWS Savings Plan: Similar discounts as RI but simplified purchasing process. There are 3 types of savings plan: Compute savings plan, EC2 instance savings plan, sagemaker savings plan(simpler than RI. i.e. no need to choose between standard/convertible, regional/zonal etc.)
![Savings Plan](image-15.png)
*Sagemaker is EC2 for ML

Zero Trust Model: Trust no one, verify everything. Identity is the primary security perimeter(first line of defense) in zero trust model. Old way used Network Centric: Focused on firewalls and VPNs to secure data. Eg. data cant be accessed from locations which are outside of premise. New way uses Identity Centric: Where no matter where you are(on premise or outside premise), needs to authenticate inorder to access data(no trust given, even if on premise). Identity Centric way augments Network centric way

Zero Trust on AWS: Set up by using Identity controls, i.e. IAM. Using IAM, you can restrict users based on ip, time, region, MFA present and also set boundaries. Eventhough AWS has these options to set zero trust for users, it is manual and not intelligent ready-to-use solution. Collection of AWS services can be used to make an intelligent-ish security. Eg. AWS Cloudtrail which tracks API calls -> Amazon GuardDuty which detects suspicious activity based on cloudtrail and other logs -> Amazon Detective which can analyze and identity security issues from GuardDuty (very hard to setup)\

Third Party Solution for Zero Trust:
![Third Party Zero Trust](image-16.png)

Directory Service: It maps the names of network resources to their network address
![Directory](image-17.png)

Active Directory: Access multiple infrastructure devices with one identity(each user has one identity)
![Active Directory](image-19.png)

Identity Provider IdP:
![Identity Providers](image-18.png)

Single-Sign-On(SSO): Authentication scheme that allows a user to login to different systems with single ID and password. Login for SSO is seamless, i.e. Once a user is logged into the primary directory, they are not asked to login multiple times in different sites. Eg. If you are logged into the primary directory, you can log in to many services like aws, slack, google workspace, salesforce, devices etc. with logging into each of them

Its how a organizations computer asks user ID and password when logging into the computer and uses the same information to log you in to multiple independent services like aws, google workspace etc. The Active Directory has your user ID and the list of services you have access to like aws, google workspace etc

LDAP(lightweight Directory Access Protocol): It enables same-sign on, which allows users to use single ID and password but they have enter it every time they want to login
![LDAP](image-20.png)

AWS X-Ray, AWS Partner Network (APN), AWS Global Acclerator, AWS Cost Explorer, AWS Budgets, AWS License Manager, AWS Launch Wizard (Enterprise applications that may require complex configurations and architecture), AWS Marketplace (Purchasing and deploying third-party software solutions on AWS, Streamlined billing and payment through existing AWS accounts, Curated catalog of enterprise-class software)

Policy: Rules for users
Roles: Rules for services. Eg. EC2 can access certain S3 bucket