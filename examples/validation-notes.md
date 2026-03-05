Validation Notes — AWS 3-Tier HA

Goal: confirm end-to-end connectivity from the app tier to the database tier.

What was validated

ALB routes HTTP traffic to healthy EC2 targets in private subnets

EC2 instances can reach the RDS MySQL endpoint in private subnets

Security group rules enforce least privilege:

internet → ALB only on 80

ALB → App only on 80

App → RDS only on 3306

Example SQL checks

SHOW DATABASES;
SELECT NOW();

Evidence

See screenshots/ (especially the SQL validation screenshot).