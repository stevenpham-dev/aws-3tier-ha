```md
# Validation Notes — AWS 3-Tier HA

Goal: confirm end-to-end connectivity across the 3-tier architecture and verify least-privilege network access between tiers.

## What was validated

### 1) ALB → EC2 (App Tier)
- The Application Load Balancer routes HTTP traffic to healthy EC2 targets.
- Target Group health checks are passing.

### 2) EC2 (App Tier) → RDS (Data Tier)
- App instances can reach the RDS MySQL endpoint.
- Security groups allow MySQL access only from the App tier.

### 3) Security group segmentation (least privilege)
- **Internet → ALB:** inbound `80` allowed
- **ALB → App:** inbound `80` allowed only from ALB SG
- **App → RDS:** inbound `3306` allowed only from App SG

## Example SQL checks

Used SQL commands to confirm DB connectivity and query execution:

```sql
SHOW DATABASES;
SELECT NOW();

See screenshots/, especially:

07-target-group-health.png

11-rds-config.png

12-security-groups.png

13-sql-validation.png