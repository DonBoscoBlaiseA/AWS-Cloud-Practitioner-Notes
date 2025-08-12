# AWS Certified Cloud Practitioner Certification Course (CLF-C02) Notes
### July 25

IAM(Identity and Access Management): add users, groups, policies etc (Amazon Linux 2 + t2 micro cpu are free tier)

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
EC2 Spot Instances: Buying spare EC2 capacity for a discount, capacity can be taken back anytime if AWS needs it
EC2 Reserved Instances: Reserve to use specific instance type in specific region for 1 or 3 years. Payment can be Full upfront, Partial upfront or Monthly
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

Data Warehouse: Performs aggregation on million of rows of data, to get analytical reports. Aggregation is grouping data. They can return queries fast, but are only uses once or twice a day to get analytical reports. Data warehouse is built for reporting, trend analysis, and decision-making. It needs to be consume relational data on a regular basis, or else the data will become outdated for reports

Key-value database: NoSQL database, fast but lacks relationship, indexes, aggregation. Value can be represented as dictionary. This can resemble a tabular database, but not every row will have every column
![Key-value DB](image-3.png)

Document Store: A sub type of Key-value stores, but stores docuemnts as primary. Could be XML, but mostly it is JSON or JSON-like
DynamoDB is AWSs serverless NoSQL key-value and document database. Has auto scaling, cost effective and fast (Used when we want a massively scalable database)
DocumentDB: NoSQL document database (Used when you want to use MongoDB like database)
Amazon Keyspaces: NoSQL database (When you want to use Apache Cassandra database)
