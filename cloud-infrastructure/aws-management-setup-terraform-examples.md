# AWS Organizations Account Setup Using Terraform

Here’s the AWS Organizations Account Setup using Terraform to provision accounts, organizational units (OUs), and apply security best practices.

## 📌 Recommended AWS Account Structure

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

## 🔹 Step 1: Set Up AWS Organizations & Accounts Using Terraform

This Terraform script:
- ✔ Creates an AWS Organization
- ✔ Provisions child accounts
- ✔ Configures organizational units (OUs)
- ✔ Sets up Service Control Policies (SCPs)

### 📝 Terraform Configuration: main.tf

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_organizations_organization" "org" {
  feature_set = "ALL"
}

# Create Organizational Units (OUs)
resource "aws_organizations_organizational_unit" "security" {
  name      = "Security"
  parent_id = aws_organizations_organization.org.roots.id
}

resource "aws_organizations_organizational_unit" "infrastructure" {
  name      = "Infrastructure"
  parent_id = aws_organizations_organization.org.roots.id
}

resource "aws_organizations_organizational_unit" "workloads" {
  name      = "Workloads"
  parent_id = aws_organizations_organization.org.roots.id
}

resource "aws_organizations_organizational_unit" "sandbox" {
  name      = "Sandbox"
  parent_id = aws_organizations_organization.org.roots.id
}

# Create Child Accounts
resource "aws_organizations_account" "security_account" {
  name      = "Security Account"
  email     = "aws-security@example.com"
  role_name = "OrganizationAccountAccessRole"
  parent_id = aws_organizations_organizational_unit.security.id
}

resource "aws_organizations_account" "logging_account" {
  name      = "Logging Account"
  email     = "aws-logging@example.com"
  role_name = "OrganizationAccountAccessRole"
  parent_id = aws_organizations_organizational_unit.security.id
}

resource "aws_organizations_account" "infra_account" {
  name      = "Infrastructure Account"
  email     = "aws-infra@example.com"
  role_name = "OrganizationAccountAccessRole"
  parent_id = aws_organizations_organizational_unit.infrastructure.id
}

resource "aws_organizations_account" "dev_account" {
  name      = "Development Account"
  email     = "aws-dev@example.com"
  role_name = "OrganizationAccountAccessRole"
  parent_id = aws_organizations_organizational_unit.workloads.id
}

resource "aws_organizations_account" "staging_account" {
  name      = "Staging Account"
  email     = "aws-staging@example.com"
  role_name = "OrganizationAccountAccessRole"
  parent_id = aws_organizations_organizational_unit.workloads.id
}

resource "aws_organizations_account" "prod_account" {
  name      = "Production Account"
  email     = "aws-prod@example.com"
  role_name = "OrganizationAccountAccessRole"
  parent_id = aws_organizations_organizational_unit.workloads.id
}

resource "aws_organizations_account" "sandbox_account" {
  name      = "Sandbox Account"
  email     = "aws-sandbox@example.com"
  role_name = "OrganizationAccountAccessRole"
  parent_id = aws_organizations_organizational_unit.sandbox.id
}
```

## 🔹 Step 2: Apply Service Control Policies (SCPs)

SCPs restrict permissions at the organization level.

### 📝 Terraform Configuration: scp.tf

```hcl
resource "aws_organizations_policy" "deny_ec2_create" {
  name        = "DenyEC2CreationInSecurityOU"
  description = "Prevents EC2 instance creation in the Security OU"
  type        = "SERVICE_CONTROL_POLICY"

  content = jsonencode({
    Version = "2012-10-17",
    Statement = [
      {
        Effect    = "Deny",
        Action    = "ec2:*",
        Resource  = "*",
        Condition = {
          StringEquals = {
            "aws:RequestedRegion" = "us-east-1"
          }
        }
      }
    ]
  })
}

resource "aws_organizations_policy_attachment" "scp_attach" {
  policy_id = aws_organizations_policy.deny_ec2_create.id
  target_id = aws_organizations_organizational_unit.security.id
}
```

## 🔹 Step 3: Enable AWS Security Best Practices

Enable AWS CloudTrail, AWS GuardDuty, AWS Config, and Security Hub in the Security Account.

### 📝 Terraform Configuration: security.tf

```hcl
provider "aws" {
  alias  = "security"
  region = "us-east-1"
  assume_role {
    role_arn = "arn:aws:iam::${aws_organizations_account.security_account.id}:role/OrganizationAccountAccessRole"
  }
}

resource "aws_cloudtrail" "main" {
  provider                 = aws.security
  name                     = "org-trail"
  s3_bucket_name           = "aws-cloudtrail-logs"
  include_global_service_events = true
  is_multi_region_trail    = true
}

resource "aws_guardduty_detector" "main" {
  provider = aws.security
  enable   = true
}

resource "aws_config_configuration_recorder" "main" {
  provider = aws.security
  name     = "config-recorder"
  role_arn = "arn:aws:iam::${aws_organizations_account.security_account.id}:role/OrganizationAccountAccessRole"
}
```

## 🔹 Step 4: Apply Terraform

Once you have all Terraform files (`main.tf`, `scp.tf`, `security.tf`), deploy the setup.

Run the following Terraform commands:

```bash
terraform init
terraform plan
terraform apply -auto-approve
```

This will:
- ✔ Create AWS Organization & OUs
- ✔ Provision child accounts
- ✔ Attach SCPs
- ✔ Enable security services

## 🔹 Summary

This Terraform setup will:
- ✅ Create an AWS Organization
- ✅ Provision core AWS accounts (Security, Infra, Workloads, Sandbox)
- ✅ Apply SCPs for security enforcement
- ✅ Enable security best practices (CloudTrail, GuardDuty, Config)

### Next Steps
- Want Terraform automation for IAM roles, S3 logging, or additional policies?
- Need CI/CD integration for automated AWS Organizations setup?
