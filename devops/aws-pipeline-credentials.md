# Creating Users for Pipelines in AWS with Role-Based Permissions

The process of creating **users for pipelines in AWS** that are restricted to only assuming specific roles with policies is an **AWS best practice** for securing CI/CD pipelines. This approach limits the permissions granted to pipeline users and leverages **IAM roles with trust relationships** for the actual operations.

---

## Steps to Create Pipeline Users with Restricted Access

### 1️⃣ Create an IAM Role for the Pipeline

This role will have the permissions needed for the pipeline (e.g., deploying resources, accessing services).

1. **Go to IAM Console**:
    - Navigate to **IAM** > **Roles** > **Create Role**.

2. **Select Trusted Entity**:
    - Choose **Another AWS Account**.
    - Enter the **AWS Account ID** of the pipeline user or service (the AWS account where the pipeline user exists).

3. **Attach Policies**:
    - Attach the necessary policies for the pipeline to perform its tasks. For example:
      ```json
      {
        "Version": "2012-10-17",
        "Statement": [
          {
            "Effect": "Allow",
            "Action": [
              "s3:*",
              "dynamodb:*",
              "ec2:*",
              "iam:*"
            ],
            "Resource": "*"
          }
        ]
      }
      ```
      Tailor the policy to restrict access to specific resources or actions.

4. **Set Trust Relationship**:
    - Edit the trust relationship to allow the pipeline user to assume the role:
      ```json
      {
        "Version": "2012-10-17",
        "Statement": [
          {
            "Effect": "Allow",
            "Principal": {
              "AWS": "arn:aws:iam::123456789012:user/my-pipeline-user"
            },
            "Action": "sts:AssumeRole"
          }
        ]
      }
      ```

---

### 2️⃣ Create the Pipeline IAM User

This is the user account that will assume the pipeline role.

1. **Go to IAM Console**:
    - Navigate to **IAM** > **Users** > **Add User**.

2. **Set Access Type**:
    - Select **Programmatic Access** to generate an **Access Key ID** and **Secret Access Key** for the pipeline.

3. **Attach Inline Policy to Allow Role Assumption**:
    - Attach a policy allowing the user to assume the pipeline role:
      ```json
      {
        "Version": "2012-10-17",
        "Statement": [
          {
            "Effect": "Allow",
            "Action": "sts:AssumeRole",
            "Resource": "arn:aws:iam::123456789012:role/my-pipeline-role"
          }
        ]
      }
      ```

4. **Save the Access Keys**:
    - Save the generated **Access Key ID** and **Secret Access Key** securely. These will be used in the pipeline configuration.

---

### 3️⃣ Configure the Pipeline to Assume the Role

Once the user is set up, configure the pipeline to use the credentials and assume the role.

1. **Set AWS CLI Profile**:
    - Configure the pipeline to use the user’s access keys:
      ```bash
      aws configure --profile my-pipeline-user
      ```

2. **Use the Role in the Pipeline**:
    - The pipeline should explicitly assume the role at the start of execution:
      ```bash
      aws sts assume-role \
        --role-arn "arn:aws:iam::123456789012:role/my-pipeline-role" \
        --role-session-name "pipeline-session"
      ```
    - This command returns temporary credentials that the pipeline can use for subsequent operations.

3. **Export Temporary Credentials**:
    - Extract the `AccessKeyId`, `SecretAccessKey`, and `SessionToken` from the `assume-role` output and set them as environment variables:
      ```bash
      export AWS_ACCESS_KEY_ID=<TemporaryAccessKeyId>
      export AWS_SECRET_ACCESS_KEY=<TemporarySecretAccessKey>
      export AWS_SESSION_TOKEN=<TemporarySessionToken>
      ```

4. **Perform Pipeline Operations**:
    - After assuming the role, the pipeline can execute commands like deploying resources, managing infrastructure, or accessing AWS services.

---

### 4️⃣ Test the Configuration

- Ensure the pipeline user can successfully assume the role and execute required tasks.
- Validate permissions by attempting operations outside the allowed scope to confirm restrictions are working.

---

## Best Practices

1. **Principle of Least Privilege**:
    - Minimize the number of open permissions granted to both the user and the role.

2. **Limit Role Trust**:
    - Restrict the role’s **trust relationship** to the specific pipeline user.

3. **Use Session Duration**:
    - Limit the duration of the session when assuming the role to reduce security risks (default: 1 hour).

4. **Rotate Access Keys**:
    - Regularly rotate the pipeline user’s **Access Key ID** and **Secret Access Key**.

5. **Monitor Activity**:
    - Use **AWS CloudTrail** to monitor role assumption and resource access activities.

---

## Diagram of the Setup
```
+----------------------+          AssumeRole          +---------------------+
|  Pipeline IAM User   | ---------------------------> |  Pipeline IAM Role  |
| (Access Key + Secret)|                              | (Policies Attached) |
+----------------------+                              +---------------------+
^                                     |
|                                     v
Configured in Pipeline                Executes AWS Operations
```
---

This approach ensures your pipeline user only has the ability to assume roles, while all actual permissions are defined in the role itself, providing greater control and security.

