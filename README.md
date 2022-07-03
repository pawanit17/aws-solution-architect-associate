# Contents
[Getting started with AWS](https://duckduckgo.com)
[IAM & CLI]()
[EC2 Fundamentals](https://duckduckgo.com)

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
