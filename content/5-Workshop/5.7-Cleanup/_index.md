---
title: "Resource Cleanup"
date: 2026-07-21
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

# Resource Cleanup

To prevent unnecessary continuous charges on your AWS account after completing this workshop, perform the cleanup steps below in order:

---

### Cleanup Steps:

1. **Delete EC2 Auto Scaling Group & Instances**:
   * Navigate to **Amazon EC2** -> **Auto Scaling Groups** -> select your ASG and click **Delete**.
   * Wait for all EC2 Spot instances and Warm Pool instances to terminate.
   * Navigate to **Launch Templates** and delete your created Launch Template.

2. **Delete Amazon API Gateway**:
   * Navigate to **API Gateway** -> select `FightingGameAPI` -> click **Actions** -> **Delete**.

3. **Delete AWS Lambda Functions**:
   * Navigate to **AWS Lambda** -> select `FightingGameMatchmaker` and `MatchAnalyticLambda` -> click **Delete**.

4. **Delete Amazon DynamoDB Tables**:
   * Navigate to **DynamoDB** -> **Tables** -> select `MatchmakingQueue` and `ActiveMatches` -> click **Delete table**.

5. **Delete Amazon Cognito User Pool & Identity Pool**:
   * Navigate to **Cognito** -> **Identity pools** -> delete `FightingGameIdentityPool`.
   * Navigate to **User pools** -> delete `ap-southeast-1_phYoaMUPC`.

6. **Empty & Delete Amazon S3 Bucket**:
   * Navigate to **S3** -> select your asset bucket -> click **Empty** to purge all stored objects, then click **Delete**.

7. **Delete IAM Roles & CodeDeploy Applications**:
   * Delete CodeDeploy application `FightingGameServerApp`.
   * Navigate to **IAM** -> **Roles** -> delete created IAM Roles.
