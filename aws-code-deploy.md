# 🚀 AWS CodeDeploy

**AWS CodeDeploy** is a fully managed deployment service that helps you automate the process of deploying applications to various compute services like:

- **Amazon EC2 instances**
- **On-premises servers**
- **AWS Lambda functions**
- **Amazon ECS containers**

---

## 🛠️ Key Features

- **Automated Deployments**: Push updates across environments (dev, test, prod) without manual intervention.
- **Supports Multiple Deployment Types**:
  - *In-place*: Updates existing instances directly.
  - *Blue/Green*: Launches new instances with the update and shifts traffic gradually or all at once.
- **Minimized Downtime**: Rolling updates and health checks ensure your app stays available.
- **Rollback Capabilities**: If something breaks, you can revert to the previous version easily.
- **Flexible Hooks**: Use lifecycle event hooks in an `appspec.yml` file to run scripts before/after deployment steps.

---

## 📦 What You Can Deploy

- Web apps  
- Serverless functions  
- Executables  
- Config files  
- Multimedia assets

---

## 🔧 Integration Possibilities

- Works with GitHub, Bitbucket, and AWS S3 for source code  
- Can be part of a CI/CD pipeline with tools like AWS CodePipeline or Jenkins

---

# 🛡️ IAM Role Creation

## Step 1: Open IAM Console

- Go to the [AWS Management Console](https://aws.amazon.com/console/)
- Navigate to **IAM** (Identity and Access Management)
- In the left sidebar, click **Roles**
- Click **Create role**

---

## Step 2: Select Trusted Entity

- Under **Trusted entity type**, choose **AWS service**
- Under **Use case**, select **EC2**
- Click **Next**  
![alt text](image-21.png)

---

## Step 3: Attach Permissions Policies

- Search for and select policies based on your use case:
  - `AmazonS3ReadOnlyAccess`
  - `AmazonEC2FullAccess`
  - `AmazonSSMManagedInstanceCore`
  - `Amazon EC2 Role For Code deploy` is selected
- You can also create a **custom policy** if needed
- Click **Next**  
![alt text](image-22.png)

---

## Step 4: Add Tags (Optional)

- Tags help organize and manage roles
- Example:
  - Key: `Project`, Value: `MyApp`
  - Key: `Environment`, Value: `Production`
- Click **Next**

---

## Step 5: Name and Review

- Give your role a **unique name**, e.g., `CodeDeployEC2Role`
- Review all settings and attached policies
- Click **Create role**  
![alt text](image-23.png)

---

## Step 6: Verify Instance Profile

- AWS automatically creates an **instance profile** with the same name as the role
- This profile is what EC2 uses to assume the role

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "s3:GetObject",
        "s3:GetObjectVersion",
        "s3:ListBucket"
      ],
      "Effect": "Allow",
      "Resource": "*"
    }
  ]
}
```

---
# ✅ Step 7: Attach IAM Role to EC2 Instance

This step shows how to assign your newly created IAM role to an EC2 instance — either during launch or to an existing one.

---

## 📦 During EC2 Launch

1. Go to **EC2 → Launch Instance**
2. In the **Configure Instance Details** section, locate the **IAM Role** dropdown
3. Select the IAM role you created (e.g., `CodeDeployEC2Role`)
4. Proceed with the remaining launch steps:
   - Add storage
   - Configure security group
   - Review and launch

---

## 🔄 For Existing EC2 Instance

1. Navigate to **EC2 → Instances**
2. Select the instance to modify
3. Click **Actions → Security → Modify IAM Role**
4. Choose your IAM role (e.g., `CodeDeployEC2Role`) from the dropdown
5. Click **Update IAM Role** to attach

> 📌 Once attached, the instance assumes the permissions granted by the IAM role and can seamlessly interact with AWS services like S3, CodeDeploy, etc.

---
