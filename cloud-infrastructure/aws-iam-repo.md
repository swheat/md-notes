## 📌 Structuring a GitHub Repository for Terraform AWS IAM Management

This article outlines how to structure a GitHub repository to manage AWS IAM resources using Terraform HCL. It covers IAM roles, policies, role attachments, groups, users, and other key IAM components, ensuring a modular, scalable, and maintainable setup.

### ✅ Recommended Repository Structure

```
aws-iam-terraform/
│── modules/                 # Modular IAM resource definitions
│   ├── roles/               # IAM Roles Module
│   │   ├── main.tf          # IAM role definitions
│   │   ├── variables.tf     # Input variables for roles
│   │   ├── outputs.tf       # Outputs for roles
│   │   ├── attachments.tf   # Role policy attachments
│   ├── policies/            # IAM Policies Module
│   │   ├── main.tf          # IAM policy definitions
│   │   ├── variables.tf     # Input variables for policies
│   │   ├── outputs.tf       # Outputs for policies
│   ├── groups/              # IAM Groups Module
│   │   ├── main.tf          # IAM group definitions
│   │   ├── variables.tf     # Input variables for groups
│   │   ├── outputs.tf       # Outputs for groups
│   ├── users/               # IAM Users Module
│   │   ├── main.tf          # IAM user definitions
│   │   ├── variables.tf     # Input variables for users
│   │   ├── outputs.tf       # Outputs for users
│   ├── instance_profiles/   # IAM Instance Profiles
│   │   ├── main.tf          # Instance profile definitions
│   ├── service_roles/       # IAM Service-Linked Roles
│   │   ├── main.tf          # Service-linked roles
│   ├── permissions_boundaries/ # IAM Permissions Boundaries
│   │   ├── main.tf          # IAM permission boundaries
│   ├── access_keys/         # IAM User Access Keys
│   │   ├── main.tf          # Access key definitions
│   ├── mfa/                 # IAM MFA Enforcements
│   │   ├── main.tf          # MFA requirements for users
│   ├── identity_providers/  # IAM SAML Providers (SSO)
│   │   ├── main.tf          # SAML provider definitions
│   ├── account_alias/       # IAM Account Alias
│   │   ├── main.tf          # AWS Account Alias
│── environments/            # Environment-Specific IAM Configurations
│   ├── dev/                 # IAM Configurations for Dev
│   │   ├── main.tf          # Calls IAM modules with dev-specific inputs
│   │   ├── variables.tf     # Dev-specific variable overrides
│   │   ├── outputs.tf       # Dev-specific outputs
│   ├── staging/             # IAM Configurations for Staging
│   ├── prod/                # IAM Configurations for Production
│── policies/                # JSON IAM Policy Documents
│   ├── admin-policy.json    # Example IAM policy file
│   ├── read-only-policy.json # Example IAM policy file
│── terraform/               # Terraform Configuration Files
│   ├── provider.tf          # AWS provider configuration
│   ├── backend.tf           # Remote backend configuration (S3, DynamoDB)
│   ├── versions.tf          # Required Terraform versions
│── .github/workflows/       # GitHub Actions for Terraform CI/CD
│   ├── terraform-plan.yml   # CI workflow to validate and plan Terraform changes
│   ├── terraform-apply.yml  # CI workflow to apply Terraform changes
│── .gitignore               # Ignore unnecessary Terraform files
│── README.md                # Documentation
│── variables.tf             # Global variables file
│── outputs.tf               # Global outputs file
│── terraform.tfvars         # Default Terraform variable values
```

### 📌 Where Each IAM Resource Fits in This Structure

| IAM Resource                | Folder Placement                  | Purpose                              |
|-----------------------------|------------------------------------|--------------------------------------|
| IAM Roles                   | modules/roles/                     | Define IAM roles                     |
| IAM Policies                | modules/policies/                  | Define standalone IAM policies       |
| IAM Role Policy Attachments | modules/roles/attachments.tf       | Attach policies to IAM roles         |
| IAM Groups                  | modules/groups/                    | Manage IAM user groups               |
| IAM Users                   | modules/users/                     | Define IAM users                     |
| IAM Instance Profiles       | modules/instance_profiles/         | Attach IAM roles to EC2 instances    |
| IAM Service-Linked Roles    | modules/service_roles/             | AWS-managed service roles            |
| IAM Permission Boundaries   | modules/permissions_boundaries/    | Restrict max IAM permissions         |
| IAM Access Keys             | modules/access_keys/               | IAM user programmatic access         |
| IAM MFA Enforcement         | modules/mfa/                       | Enforce MFA for users                |
| IAM SAML Providers (SSO)    | modules/identity_providers/        | Enable SAML authentication           |
| IAM Account Alias           | modules/account_alias/             | Customize AWS login URL              |

### 📌 Explanation of Key IAM Modules

#### 1️⃣ IAM Roles (modules/roles/)
- Defines IAM roles with trust policies.
- Uses policy attachments for managing permissions.

Example (modules/roles/main.tf):

```hcl
resource "aws_iam_role" "example_role" {
  name               = var.role_name
  assume_role_policy = file("${path.module}/policies/assume-role.json")
}
```

#### 2️⃣ IAM Role Policy Attachments (modules/roles/attachments.tf)
- Attaches policies to IAM roles.

Example:

```hcl
resource "aws_iam_role_policy_attachment" "example_attachment" {
  role       = aws_iam_role.example_role.name
  policy_arn = aws_iam_policy.example_policy.arn
}
```

#### 3️⃣ IAM Instance Profiles (modules/instance_profiles/main.tf)
- Attaches IAM roles to EC2 instances.

Example:

```hcl
resource "aws_iam_instance_profile" "ec2_profile" {
  name = "instance-profile"
  role = aws_iam_role.example_role.name
}
```

#### 4️⃣ IAM Permissions Boundaries (modules/permissions_boundaries/main.tf)
- Restricts maximum permissions for IAM roles/users.

Example:

```hcl
resource "aws_iam_policy" "permissions_boundary" {
  name   = "developer-boundary"
  policy = file("${path.module}/policies/permissions-boundary.json")
}
```

#### 5️⃣ IAM MFA Enforcement (modules/mfa/main.tf)
- Ensures IAM users use MFA.

Example:

```hcl
resource "aws_iam_user_policy" "mfa_policy" {
  user = aws_iam_user.mfa_user.name
  policy = jsonencode({
    Version = "2012-10-17",
    Statement = [{
      Effect = "Deny",
      Action = "*",
      Resource = "*",
      Condition = {
        Bool = { "aws:MultiFactorAuthPresent" = "false" }
      }
    }]
  })
}
```

#### 6️⃣ IAM SAML Identity Providers (modules/identity_providers/main.tf)
- Enables SSO authentication.

Example:

```hcl
resource "aws_iam_saml_provider" "okta_saml" {
  name                   = "okta-saml-provider"
  saml_metadata_document = file("${path.module}/saml_metadata.xml")
}
```

### 📌 Summary

This Terraform repository structure ensures:
✔ Scalability – Supports new IAM resources as requirements grow.  
✔ Security Best Practices – Enforces least privilege & MFA.  
✔ Automation Ready – Integrates with GitHub Actions for CI/CD.  
✔ Modular Design – Easy to maintain & reuse IAM resources.

