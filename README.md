# Contents
- [Getting started with AWS](https://github.com/pawanit17/aws-solution-architect-associate/blob/main/README.md#getting-started-with-aws)
- [IAM & CLI](https://github.com/pawanit17/aws-solution-architect-associate/blob/main/README.md#iam)
- [EC2 Fundamentals](https://github.com/pawanit17/aws-solution-architect-associate/blob/main/README.md#ec2-fundamentals)

# Getting started with AWS
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

# IAM
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

# EC2 Fundamentals

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

- Instantiate Applications Quickly
  - EC2 Instances - Golden AMI. Install applications, OS dependencies beforehand and launch EC2 instances form this AMI.
  - Bootstrap using User Data
  - Hybrid - Mix Golden AMI and User Data
  - RDS Databases
    - Restore from snapshot. The db will have schemas and data ready.
  - EBS Volumes
    - Restore from snapshot. The disk will already be formatted and have data.  



# Databases
- ![image](https://user-images.githubusercontent.com/42272776/177026154-8eba14b1-249c-405f-8d06-e76bb1c75dce.png)
- Managed PostgreSQL, MySQL, Oracle or SQL Server
- S3 is good for big objects, not so great for small objects.
- Amazon S3 is indeed a key-value store! (where the key is the full path of the object in the bucket).
- DynamoDB maximum object size is 400kb. Not fit for larger objects.
- Amazon Athena is an interactive serverless query service that makes it easy to analyze data in S3 buckets using Standard SQL.
- RedShift does not replicate by default. So we need to automate snapshot copy across regions.
- ElasticSearch is renamed to OpenSearch. It allows look ups based on partial matches.
- AWS Glue is a serverless data-preparation service for extract, transform, and load (ETL) operations.

# Machine Learning
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

# More Solution Architectures


# Other Services

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


# Learn
- Reserved instances cost effective?
- DNS A, CNAME and Alias records.
- Well architcted application.
- Security Groups
- Private and Public IP
- Elastic IP
- Autoscaling group

 





