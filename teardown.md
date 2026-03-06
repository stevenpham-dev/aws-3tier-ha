# Teardown — AWS 3-Tier HA

Purpose: remove AWS resources to avoid ongoing costs.

Recommended deletion order (to avoid dependency errors):

## 1) Compute and load balancing
1. **Auto Scaling Group**
   - Delete: `ASG-App`
2. **Launch Template**
   - Delete: `LT-App`
3. **Target Group**
   - Delete: `TG-App`
4. **Application Load Balancer**
   - Delete: `ALB-App`

## 2) Database
5. **RDS**
   - Delete DB instance: `rds-app-db`
6. **DB subnet group**
   - Delete if created specifically for this project

## 3) Networking
7. **NAT Gateway** (if created)
   - Delete NAT Gateway
8. **Elastic IP**
   - Release NAT EIP (if allocated)
9. **Subnets**
   - Delete public + private subnets
10. **Internet Gateway**
   - Detach and delete IGW
11. **VPC**
   - Delete: `AWS-3Tier-VPC`

## 4) Security groups
12. Delete security groups (after dependencies are removed):
- `SG-ALB`
- `SG-App`
- `SG-RDS`

## Final sanity check
Confirm these are gone:
- ALB / Target Group / ASG / Launch Template
- RDS instance + subnet group
- NAT Gateway + EIP (if used)
- Subnets + IGW + VPC
- Project security groups