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

- 
