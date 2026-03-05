AWS High Availability 3-Tier Architecture (HA)

A highly available 3-tier AWS architecture deployed in us-west-2 demonstrating networking fundamentals, load balancing, auto scaling, and database security best practices. The environment uses public and private subnet isolation across two Availability Zones, an internet-facing Application Load Balancer, an Auto Scaling Group of private EC2 instances, and a Multi-AZ RDS MySQL database in private subnets.

Architecture

User / Internet
→ ALB (public subnets across 2 AZs)
→ EC2 App Tier (private subnets, Auto Scaling Group)
→ RDS MySQL (private subnets, Multi-AZ)

Security segmentation:

ALB SG allows inbound HTTP (80) from the internet

App SG allows inbound HTTP (80) only from ALB SG

RDS SG allows inbound MySQL (3306) only from App SG

Diagram:

diagrams/architecture.png

AWS Resources (as deployed)

Region: us-west-2

Networking

VPC: AWS-3Tier-VPC

Public subnets: 2 AZs (for ALB)

Private subnets: 2 AZs (for EC2 + RDS)

Internet Gateway for public routing

Temporary NAT Gateway used for private instance provisioning (cost/lifecycle managed)

Load Balancing

Application Load Balancer: ALB-App (internet-facing)

Target Group: TG-App

Listener: HTTP :80

Compute (App Tier)

Launch Template: LT-App

Auto Scaling Group: ASG-App

Min: 2

Desired: 2

Max: 3

Scaling: Target tracking policy (CPU 50%)

Database (Data Tier)

RDS MySQL: rds-app-db

Instance class: db.t3.micro

Multi-AZ enabled

Public access disabled

Subnet group uses private subnets only

Security Groups (least-privilege access)

SG-ALB:

Inbound: 80 from 0.0.0.0/0

SG-App:

Inbound: 80 from SG-ALB

SG-RDS:

Inbound: 3306 from SG-App

Validation (end-to-end proof)

Connectivity was validated by connecting from a controlled environment and confirming database access with SQL commands, such as:

SHOW DATABASES;

SELECT NOW();

Notes:

Validation steps and screenshots are stored in screenshots/

Additional notes: examples/validation-notes.md

Screenshots

Place screenshots in screenshots/ using consistent numbering:

01-vpc-overview.png

02-subnets.png

03-route-tables.png

04-internet-gateway.png

05-nat-gateway.png (if created)

06-alb.png

07-target-group-health.png

08-launch-template.png

09-auto-scaling-group.png

10-scaling-policy.png

11-rds-config.png

12-security-groups.png

13-sql-validation.png

14-architecture-diagram.png (optional duplicate of diagrams/)

Teardown / Cost Control

See:

teardown.md