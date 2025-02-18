# 🚀 How to Set Up AWS Control Tower

AWS Control Tower provides a fully managed way to set up and govern a multi-account AWS environment using best practices, guardrails, and automation.

## 📌 Key Features of AWS Control Tower

- ✅ Automated Multi-Account Setup (Creates core accounts & OUs)
- ✅ Preconfigured Security Best Practices (CloudTrail, GuardDuty, IAM roles)
- ✅ Centralized Governance & Guardrails (SCPs, Config, IAM)
- ✅ Easy Account Provisioning (via Account Factory)
- ✅ Integrated Cost Tracking (AWS Budgets, Consolidated Billing)

## 🔹 Step-by-Step AWS Control Tower Setup

### Step 1: Enable AWS Control Tower
1. Log in to the AWS Management Console (Root Account)
2. Navigate to AWS Control Tower:
   👉 AWS Console → Control Tower
3. Click “Set Up Landing Zone”
4. Select AWS Region (Control Tower is available in limited regions)
5. Enable AWS Organizations (if not already enabled)
6. Configure Core Accounts:
   - Management Account (Billing & Governance)
   - Log Archive Account (Stores security logs, CloudTrail logs)
   - Audit Account (For security monitoring & compliance)
7. Enable Mandatory Guardrails (Preventive & Detective controls)
8. Click “Set Up Landing Zone”

⏳ Wait ~30 minutes for AWS Control Tower to deploy.

### Step 2: Create Organizational Units (OUs)

AWS Control Tower automatically creates:
- ✔ Security OU (for logging & auditing)
- ✔ Sandbox OU (for experimentation)

📌 You can create additional OUs for Development, Staging, Production:
1. Go to AWS Control Tower → Organizational Units
2. Click “Create Organizational Unit”
3. Enter OU Names (e.g., Development, Production, Infrastructure)
4. Assign Accounts to the OUs

### Step 3: Provision New AWS Accounts

To create new AWS accounts under AWS Control Tower:
1. Go to AWS Control Tower → Account Factory
2. Click “Enroll Account”
3. Enter Account Details:
   - Account Name (e.g., “Development Account”)
   - Email Address (e.g., aws-dev@example.com)
   - Select an Organizational Unit (e.g., Development)
4. Click “Enroll Account”

⏳ AWS Control Tower will provision the account automatically.

### Step 4: Apply Guardrails (Policies & SCPs)

Control Tower includes pre-configured Guardrails:
- ✔ Preventive Guardrails (SCPs that block unwanted actions)
- ✔ Detective Guardrails (AWS Config rules that monitor compliance)

To apply additional SCPs:
1. Go to AWS Control Tower → Guardrails
2. Select a Guardrail (e.g., “Prevent Public S3 Buckets”)
3. Click “Enable”
4. Attach it to an Organizational Unit

📌 Example Custom SCP (Deny EC2 in the Dev OU)

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": "ec2:*",
      "Resource": "*"
    }
  ]
}
```

Attach this SCP to the Development OU.

### Step 5: Enable AWS Security Best Practices

AWS Control Tower automatically enables:
- ✔ CloudTrail (Logs all AWS API activity)
- ✔ AWS Config (Monitors AWS resource changes)
- ✔ GuardDuty (Threat detection for AWS accounts)
- ✔ IAM Identity Center (SSO) (Centralized user authentication)

## 📝 Terraform Configuration for AWS Control Tower

You can also automate AWS Control Tower setup using Terraform.

📌 Example: `main.tf`

```hcl
provider "aws" {
  region = "us-east-1"
}

# Enable AWS Control Tower (Creates Landing Zone)
resource "aws_controltower_control" "enable_control_tower" {
  target_identifier   = "r-xxxx" # AWS Root ID
  control_identifier  = "guardrails.aws-managed"
}

# Create OUs
resource "aws_controltower_organizational_unit" "dev_ou" {
  name = "Development"
}

resource "aws_controltower_organizational_unit" "prod_ou" {
  name = "Production"
}

# Provision AWS Accounts
resource "aws_controltower_account" "dev_account" {
  account_name        = "Development Account"
  email               = "aws-dev@example.com"
  organizational_unit = aws_controltower_organizational_unit.dev_ou.id
}

resource "aws_controltower_account" "prod_account" {
  account_name        = "Production Account"
  email               = "aws-prod@example.com"
  organizational_unit = aws_controltower_organizational_unit.prod_ou.id
}
```

Deploy Terraform

```sh
terraform init
terraform plan
terraform apply -auto-approve
```

## 🔹 AWS Control Tower Directory Structure (GitHub Repo)

If using GitHub for infrastructure as code (IaC), your repo should be structured like this:

```
aws-control-tower-setup/
│── terraform/
│   ├── main.tf               # Creates AWS Control Tower
│   ├── accounts.tf           # Provisions AWS accounts
│   ├── organizational-units.tf # Sets up OUs
│   ├── guardrails.tf         # Defines SCPs & Policies
│   ├── security.tf           # Enables security services
│── .github/
│   ├── workflows/
│       ├── terraform-ci.yml  # GitHub Actions for Terraform automation
│── README.md                 # Documentation
```

## 🚀 Summary

| Step | Task |
|------|------|
| 1    | Enable AWS Control Tower (Landing Zone) |
| 2    | Create Organizational Units (OUs) |
| 3    | Provision AWS Accounts via Account Factory |
| 4    | Apply Guardrails (SCPs & AWS Config) |
| 5    | Enable Security Services (CloudTrail, GuardDuty, AWS Config) |

- ✅ Best for: Enterprises who want AWS-recommended best practices & automated setup
- ✅ Built-in compliance & security policies
- ✅ Fully automated multi-account setup

