# Contents
- [Getting started with AWS](https://github.com/pawanit17/aws-solution-architect-associate/blob/main/README.md#getting-started-with-aws)
- [IAM & CLI](https://github.com/pawanit17/aws-solution-architect-associate/blob/main/README.md#iam)
- [EC2 Fundamentals](https://github.com/pawanit17/aws-solution-architect-associate/blob/main/README.md#ec2-fundamentals)

# 3 Getting started with AWS
- Regions - ap-southeast-2 etc. It is a cluster of data centers.
  - How to chose a region?
    - Compilance. Latency. Available services, Pricing are to be taken into consideration to arrive at a decision.
    - Each region would have multiple Availability Zones, usually 3.
- Availability Zone
  - ap-southeast-2a / ap-southeast-2b / ap-southeast-2c
  - AZ is one or more discrete data centers.
- Edge Locations ( Points of Presence )
  - Content delivery
- Global services apply to all regions. Ex - Route 53, IAM. On the othe hand, EC2 is not.

# 4 IAM
- Identity and Access Management
- Users are people in org. Groups can only contain Users, not other groups.
- User need not belong to any group or can belong to multiple groups.
- ![image](https://user-images.githubusercontent.com/42272776/176990782-7b2dd9c7-ae65-4a83-a5b5-76d69aebd552.png)
- Permissions are defined via JSON Policy documents.
- When creating users, we optionally add them to groups and assign policies at group level.
- Managed policies are the ones that come OOTB.
- ![image](https://user-images.githubusercontent.com/42272776/176990987-6fd78769-ab71-40c2-b939-6f37af691a67.png)
- IAM has password policy that is useful. Also MFA = Password + Security Device.
  - Virtual MFA device { Google Authenticator, Authy }.
  - Universal 2nd Factor Security Key - physical device.
  - Hardware Keyfob
- We can connect to AWS by console, cli and sdk. Cli is built on top of python sdk.
- Note that Cloud Shell is just a terminal within the AWS Console. So it is similar to Terminat + CLI.
- ```aws configure```
- ```aws iam list-users```
- IAM Roles
  - Used by services, not by people.
  - For this, we create permissions and assign them to the AWS Services with IAM Roles.
  - Ex: EC2 Instance Roles, Lambda Function Roles and Roles for Cloud Formation.
- IAM Security Tools - these help in auditing
  - IAM Credentials Report (account level) - lists all the users in the AWS Account
  - IAM Access Advisor (user level) - When a service is last used. This helps in evaluating the removal of that service for the user.

# 5 EC2 Fundamentals
- Budgets let you set custom budgets and AWS would alert you when the threshold is exceeded.
- EC2 is IAS.
- EC2 User Data
  - Bootstrap of our instances.
  - Run only once at the instance first start.
  - Ex: Install software, updates, download common packages from internet.
  - Scripts would be run as admin user.
  - EC2 User Data is used to bootstrap your EC2 instances using a bash script. This script can contain commands such as installing software/packages, download files from the Internet, or anything you want.
- When the instance starts, stops and starts again, the Public IPV4 changes. The private IP address does not change.
- SSH ![image](https://user-images.githubusercontent.com/42272776/177472860-84570285-e2ab-45e4-808e-e2618b30ac16.png)
- SSH lets users log onto the remote machines on AWS. For SSH to work, the port 22 has to be enabled in the security group.
- ```chmod 0400 keys.pem```
- ```ssh -i keys.pem ec2-user@publicip```
- While ec2-user is the default userid, when an EC2 is built using an AMI, the userid can be configured differently.
- m5.2xlarge
  - m is instance class
  - 5 is the generation
  - 2xlarge is size within instance class
- General purpose ( webservers )
- Compute Optimized C-series ( gaming, machine level, batch processing )
- Memory optimized R-Series ( large datasets in memory, high performance rdbms, distributed caches, BigData )
- Storage Optimized ( OLTP, DBs, Cache, File systems )
  - Storage Optimized EC2 instances are great for workloads requiring high, sequential read/write access to large data sets on local storage. 
- Security Groups - Firewalls
  - Security Groups can be attached to multiple EC2 instances within the same AWS Region/VPC.
  - Determined how traffic is allowed into our EC2 instances.
  - ![image](https://user-images.githubusercontent.com/42272776/177498789-e19213a8-19e2-482f-a1f5-5d2a9d01ee0d.png)
  - Only contain allow rules.
  - Can reference IP or other security groups.
  - They regulate
    - Access to ports
    - Authorized ip ranges - V4 and V6
    - Controls Inbound and Outbound network.
    - ![image](https://user-images.githubusercontent.com/42272776/177497869-c2aa59a4-0cab-467f-a2bd-e13bb8e00f4c.png)
    - Security groups can be attached to multiple instances.
    - Locked down to region and vpc combination.
    - If requests are blocked at SG, the EC2 isntances won't even be aware of them.
    - Good to maintain one separate security group for SSH access.
    - If your application is not accessible (time out), then it's a security group configuration issue.
    - If your application gives a connection refused error, then its an application error or the application is not running/listening on that port.
    - By default, all inbound traffic is blocked and all outgoing traffice is allowed.
    - ![image](https://user-images.githubusercontent.com/42272776/177503412-fea11b6c-c895-4786-b420-ea8be9527654.png)
    - ![image](https://user-images.githubusercontent.com/42272776/177503614-eee043f1-4e17-477c-8c74-5ee15b256466.png)
- Instant Connect is a browser based terminal that lets us connect to our EC2 servers. It comes with AWS CLI installed.
- Never enter your access key and ids onto an EC2 terminal. Instead we should provide IAM Roles to EC2 instances - this is how AWS credentials are passed.
- On Demand Instances - short workload.
  - Short term and uninterrupted workloads where we cant predict the application behavior. 
- Reserved ( 1 & 3 years )
  - Databases.
- Savings plan (1 & 3) years commitment to an amount of usage.
- Spot Instances - may lose instances, but cheap.
  - This is like bids. If the current spot instance price < what we are paying, the instance stays with us. Otherwise, we get a 2 min grace period.
  -  Spot request and Spot instances.
  -  Spot Fleets = Set of Spot instances + (Optional) On-Demand instances.
- Dedicated Hosts - book entire physical server, control instance placement.
  - Bring your own license. Very costly. 
- Dedicated Instances - no other customer will share your hardware.
- Capacity Reservations - reserve capacity in AZ.
- ![image](https://user-images.githubusercontent.com/42272776/177523929-1b2e4fcc-cced-4f49-9df2-ba5116532e58.png)
- ![image](https://user-images.githubusercontent.com/42272776/177523976-efb8774b-91d6-49a4-84f3-1bea33a2aaf7.png)
- Spot Instances are good for short workloads and this is the cheapest EC2 Purchasing Option. But, they are less reliable because you can lose your EC2 instance.

# 6 EC2 - Solutions Architect Associate Level
- Private vs Public IP
  - ![image](https://user-images.githubusercontent.com/42272776/177610785-e6d00d4d-846f-49df-b007-58eff05cc7a0.png)
  - Elastic IP
    - Upon restart of EC2 instance, its public address may change.
    - If you need to have a fixed public IP for your instance, you need an Elastic IP.
    - Recommendation is to avoid them.
    - We create an Elastic IP, then associate them to an instance. With this, the IP address of the EC2 instance changes to the Elastic IP.
    - Elastic IP is charged as long as we don't associate them to an instance.
    - Also, we would have to disassociate after use.
  - Placement Groups
    - Cluster
      - Same rack, same AZ. Low latency but if the rack fails, all the instances fail at the same time.
      - Big Data job that needs to complete fast or low latency and high throughput usecases.  
    - Spread
      - Spread across multiple hardwares in a region. Can span across AZs.
      - High availability usecases.
      - 7 is the limit.
    - Partition
      - Each partition is a rack in AWS.
      - 100s of EC2 instances - Kafka, Cassandra, HDFC, HBASE.
  - While launching an EC2 instance, in Advanced section, we can associate the Placement group. 
- Elastic Network Interface (ENI)
  - Represent Virtual Network Card.
  - These are what give EC2 instances access to the network.
  - Each ENI can have
    - A primary private IPv4 and one or more secondary IPv4.
    - Bounded to a specific Availability zone.
    - ![image](https://user-images.githubusercontent.com/42272776/177621658-2aec0817-5e85-461a-9a15-d41018c5bb40.png)
    - ENIs can be moved from one EC2 to another EC2 - useful for failover when a static ip is used.
- EC2 Hibernate
  - RAM state is written to root EBS volume.
  - Long running processes and boot up time. 
  - RootVolume must be EBS, encrypted, not instance store and large.
  - To enable EC2 Hibernate, the EC2 Instance Root Volume type must be an EBS volume and must be encrypted to ensure the protection of sensitive content.z
- EC2 Nitro
  - Underlying platform for EC2 instances.
  - Better networking options, Higher speed EBS. 
- vCPU
  - Multiple threads run on 1 CPU
  - Each thread is a virtual CPU. 

## 11 Classic Solutions Architecture Discussions
-Stateless Application ( Time app )
  - ![image](https://user-images.githubusercontent.com/42272776/177169282-fdd87e14-2c29-49bc-be9c-190feb9755f3.png)
- Stateful Application ( Shopping Cart )
  - We can start with sticky sessions in order to ensure that the user content like cart information is not lost.
  - But this is not fault tolerant. If the server to which the user is assigned is not responsive, the requests may still go to it or the cart may be lost.
  - So another variant is to have client side cookies. This way, the loadbalancer can forward the requests to any EC2 server. But client cookies have a hard limit of 4KB. And http headers increase in size.
  - Another variant is to have an Elastic Cache that holds the session information so that any of the EC2 instances can serve the client. They look at the ElasticCache instead of the cookie/sticky session approach.
  - The solution can also be configured to be MultiAZ to be disaster recoverable. 
  - ![image](https://user-images.githubusercontent.com/42272776/177171616-5981b1f0-1fe3-484b-a317-50a81424f67a.png)
- Picture Upload Usecase
  - ![image](https://user-images.githubusercontent.com/42272776/177185647-3707a835-f754-4c6f-8bd2-bdffd2555624.png) 
- Instantiate Applications Quickly
  - EC2 Instances - Golden AMI. Install applications, OS dependencies beforehand and launch EC2 instances form this AMI.
  - Bootstrap using User Data
  - Hybrid - Mix Golden AMI and User Data
  - RDS Databases
    - Restore from snapshot. The db will have schemas and data ready.
  - EBS Volumes
    - Restore from snapshot. The disk will already be formatted and have data.  
- ![image](https://user-images.githubusercontent.com/42272776/177186210-8a287630-0fb3-4087-b969-067a35fffdb4.png)
- EBS volumnes can only be attached to one EC2 instance at any time.
- EFS is a network file system (NFS) that allows you to mount the same file system to 100s of EC2 instances. Storing software updates on an EFS allows each EC2 instance to access them.

# 7 EC2 Instance Storage
- (Elastic Block Storage) EBS Volume
  - is a network drive that can be attached to instances.
  - Data persists even after EC2 termination.
    - Delete on Termination is also available.
  - Can be attached to only one instance at a time.
  - Bound to a specific availability zone. Just like EC2 instances.
    -  EBS in us-east-1a cannot be attached to us-east-1b.
  - Network USB stick.
  - Root EBS is by default attached to an EC2 and by default it is marked for termination upon EC2 termination.
- EBS Snapshot
  - Make backups.
  - Select the EBS volume of interest and click on Create Snapshot.
  - Can copy snapshots across AZ or region.
  - ![image](https://user-images.githubusercontent.com/42272776/178043885-27c14cf3-c7ac-4040-aaca-1f105d32c28f.png)
  - Archive tier is 75% cheaper, but takes 24-72 hours for restoring the archive.
  - Recycle bin for EBS snapshots are useful to retain snapshots so that they can be recovered after deletion.
- AMI Overview
  - Customization of an EC2 instance.
  - AMIs are built for a specific AWS Region, they're unique for each AWS Region. You can't launch an EC2 instance using an AMI in another AWS Region, but you can copy the AMI to the target AWS Region and then use it to create your EC2 instances.
  - Faster boot time.
  - AMI are built for a specific region and can be copied acorss regions.
  - Public AMI, AWS Marketplance AMI, Own AMI.
  - Building an AMI
    - User Data script helps in customization of an EC2 instance.
    - Image and Template -> Create Image.
    - Launch Instance from AMI or from My AMIs tab in EC2 page.
- EC2 Instance Store
  - For usecases that need higher performance - hardware disk/phyisical connection.
  - Ephemeral storages.
  - Buffer, cache, temporary content.
  - If EC2 instance fails, the hardware fails.
- EBS Volume Types
  - ![image](https://user-images.githubusercontent.com/42272776/178048305-0a883adb-e2f9-40d3-979b-c51e31464bc5.png)
  - When creating EC2 instances, you can only use the following EBS volume types as boot volumes: gp2, gp3, io1, io2, and Magnetic (Standard).
- EBS Multi-attach
  - ![image](https://user-images.githubusercontent.com/42272776/178048687-02a66169-6239-4925-8358-a5074d66617e.png)
  - Using EBS Multi-Attach, you can attach the same EBS volume to multiple EC2 instances in the same AZ. Each EC2 instance has full read/write permissions.
- Encryption
  - ![image](https://user-images.githubusercontent.com/42272776/178048821-c21ca26b-3e92-4b28-9584-47999070d26b.png)
  - ![image](https://user-images.githubusercontent.com/42272776/178049084-4eb0715c-ddc1-46f8-837d-fae74c9c3616.png)
- Amazon EFS
  - Network File System
  - Works with EC2 instances in multi-AZ
  - Highly scalable
  - Only compatible with Linux based AMI (not Windows)
  - ![image](https://user-images.githubusercontent.com/42272776/178049457-677c325f-c24c-45ca-af6d-6c8f7b13f129.png)
  - Content management, web serving
  - ![image](https://user-images.githubusercontent.com/42272776/178049755-8428dccb-f21e-4b7e-9464-a7a94b2b837a.png)
- EBS vs EFS vs Instance store
  - EFS IA helps in reducing costs.
  
# 8 High Availability & Scalability : ELB & ASG
- Load balancer
  - Spreads the load across multiple downstream instances.
  - Expose single point of access to your application. Application can be scaled with lots of additional servers.
  - If downstream instances fail, we can replace them hiding them from user. Also LB does not forward requests to these instances.
  - Does health checks.
  - SSL Termination for your websites.
  - Stickiness.
  - Seperate public and private traffice.
  

- Elastic Load Balancer
  - Managed load balancer.
  - Always working.
  - Only few configurations to be customized.

- Types of LB on AWS
  - ![image](https://user-images.githubusercontent.com/42272776/179928885-1f8a042a-aa51-4873-92bc-5f602f48dd1c.png)
  - ![image](https://user-images.githubusercontent.com/42272776/179929102-4d743ac0-a551-4189-ae33-248f7bb50274.png)
  - On EC2 instances, we only allow traffic from the LOAD Balancer and not from the external world. So security group of LB to security group of EC2.
  
- Classic Load Balancer
  - Supports TCP (Layer 4) & HTTP(s) (Layer 7)
  - Fixed hostname  XXX.region.elb.amazonaws.com
  
- Application Load Balancer
  - Only Layer 7
  - Fixed hostname XXX.region.elb.amazonaws.com
  - Supports load balancing across machines (target groups) and on the same machine (containers).
  - Supports HTTP/2 and WebSocket.
  - Http -> Https redirections are also supported.
  - Also supports routing based on URL to different target groups also on query Strings, headers
    - example.com/users
    - example.com/posts
  - Also supports routing based on hostname in the URL
    - one.example.com
    - other.example.com
  - Best for Microservices and container based applications - Docker and Amazon ECS.
  - Target Groups
    - EC2 instances (can be managed y an Auto Scaling Group) - HTTP
    - ECS tasks
    - Lambda functions ( HTTP request is translated to JSON event)
    - IP Addresses - must be private IPs.
    - ALB can route to multiple target groups.
 
- Examples
  - ![image](https://user-images.githubusercontent.com/42272776/179932617-587a46cc-9306-422b-8473-0884e0a5e510.png)
  - We can split the mobile and desktop traffice across cloud and on-premise hosting platforms using an application load balancer.
  - ![image](https://user-images.githubusercontent.com/42272776/179932691-1681f511-efdf-48ec-acf4-f81a889f5cc1.png)

- Application servers don't see the client IP directly.
  - True IP of the client is in the header X-Forwarded-For.
  - X-Forwarded-Port and X-Forwarded-Proto
  - ![image](https://user-images.githubusercontent.com/42272776/179933859-f187ffc4-46aa-48a8-9de5-9d205e0ffb63.png)

- Network Load Balancer
  - TCP & UDP traffic to your instances.
  - Can handle Millions of request per seconds.
  - Less latency ~ 100ms. For ALB this is 400ms.
  - Has one static IP per AZ and supports assigning Elastic IP - helpful for whitelisting specific IP.
  - Extreme performance. TCP / UDP.
  - Target groups
    - ![image](https://user-images.githubusercontent.com/42272776/179935162-44cbef43-dcad-493e-a64d-8a8e676d1ead.png)

- Gateway Load Balancer
  - Manage a fleet of 3rd party network virtual appliances. 
  - Intrustion detection, Firewalls, Packet inspection usecases.
  - ![image](https://user-images.githubusercontent.com/42272776/180041029-307c13b0-ac6d-438c-a524-891575388b0f.png)
  - Operates at Layer 3 - IP packets / Network layer.
  - Network Gateway + Load Balancer.
  - GENEVE protocol 6081.
  - Target Group (Virtual appliances)
    - EC2 instances
    - IP Addresses

- Stickyness
  - Works for CLB nd ALB.
  - Cookies with expiration are used for stickiness.
  - Cookies
    - Application based - Custom and Application cookies
      - AWSALB, AWSALBAPP, AWSALBTG, AWSALBAPP
    - Duration based
      - AWSALB, AWSELB
  - Stickyness is a concept concerning the target groups a Load balancer bounds.

## 12 Amazon S3
- Many AWS Services have integration with Amazon S3.
- Buckets should be globally unique.
- S3 is global service, Buckets are defined at regional level.
- ![image](https://user-images.githubusercontent.com/42272776/177187980-322922b4-b1b2-4dd8-8862-1cb6af1434e4.png)
- Objects in bucket have Key.
- Key is the Full Path. Object values are the content of the body.
- ![image](https://user-images.githubusercontent.com/42272776/177187738-4552b590-5483-4e1d-9150-3d73f6b416b9.png)
- Maximum size is 5TB. Multi-part upload if the chunk is more than 5GB.
- Versioning has to be enabled at Bucket level.
- ![image](https://user-images.githubusercontent.com/42272776/177188496-3d25cbfc-d0cf-4dcc-8e1e-3c9bed18f19e.png)
- ![image](https://user-images.githubusercontent.com/42272776/177188748-34394dad-ed82-4fe9-ba87-06c57a701d27.png)
- With versions, deleting a main version creates a Delete Marker. Deleting a specific version deletes the file permanently.
- Encryption
  - ![image](https://user-images.githubusercontent.com/42272776/177189263-6a0f6020-9d18-490d-bac1-e0dfc1f517c1.png)
  - ![image](https://user-images.githubusercontent.com/42272776/177189333-7f5388df-c8c2-4f4b-b07f-59c2d3780c5e.png)
  - With SSE-KMS, the encryption happens in AWS, and the encryption keys are managed by AWS but you have full control over the rotation policy of the encryption key. Encryption keys stored in AWS.
  - ![image](https://user-images.githubusercontent.com/42272776/177189433-fa0c476e-e8fb-4bac-aa8e-16cc0321fd72.png)
  - ![image](https://user-images.githubusercontent.com/42272776/177189568-9c124a06-e3ed-42e9-9a37-b6d1ba98df3a.png)
  - With Client-Side Encryption, you have to do the encryption yourself and you have full control over the encryption keys. You perform the encryption yourself and send the encrypted data to AWS. AWS does not know your encryption keys and cannot decrypt your data.
  - ![image](https://user-images.githubusercontent.com/42272776/177189640-2b428968-6824-4bcc-82ca-de3b81aba7c8.png)
- Security
  -  ![image](https://user-images.githubusercontent.com/42272776/177190262-f517e751-87d5-4a06-8c72-f6e3605d01ab.png)
  -  ![image](https://user-images.githubusercontent.com/42272776/177190489-0735768f-1854-4e67-9c07-812bae34e3b0.png)
  -  ![image](https://user-images.githubusercontent.com/42272776/177190539-67f519f7-b984-4ac5-b2ed-d8355b54f5c0.png)
  -  ACLs are another way of protecting objects at object level.
- Website
  - ![image](https://user-images.githubusercontent.com/42272776/177191150-2fb85b21-6dd4-4976-9bf3-16c347f8ad90.png)
  - Enable Static Hosting
  - Uncheck Block Public Access
  - Add a policy - anyone can do a GetObject on S3 for our specific S3 bucket ARN.
- CORS
  -  ![image](https://user-images.githubusercontent.com/42272776/177191722-9079acac-fa04-4699-8104-13621bd3dda4.png)
  -  ![image](https://user-images.githubusercontent.com/42272776/177191902-df74a2bb-21a3-4127-a756-34213477627e.png)
  -  ![image](https://user-images.githubusercontent.com/42272776/177192485-304178f0-73a0-4957-89c1-cb888839ffc9.png)
  -  Website 2 needs CORS setting to be configured if Website 1 has to use it.
  -  Cross-Origin Resource Sharing (CORS) defines a way for client web applications that are loaded in one domain to interact with resources in a different domain. To learn more about CORS, go here: https://docs.aws.amazon.com/AmazonS3/latest/dev/cors.html
 - S3 uses strong consistency.
 - Explicit DENY in an IAM Policy will take precedence over an S3 bucket policy.

# 8 High Availability and Scalability : ELB and ASG





# 13 AWS SDK, IAM Roles & Policies
- AWS Instance Metadata - EC2 instances can learn about themselves without using IAM Role.
- http://169.254.169.254
- us-east-1 would be taken as default for AWS SDKs.
- IAM Roles are the right way to provide credentials and permissions to an EC2 instance.
- AWS Policy simulator helps in understanding if a right is denied or not.\
- meta-data would contain information like AMI-ID, instance-id etc.

# 15 CloudFront & AWS Global Accelerator
- AWS CloudFront
  - Prevents DDoS
  - CDN
  - Distribute READs across the world.
  - Exposes external HTTPS and can talk to internal HTTPS backends.
  - Origins
    - S3 bucket
    - Custom Origin (HTTP)

# 18 Containers - ECS, Fargate, ECR and EKS
- ECS
  - ECS Tasks on ECS Clusters
  - EC2 Launch Type
    - You must provision and maintain the infrastructure.
    - Each EC2 instance must run the ECS Agent to register in the ECS cluster.
    - AWS takes care of starting and stopping the containers.
    - ![image](https://user-images.githubusercontent.com/42272776/177372814-a71683a7-9d26-480c-816f-f5be45534d75.png)
  - Fargate Launch Type
    - Do not provision the infrastructure - Serverless.
    - Just create task definitions.
    - To scale, increase the number of tasks.
    - ![image](https://user-images.githubusercontent.com/42272776/177372890-af12f4f2-59f9-429b-9b32-7fb5d67050a7.png)
  - ![image](https://user-images.githubusercontent.com/42272776/177373032-0f61dc5a-cdde-4356-b71f-06576a450834.png)
  - Application load balancer goes well with ECS Container deployments. Elastic load balancer does not integrate with Fargate.
  - Amazon EFS can be used to share / act like a network file system for ECS/Fargate.
  - EFS volume can be shared between different EC2 instances and different ECS Tasks. It can be used as a persistent multi-AZ shared storage for your containers.
- ECS Service Auto Scaling
  - ECS Tasks are available in 3 AZs.
  - ALB Request Counter per target, ECS Service Average CPU Utilization or Memory Utilization.
  - ECS Cluster Capability Provider is preferred to Auto Scaling Group Scaling.
  - ![image](https://user-images.githubusercontent.com/42272776/177375666-a10c3db7-7648-4a07-baa1-4a6a070b43b8.png)
  - EC2 Instance Profile is the IAM Role used by the ECS Agent on the EC2 instance to execute ECS-specific actions such as pulling Docker images from ECR and storing the container logs into CloudWatch Logs.
  - ECS Task Role is the IAM Role used by the ECS task itself. Use when your container wants to call other AWS services like S3, SQS, etc.
- EKS
  - Managed Kubernetes clusters on AWS.
  - If your company is already using K8s then EKS is a good choice.
  - ![image](https://user-images.githubusercontent.com/42272776/177374725-ce530089-8275-4dd3-b22b-52efe7459ada.png)
- ECR
  - Backed by Amazon S3. Images stored here.
  - ECS Clusters use an IAM Role to connect to ECR Reporsitory and pull images.

# 21 Databases
- ![image](https://user-images.githubusercontent.com/42272776/177026154-8eba14b1-249c-405f-8d06-e76bb1c75dce.png)
- Managed PostgreSQL, MySQL, Oracle or SQL Server
- S3 is good for big objects, not so great for small objects.
- Amazon S3 is indeed a key-value store! (where the key is the full path of the object in the bucket).
- DynamoDB maximum object size is 400kb. Not fit for larger objects.
- Amazon Athena is an interactive serverless query service that makes it easy to analyze data in S3 buckets using Standard SQL.
- RedShift does not replicate by default. So we need to automate snapshot copy across regions.
- ElasticSearch is renamed to OpenSearch. It allows look ups based on partial matches.
- AWS Glue is a serverless data-preparation service for extract, transform, and load (ETL) operations.

# 26 Disaster Recovery
- Recovery Point Object
  - How frequently do you take backups. 
- Recovery Time Object
  - When do you recover in case of a disaster
- ![image](https://user-images.githubusercontent.com/42272776/178060344-ec9cd337-1fab-4a77-8cb0-e152c9f918b3.png)



# 27 Machine Learning
- Rekognition
  - Find objects, people, text, scenes in images and videos using ML.
  - Facial analysis and Facial search, people counting.
  - Use cases:
    - Laveling, content moderation, text detection, face analysis, celebrity recognition, pathing etc.
- Transcribe
  - Convert speech to text.
  - Automatic speech recognition.
  - Use cases: 
    - Transcribe customer service calls / captioning and subtitling.
- Polly
  - Convert text to speech.
- Translate
  - Convert from one language to another.
- Amazon Lex (Alexa)
  - Build chatbots
  - ASR to convert speech to text
  - NLP
- Amazon Connect
  -  Virtual contact center
  -  80% cheaper
  -  ![image](https://user-images.githubusercontent.com/42272776/177027003-9c1ca3ec-1dd1-4362-a5e4-a41046acd555.png)
- Amazon Comprehend
  -  NLP
- SageMaker
  - ML Models
- Forecast
  - Predict the future sales of a raincoat
- Kendra
  - ML powered Document search service
- Personalize
  - Recommendations
  - This is the same tech used by Amazon.com for giving recommendations
- Textract
  - Extract data from any scanned documents
  - Ex: Driving license 

# 28 More Solution Architectures
- S3 events can only be routed to Lambda, SNS or SQS.
- SQS and SQS FIFO can be polled by Lambda functions and the problematic messages could be sent to Dead Letter Queue after few retries.
- SNS messages could be pushed to Lambda with similar retry + DLQ mechanism.
- SNS could be subscribed to multiple SQS queues - fan out.
- Caching is done at CloudFront, a bit on API Gateway, Redis etc.
- Blocking an IP Address
  - TODO Revisit
- High performance computing
  - AWS Direct Connect
    - Move GBs of data to the cloud over a private secure network.
  - Snowball and Snowmobile
    - Move PBs of data to the cloud.
  - DataSync
    - Move large amount of data between on-premise and S3, EFS, FSx for Windows.

# 29 Other Services

- CI/CD
  - CodeCommit, CodeBuild, Elastic Beanstalk, EC2 fleet using CloudFormation - AWS CodeDeploy. CodePipeline for orchestrating everything.
- CloudFormation
  - IAC
  - Useful for replication of the entire stack.
  - Useful when we want to take down the system and recreate to save costs.
  - CloudFormation StackSets allows you to create, update, or delete CloudFormation stacks across multiple AWS accounts and AWS regions with a single operation.
  - Admin account is needed to create stacks.
- STEP Functions
  - Serverless visual workflow to orchestrate lambda functions. 
  - JSON State machine.
  - ![image](https://user-images.githubusercontent.com/42272776/177164358-1bb2fa20-e82d-4e90-906b-8673d56bab24.png)
  - Simple Workflow Service is older version.
- EMR
  - Supports, Spark, HBase, Presto, Flink
  - Data processing, Machine learning, Web indexing, BigData
- OpsWork
  - Chef and Puppet - server configuration.
  - Configuration as code.
  - Recipes and Manifests.
  - OpsWork = Managed Chef and Puppet.
- AWS Workspaces
  - Managed, Secured Cloud Desktop
  - Eliminates VDI
- AssSync
  - Sycn Mobile and Webapps in real-time.
  - Uses GraphQL
- Cost Explorer
  -  Forecast Usage
  -  Visualize AWS costs and usage over time
  -  Reports that analyze cost and usage data.

# 30 Whitepapers and Architectures - AWS CSAA
- Use auto-scaling groups
- Test systems at production scale
- Automate
- Drive architectures using data
- Well Architected Framework
  - Operational Excellence
  - Security
  - Reliability
  - Performance Efficiency
  - Cost Optimization
  - Sustainability
- Well-architected tool helps in evaluating to what extent we are confirming to the above framework.
- Trusted Advisor
  - AWS Trusted Advisor provides recommendations that help you follow AWS best practices. It evaluates your account by using checks. These checks identify ways to optimize your AWS infrastructure, improve security and performance, reduce costs, and monitor service quotas.
  - Cost Optimization, Performane, Security, Fault Tolerance, Service Limits.
  - Business and Enterprise support plans - API access for example. Otherwise, just Core checks listed above.
  - Weekly notifications are also possible.


# Learn
- Reserved instances cost effective?
- DNS A, CNAME and Alias records.
- Well architcted application.
- Security Groups
- Private and Public IP
- Subnet and VPC
- Elastic IP
- Autoscaling group
- When to use EFS and when to use EBS?.
- X-ray
- ![image](https://user-images.githubusercontent.com/42272776/177186922-f4444f1e-2272-488d-93be-4cac42eec8ee.png)

# Revisit
- 201 and 202 Content in section 18
- 348 - Blocking an IP address
- 44 Spot Instances & Spot Fleet
- 45 EC2 Instances Launch Type Hands On
- 55 Advanced concepts - Nitro, vCPU, Capacity Reservations
- 63 EBS Volume Types
- 67 EFS Hands on

 





