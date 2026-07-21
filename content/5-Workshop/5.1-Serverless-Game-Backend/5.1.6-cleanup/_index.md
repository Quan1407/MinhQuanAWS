---
title: "Resource Cleanup"
date: 2026-07-21
weight: 6
chapter: false
pre: " <b> 5.1.6. </b> "
---

# 5.1.6. Resource Cleanup

To prevent unnecessary continuous charges on your AWS account after completing this workshop, follow the detailed step-by-step instructions below to remove all provisioned services and resources:

---

### Step 1: Clean up Amazon Cognito

1. Navigate to **Amazon Cognito** -> **User pools**.
2. Select your provisioned **User pool** (e.g., `ap-southeast-1_phYoaMUPC`), click **Delete**, and confirm deletion.

![Delete Cognito User Pool](/images/5-Workshop/cleanup/image1.png)

![Confirm Cognito User Pool Deletion](/images/5-Workshop/cleanup/image2.png)

3. Navigate to **Identity pools**, select your provisioned **Identity pool** (e.g., `FightingGameIdentityPool`), click **Delete**, and confirm deletion.

![Delete Cognito Identity Pool](/images/5-Workshop/cleanup/image3.png)

![Confirm Cognito Identity Pool Deletion](/images/5-Workshop/cleanup/image4.png)

---

### Step 2: Clean up Amazon DynamoDB

1. Navigate to **Amazon DynamoDB** -> **Tables**.
2. Select all workshop tables (`MatchmakingQueue` and `ActiveMatches`), click **Delete**, and confirm deletion of all tables.

![Select DynamoDB Tables to Delete](/images/5-Workshop/cleanup/image5.png)

![Confirm DynamoDB Tables Deletion](/images/5-Workshop/cleanup/image6.png)

---

### Step 3: Clean up AWS Lambda Functions

1. Navigate to **AWS Lambda** -> **Functions**.
2. Select all provisioned Lambda functions (`FightingGameMatchmaker` and `MatchAnalyticLambda`), click **Actions** -> **Delete**, and confirm deletion.

![Select Lambda Functions to Delete](/images/5-Workshop/cleanup/image7.png)

![Confirm Lambda Functions Deletion](/images/5-Workshop/cleanup/image8.png)

---

### Step 4: Clean up Amazon API Gateway

1. Navigate to **Amazon API Gateway** -> **APIs**.
2. Select your provisioned REST API (`FightingGameAPI`), click **Delete**, and enter confirmation text to permanently remove the API Gateway.

![Select API Gateway to Delete](/images/5-Workshop/cleanup/image9.png)

![Confirm API Gateway Deletion](/images/5-Workshop/cleanup/image10.png)

---

### Step 5: Clean up Amazon CloudFront Distribution

1. Navigate to **Amazon CloudFront** -> **Distributions**.
2. Select your application distribution and click **Disable** to suspend edge traffic routing.

![Disable CloudFront Distribution](/images/5-Workshop/cleanup/image11.png)

![Confirm Disabling CloudFront Distribution](/images/5-Workshop/cleanup/image12.png)

3. **Note**: Wait approximately 5 to 10 minutes for status propagation across AWS Edge locations until the status changes to **Disabled**. Once enabled, the **Delete** button will activate; click **Delete** to remove the distribution.

![Delete CloudFront Distribution](/images/5-Workshop/cleanup/image13.png)

---

### Step 6: Clean up AWS WAF & Shield

1. Navigate to **AWS WAF & Shield** -> **Web ACLs**.
2. Select your Web ACL (e.g., `CreatedByCloudFront-56a8180e`), click **Actions** -> **Manage resources**.
3. Click **Disassociate** to unbind the Web ACL from your CloudFront distribution, then click **Delete** to permanently remove the Web ACL.

![Manage Web ACL Association](/images/5-Workshop/cleanup/image14.png)

![Delete Web ACL in AWS WAF](/images/5-Workshop/cleanup/image15.png)

---

> [!TIP]
> Completing all resource cleanup steps ensures your AWS account incurs no unexpected residual costs post-workshop.
