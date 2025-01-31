# How to Set Up a New AWS Management Account with Two Child Accounts in an Organization

This guide explains how to set up an **AWS Management Account** with two child accounts using AWS Organizations. The structure will enable consolidated billing, centralized management, and account isolation.

---

## **Step 1: Create the AWS Management Account**
1. **Sign Up for a New AWS Account**:
    - Go to the [AWS Signup Page](https://aws.amazon.com/).
    - Use an email address dedicated to administration (e.g., `aws-admin@example.com`).

2. **Verify the Account**:
    - Enter billing details and verify your account via SMS or email.
    - This account becomes your **Management Account**.

3. **Enable MFA for the Root User**:
    - Navigate to **IAM** > **Users** > **Your Security Credentials**.
    - Enable **Multi-Factor Authentication (MFA)** for the root user to secure the account.

---

## **Step 2: Enable AWS Organizations**
1. **Log In to the Management Account**:
    - Use the root user credentials to access the AWS Management Console.

2. **Enable AWS Organizations**:
    - Navigate to **AWS Organizations**.
    - Select **Create an organization** and choose **Enable all features** to unlock advanced account management.

---

## **Step 3: Create Child Accounts**
AWS Organizations allows you to create child accounts directly under the management account.

1. **Create the First Child Account**:
    - Navigate to **AWS Organizations** > **Accounts** > **Add an account** > **Create an AWS account**.
    - Provide:
        - **Account Name**: e.g., "Development Account"
        - **Email Address**: e.g., `aws-dev@example.com`

2. **Create the Second Child Account**:
    - Repeat the process:
        - **Account Name**: e.g., "Production Account"
        - **Email Address**: e.g., `aws-prod@example.com`

3. **Verify the Accounts**:
    - Check the email inbox for each child account and confirm activation.
    - Log in to complete any required setup (e.g., MFA, IAM user creation).

---

## **Step 4: Set Up Consolidated Billing**
1. **Enable Consolidated Billing**:
    - Navigate to **Billing** > **Consolidated Billing** in the AWS Management Console.
    - Ensure that charges from child accounts are grouped under the management account.

2. **Enable Cost Explorer**:
    - Go to **Billing** > **Cost Explorer** and enable it to track spending across all accounts.

3. **Set Budgets** (Optional):
    - Use **AWS Budgets** to monitor and alert on spending in each account.

---

## **Step 5: Organize Accounts with Organizational Units (OUs)**
1. **Create OUs**:
    - Navigate to **AWS Organizations** > **Organizational units** > **Create an OU**.
    - Create OUs for different environments, such as:
        - **Development**
        - **Production**

2. **Move Accounts into OUs**:
    - Drag and drop each child account into its respective OU.

---

## **Step 6: Apply Service Control Policies (SCPs)**
1. **Enable SCPs**:
    - Go to **AWS Organizations** > **Policies** > **Service Control Policies**.
    - Enable SCPs for your organization.

2. **Create SCPs**:
    - Example: Deny certain actions in the "Development" OU:
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

3. **Attach SCPs to OUs**:
    - Assign the SCP to the appropriate OU (e.g., "Development").

---

## **Step 7: Configure Security Best Practices**
1. **Enable AWS CloudTrail**:
    - Enable CloudTrail in the management account to monitor actions across all accounts.

2. **Enable AWS Config**:
    - Set up AWS Config to track resource changes in all accounts.

3. **Enable GuardDuty**:
    - Enable GuardDuty for continuous threat detection in all accounts.

4. **Secure Root Users**:
    - Enable MFA for the root user in all accounts.
    - Avoid using the root user for daily tasks—create IAM users or roles for operations.

---

## **Step 8: Test and Validate the Setup**
1. **Verify Billing**:
    - Check the **Billing Dashboard** to ensure charges from all child accounts are consolidated.

2. **Test SCPs**:
    - Attempt restricted actions in the "Development" account to confirm SCP enforcement.

3. **Validate Account Isolation**:
    - Ensure that resources in one account are not accessible from another.

---

## **AWS CLI Commands for Automation**
You can automate parts of this setup using the AWS CLI:

1. **Enable AWS Organizations**:
   ```bash
   aws organizations enable-aws-service-access --service-principal organizations.amazonaws.com

	2.	Create a Child Account:

aws organizations create-account --email aws-dev@example.com --account-name "Development Account"
aws organizations create-account --email aws-prod@example.com --account-name "Production Account"


	3.	List All Accounts:

aws organizations list-accounts


	4.	Create Organizational Units (OUs):

aws organizations create-organizational-unit --parent-id <root-id> --name "Development"
aws organizations create-organizational-unit --parent-id <root-id> --name "Production"


	5.	Attach an SCP to an OU:

aws organizations attach-policy --policy-id <scp-id> --target-id <ou-id>

Summary

By following these steps, you will:
•	Set up a management account.
•	Create two child accounts for development and production environments.
•	Use AWS Organizations to centralize management, billing, and access control.

This structure ensures best practices for account isolation, cost monitoring, and security.

Let me know if you need additional assistance! 🚀

