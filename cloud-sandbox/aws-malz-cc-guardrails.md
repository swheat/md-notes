# Determining and Setting Up Guardrails for a Multi-Account Landing Zone of Sandbox Accounts

Guardrails are critical for maintaining security, compliance, and governance in a multi-account environment. They act as preventative or detective controls, ensuring accounts adhere to organizational policies.

---

## **Process for Determining Necessary Guardrails**

### **1. Understand Organizational Requirements**
- **Security Policies**: Review the organization’s security and compliance standards.
- **Usage Patterns**: Identify how sandbox accounts will be used (e.g., development, testing).
- **Risk Assessment**: Analyze risks associated with sandbox accounts, such as data exposure or overprovisioned resources.

### **2. Classify Guardrail Types**
AWS Control Tower offers two main types of guardrails:
- **Preventive**: Restrict actions that violate policies.
- **Detective**: Monitor resources and report violations.

Example categories:
- **Security**: Enforce encryption, restrict public access.
- **Operations**: Ensure resource tagging, monitor usage.
- **Cost Management**: Restrict high-cost resources.

### **3. Select Guardrails Based on Sandbox Requirements**
Common guardrails for sandbox accounts include:
- **Preventive Guardrails**:
    - Disallow public access to S3 buckets.
    - Enforce the use of customer-managed keys for encryption.
    - Limit account creation within specific organizational units (OUs).
- **Detective Guardrails**:
    - Detect untagged resources.
    - Monitor CloudTrail for changes to sensitive configurations.
    - Identify excessive resource usage.

### **4. Prioritize Guardrails**
- **High Priority**: Guardrails addressing critical security risks (e.g., unauthorized public access, unencrypted data).
- **Medium Priority**: Guardrails focused on operational efficiency and compliance.
- **Low Priority**: Guardrails for non-critical usage, such as cost monitoring.

### **5. Review AWS Control Tower’s Built-In Guardrails**
AWS Control Tower provides preconfigured guardrails that align with AWS best practices. Start with these and extend as needed:
- Preventive guardrails: Restrict configurations.
- Detective guardrails: Monitor compliance.

---

## **Setting Up Guardrails**

### **1. Launch AWS Control Tower**
- Navigate to the AWS Control Tower dashboard.
- Ensure your landing zone is set up and operational.

### **2. Assign Guardrails to Organizational Units (OUs)**
- In the AWS Control Tower console:
    1. Go to the **Guardrails** section.
    2. Select the desired **OU** (e.g., `Sandbox`).
    3. Review and attach relevant guardrails.

### **3. Configure Preventive Guardrails**
- **Example**: Prevent public access to S3 buckets.
- Steps:
    1. Select a preventive guardrail from the AWS Control Tower list.
    2. Attach it to the `Sandbox` OU.
    3. Apply changes.

### **4. Configure Detective Guardrails**
- **Example**: Detect untagged resources.
- Steps:
    1. Enable a detective guardrail in the AWS Control Tower console.
    2. Specify the monitoring interval (if applicable).
    3. Attach it to the `Sandbox` OU.

### **5. Validate Guardrail Implementation**
- Review the **Governance Reports** in the AWS Control Tower console to ensure guardrails are active.
- Test sandbox accounts to verify guardrails prevent or detect violations as expected.

---

## **Best Practices for Guardrail Setup**
- **Start Small**: Begin with critical preventive guardrails and expand as necessary.
- **Test Changes**: Validate guardrail impacts in non-production environments before applying them broadly.
- **Monitor Reports**: Regularly review compliance reports in AWS Control Tower to identify violations.
- **Iterate**: Adjust guardrails based on organizational changes or evolving requirements.

---

## **Example Use Cases for Sandbox Guardrails**
1. **Security**:
    - Prevent S3 buckets from being publicly accessible.
    - Enforce encryption for all data at rest.
2. **Operations**:
    - Require tagging for all resources.
    - Monitor unused resources for cost optimization.
3. **Cost Management**:
    - Restrict the use of expensive instance types.
    - Monitor resource usage for unexpected spikes.

---

Guardrails are an essential part of maintaining governance and security in a multi-account landing zone. By carefully selecting and implementing guardrails based on organizational needs, you can ensure sandbox accounts operate securely and efficiently.