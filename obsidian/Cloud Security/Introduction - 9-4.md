# Introduction to Cloud Computing
History - a very long history of people wanting cloud computing
John McCarthy proposed 'service bureau' model in 1961
JCR Licklider (contributed ideas to ARPANET) published a memo on intergalactic computer network in 1963
AWS - SQS, S3, EC2, CLI only initially
Carr - equats rise of cloud computing in the info age to buildout of the electrical grid in the industrial era.
	Before the electrical grid, building owners were expected to build their own electricity systems to power
NIST Defintion of Cloud Computing
	"Cloud computing is a model for enabling ubiquitous, convenient, on-demand network access to a shared pool of configurable computing resources (e.g., networks, servers, storage, applications, and services) that can be rapidly provisioned and released with minimal management effort or service provider interaction"

Essential Characteristics
1. On-Demand Self Service - cloud computing customer can provision services without assistance
2. Broad network access - services delivered over internet
3. Resource Pooling - multi tenant: Multiple cloud customers can share the same underlying infrastructure
4. Rapid Elasticity - scale up and down rapidly and easily to add to or decrease compute capacity
5. Measured service - service is sold based on compute time, bits transferred, usage

Service Models
SaaS: The capability provided to the consumer is to use the provider's applications running on a cloud infrastrucutre
	Zoom, Cisco Webex
PaaS - The capability enables developers to build, run, and operate applications entirely in the cloud, without directly provisioning infrastructure components.
	Ex: Apprenda, Heroku
IaaS: The capability provided to the consumer is to provison, processing, storage, networks and other fundamental computing resources where the consumer is able to delploty and run arbitrary software
	Ex: AWS, Azure

CLoud Deployment Models
-Private - operated and maintained by an org for internal use
	Ex: VMware, Openstack
-Public: Offered on the internet and available to multiple consumers
	Ex: AWS, Azure
-Hybrid - Combines Private and Public so that compute workload can "burst" to the public cloud as needed. Workloads can run on either the private or public cloud
-Community - built and architected for a specific group and use case. Ex: Google Apps for Ed, AWS for Federal

Multi Tenancy in Public vs Private Clouds
Private: Company A uses Company A's private Cloud
Public: Company B, C, D, E shares a public cloud

Cloud Computing Actors: The stakeholders, who is involved
-Cloud Consumer: A person or organization that maintains a business relationship with and uses service from cloud providers
-Cloud Proviers: responsbile for making the service available
-Cloud Auditor: A party that can conduct independent assessment of cloud services, information system operations, performance and security of the cloud implementation
-Cloud Broker - an entity that manages the use, performance and delivery of cloud services and negotiates relationships between Cloud Providers and Cloud Consumers
-Cloud Carrier - an intermediary that provides connectivity and transport of cloud services from Cloud Providers to Cloud Consumers

# Aws
Amazon is only one of several cloud providers
There is no common standard (yet)

What is AWS?
Provides a number of different services:
EC2 - virtual machines for running custom software, e.g. servers
S3 - Simple key-value stroe, accessible as a web service e.g. object storage
DynamoDB - Distributed "NoSQL" database, one of several database types offered
EBS - block-storage service

Shared Responsibility Model
Customer Responsible for Security "in the Cloud"
	Customer Data
	Platform, Applications, Identity & Access Management
	Operating System, Network & Firewall Configs
	Client-Side Data
	Encryption & Data
	Integrity Authentication
	Server-side Encryption
	Network Traffic Protection

AWS Responsible for Security "of" the cloud
	Compute, storage, database, networking
	Regions
	Availability Zones
	Edge Locations

AWS Access - 1
Web Site and management console
	Username and password
	X.509 Certificates
		Use Case: typically for IoT device, small device that needs to talk to the cloud, but will never touch
	EC2 Key Pairs - connect using SSH
	Access Keys - using rest APIs


Management console used to control the AWS services: start/stop instances, create

Managing your cloud:
Everything can be an API call
Everything has an ARN - Amazon Resource Name
	Ex: arn:aws:s3:::cdodge-nyu-demo
	::: - empty spaces where other metadata might exist
		Ex: Region the resource is part of
Each API call can be logged - CloudTrail service logs the API calls as events
	you can also control permissions on a API call basis

Pets Vs Fish
	It treats the server as a Pet. A lot of care and time is spent to ensure the server is running.
	versus as a cloud environment, it treats the servers as a school of fish. Even if a single fish dies, the "school" will survive.
	If your servers have names - they are Pets

Regions and Availability Zones
Infrastructure is launched in a specific region
	Currently 26 regions exist
	8 more announced
	Most commonly used - US East(Northern Virginia), US West (Northern California), EU (Ireland)
	Important for reducing latency to customers
Instances can be assigned to availability zones
	84 availability zones
	24 more announced
	Several avaiablity zones within each region


Amazon S3 - Simple Storage System
Rougly an internet file system
Stores large objects (=values) that may have access permissions
	Used in cloud backup services like Jungle Disk, streaming video like Netflix
	Used to distribute software packages
	Used Internally by amazon to store Virtual machines
“Up to 99.99999999% durability, 99.99% availability” (“ten nines” and “four nines”)
Amazon states that “if you store 10,000,000 objects with Amazon S3, you can
on average expect to incur a loss of a single object once every 10,000 years.”


S3 Key concepts
S3 consists of:
	buckets of objects, think of these as volumes in a filesystem. Names must be globally unique
		If you create luke-demo, nothing else can take that
	objects - named items stored in an s3 bucket
Names within a bucket must uniquely identify a single object
	i.e., keys must be unique
Security Risk
	Bucket squatting: a user deletes a bucket that is used for some workload, then another different user comes along and creates a bucket with that same name. They can now access any data written to that bucket.

S3 Keys and Objects
What can we use as keys?
	keys can be any strings
What can we use as objects
	Objects can be from 1 byte to 5 TB, any format
	Number of objects is 'unlimited'
	Default limit of 100 buckets per account

S3: Access Control
Access Control Lists - Not Recommended, legacy, users are recommended to disable ACLs except in unusual circumstances where you need to control access for each object
	Enable you to manage access to buckets and objects
	Each bucket and object has an ACL attached to it as a subresource.

Bucket policies and user policies
	Two access policy options available for granting permission to your Amazon S3 resources.

Condition Keys
	The access policy language enables you to specify conditions
	In the Condition element, you build expressions in which you use Boolean operators to match your condition against values in the request
		AWS-wide condition keys
		Amazon S3-specific conditon keys

When to use ACL:
	You need to control acces for each object individually - not recommended, you should disable and rely on policies
When to use bucket policy:
	 If an AWS account that owns a bucket wants to grant permission to users
	in its account, it can use either
	You want to manage cross-account permissions for all Amazon S3
	permissions. Both bucket and user policies support granting permission for
	all Amazon S3 operations

When to use a user policy
	In general, you can use either a user policy or a bucket policy to manage
permissions. 
	It is a good practice to enforce s3 resource based policies when the bucket
requires more security consideration and not only use principal based
policies.

S3 Access Points
	Pros: each ap has its own policy, a single account can have thousands of ap, solves the problem of bucket policies getting too big
	Cons: can be hard to discover the right access point for users/clients, can be hard to track who exactly has access

S3 Access Grants
	Solves for the problem of connecting user identities to policies, e.g., identities from Active Directory
	Can only grant "read" or "Write" access
	But if you're rolling your own audit tool, this is yet another thing to audit


Amazon Machine Images
When launching an instance, what software is installed?
Taken from Amazon Machine Image
	Selected when you launch an instance
	Machine image that contains the OS, applications, and other data
How do I get an AMI?
	Amazon provides several generic ones, e.g, Amazon Linux, Fedora Core, Windows Server
	You can make your own
		Security Risk - beware of leaving secrets on a public image

Instance Types
On-demand instances
Reserved instances
	One-time reservation fee to purchase for 1 or 3 years
	Usage still billed by the hour, but at a considerable discount
Spot Instances
	Spot market: can bid for available capacity
	Instance continues until terminated or price rises above bid

Elastic Block Store
Persistent Storage
	Unlike the local instance store, data stored in EBS is not lost when an instance fails or is terminated
Should I use the instance store or EBS?
	typically instance store is used for temporary data
Beware of regions 
	Can't access EBS volumes in other regions
