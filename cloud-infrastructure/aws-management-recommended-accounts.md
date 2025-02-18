# Recommended AWS Account Structure for a New AWS Organization

When setting up a new AWS Management Account, you should create initial accounts to follow AWS best practices for security, governance, and cost management. This ensures isolation of workloads, centralized management, and billing control.

## 📌 Recommended AWS Accounts to Provision

Here’s an optimal account structure to follow:

| Account Name         | Purpose                                                        | Example Email            |
|----------------------|----------------------------------------------------------------|--------------------------|
| Management Account   | Controls AWS Organizations, billing, and root administration   | aws-root@example.com     |
| Security Account     | Centralized security services like AWS Security Hub, GuardDuty, IAM, and logging | aws-security@example.com |
| Logging Account      | Stores CloudTrail logs, AWS Config, and SIEM integration       | aws-logging@example.com  |
| Infrastructure Account | Manages shared networking, VPCs, and Transit Gateway         | aws-infra@example.com    |
| Development Account  | Isolated environment for development/testing workloads         | aws-dev@example.com      |
| Staging Account      | Staging/pre-production environment before production deployment | aws-staging@example.com  |
| Production Account   | Hosts production workloads, high security, and restricted access | aws-prod@example.com     |
| Sandbox Account      | Experimental environment for testing AWS services              | aws-sandbox@example.com  |

## 🔹 Step-by-Step Account Setup

### Step 1: Set Up AWS Management Account
- Sign up for a new AWS account (aws-root@example.com).
- Enable AWS Organizations in this account.
- Enable MFA on the root user.
- Set up billing & cost monitoring (AWS Cost Explorer, AWS Budgets).

### Step 2: Create Child Accounts

Create the following child accounts under the Management Account:

```bash
aws organizations create-account --email aws-security@example.com --account-name "Security Account"
aws organizations create-account --email aws-logging@example.com --account-name "Logging Account"
aws organizations create-account --email aws-infra@example.com --account-name "Infrastructure Account"
aws organizations create-account --email aws-dev@example.com --account-name "Development Account"
aws organizations create-account --email aws-staging@example.com --account-name "Staging Account"
aws organizations create-account --email aws-prod@example.com --account-name "Production Account"
aws organizations create-account --email aws-sandbox@example.com --account-name "Sandbox Account"
```

### Step 3: Set Up Organizational Units (OUs)

Organize accounts into OUs for better policy enforcement:

```bash
aws organizations create-organizational-unit --parent-id <org-root-id> --name "Security"
aws organizations create-organizational-unit --parent-id <org-root-id> --name "Infrastructure"
aws organizations create-organizational-unit --parent-id <org-root-id> --name "Workloads"
aws organizations create-organizational-unit --parent-id <org-root-id> --name "Sandbox"
```

- Move Security & Logging Accounts → into Security OU.
- Move Infra Account → into Infrastructure OU.
- Move Dev, Staging, Production Accounts → into Workloads OU.
- Move Sandbox Account → into Sandbox OU.

### Step 4: Apply Service Control Policies (SCPs)

Restrict actions at an organizational level.

Example: Deny EC2 Creation in Security Account

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": "ec2:*",
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "aws:RequestedRegion": "us-east-1"
        }
      }
    }
  ]
}
```

Attach the policy to the Security OU:

```bash
aws organizations attach-policy --policy-id <scp-policy-id> --target-id <security-ou-id>
```

### Step 5: Enable AWS Security Best Practices

- ✔ Enable AWS CloudTrail (log all API actions across accounts)
- ✔ Enable AWS GuardDuty (threat detection for accounts)
- ✔ Enable AWS Config (track AWS resource changes)
- ✔ Enable AWS Security Hub (centralized security management)

Enable these services centrally from the Security Account.

### Step 6: Set Up Cross-Account IAM Roles
- Create IAM Roles for cross-account access (OrganizationAccountAccessRole).
- Allow the Security Account to assume roles in other accounts for monitoring.

Example: Create an IAM Role in each child account

```bash
aws iam create-role --role-name SecurityAuditRole --assume-role-policy-document file://trust-policy.json
```

`trust-policy.json`:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::<SECURITY_ACCOUNT_ID>:root"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

### Step 7: Configure Consolidated Billing
1. Enable Consolidated Billing in AWS Organizations.
2. Enable AWS Cost Explorer for cost tracking.
3. Set AWS Budgets to track and alert on spending.

## 🔹 Final AWS Account Structure

```
AWS Organization (Root)
│── Management Account (aws-root@example.com)
│── Security OU
│   ├── Security Account (aws-security@example.com)
│   ├── Logging Account (aws-logging@example.com)
│── Infrastructure OU
│   ├── Infrastructure Account (aws-infra@example.com)
│── Workloads OU
│   ├── Development Account (aws-dev@example.com)
│   ├── Staging Account (aws-staging@example.com)
│   ├── Production Account (aws-prod@example.com)
│── Sandbox OU
│   ├── Sandbox Account (aws-sandbox@example.com)
```

## 🔹 Summary

By following this approach, you achieve:
- ✔ Security best practices (isolated security & logging).
- ✔ Cost control (consolidated billing & monitoring).
- ✔ Clear workload separation (Development, Staging, Production).
- ✔ Strong governance (Service Control Policies, IAM Roles).

