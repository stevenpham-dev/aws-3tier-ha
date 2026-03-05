Teardown — AWS 3-Tier HA

Purpose: remove AWS resources to avoid ongoing costs.

Recommended deletion order (to avoid dependency errors):

Compute and load balancing

Delete Auto Scaling Group: ASG-App

Delete Launch Template: LT-App

Delete Target Group: TG-App

Delete Application Load Balancer: ALB-App

Database

Delete RDS instance: rds-app-db

Delete DB subnet group (if created specifically for this project)

Networking

Delete NAT Gateway (if created)

Release Elastic IP used by NAT Gateway

Delete route table associations (if needed)

Delete subnets (public and private)

Detach and delete Internet Gateway

Delete VPC: AWS-3Tier-VPC

Security

Delete security groups:

SG-ALB

SG-App

SG-RDS

Final sanity check

EC2: no instances / ASGs running

RDS: no DB instances

VPC: deleted

ALB/TG: deleted

NAT/EIP: deleted