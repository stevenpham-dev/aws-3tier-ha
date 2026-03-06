# Validation Notes - AWS 3-Tier HA (us-west-2)

These notes document how the environment was validated after deployment.

## What "success" looks like

- ALB is reachable and returns a valid HTTP response.
- Target group shows healthy instances.
- App tier instances are in private subnets and managed by Auto Scaling.
- RDS is private (not publicly accessible).
- Security group segmentation is enforced:
  - Internet -> ALB on 80
  - ALB -> App on 80
  - App -> RDS on 3306
- Database connectivity is verified using MySQL commands.

## Validation steps

### 1) ALB reachability

- Open the ALB DNS name in a browser.
- Expected: HTTP 200 (or the expected default Nginx response if used).

Related screenshots:
- `screenshots/06-alb.png`
- `screenshots/07-target-group-health.png`

### 2) Target group health

- Go to EC2 -> Target Groups -> Targets.
- Confirm targets are `healthy` in both AZs.

Related screenshots:
- `screenshots/07-target-group-health.png`

### 3) Auto Scaling group status

- Confirm:
  - Desired capacity is met.
  - Instances are being launched via the launch template.
  - Scaling policy exists (target tracking CPU).

Related screenshots:
- `screenshots/08-launch-template.png`
- `screenshots/09-auto-scaling-group.png`
- `screenshots/10-scaling-policy.png`

### 4) RDS configuration check

- Confirm:
  - Multi-AZ enabled
  - Public access disabled
  - Subnet group uses private subnets only

Related screenshots:
- `screenshots/11-rds-config.png`

### 5) Security group segmentation check

Confirm the inbound rules match the intended design:

- SG-ALB:
  - Inbound HTTP 80 from `0.0.0.0/0`
- SG-App:
  - Inbound HTTP 80 from SG-ALB
- SG-RDS:
  - Inbound MySQL 3306 from SG-App

Related screenshots:
- `screenshots/12-security-groups.png`

### 6) SQL connectivity validation (proof)

Connect from a controlled environment that has network access to the database.

Common approaches:
- Use SSM Session Manager into an app instance (preferred if enabled)
- Use a temporary bastion host in a public subnet (then remove it)
- Use an instance that already has the MySQL client installed

Example commands:

```bash
mysql -h <RDS_ENDPOINT> -u <DB_USER> -p