---
title: "Asynchronous Analytics"
date: 2026-07-21
weight: 5
chapter: false
pre: " <b> 5.1.5. </b> "
---

# 5.1.5. Asynchronous Post-Match Analytics with DynamoDB Streams

In this section, we will enable **DynamoDB Streams** on the DynamoDB tables and implement the **MatchAnalytic Lambda** function to capture post-match logs, metrics, and player statistics asynchronously (Asynchronous Event Processing), isolating analytics tasks from the core matchmaking path.

---

### Step 1: Enable DynamoDB Streams
1. Navigate to **Amazon DynamoDB** -> **Tables** -> select table `ActiveMatches`.
2. Open the **Exports and streams** tab -> **DynamoDB stream details**.
3. Click **Turn on**, select **New and old images**, and click **Turn on stream**.

---

### Step 2: Create IAM Role for MatchAnalytic Lambda
1. Navigate to **AWS IAM** -> **Roles** -> **Create role**.
2. Select Trusted Entity: **AWS service** -> **Lambda**.
3. Name the role `MatchAnalyticLambdaRole`.
4. Attach an inline policy granting stream read permissions:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "dynamodb:GetRecords",
        "dynamodb:GetShardIterator",
        "dynamodb:DescribeStream",
        "dynamodb:ListStreams"
      ],
      "Resource": "arn:aws:dynamodb:ap-southeast-1:*:table/ActiveMatches/stream/*"
    }
  ]
}
```

5. Click **Create role**.

---

### Step 3: Provision & Connect MatchAnalytic Lambda
1. Navigate to **AWS Lambda** -> **Create function**.
2. Set Function name to `MatchAnalyticLambda`, attach IAM Role `MatchAnalyticLambdaRole`.
3. Click **Add trigger**, select **DynamoDB**.
4. Select table `ActiveMatches`, Batch size: `100`, Starting position: **Latest**. Click **Add**.

![Create MatchAnalytic Lambda & DynamoDB Stream Trigger](/images/5-Workshop/img_B/image8.png)

5. Paste the post-match log parser code under **Code** and click **Deploy**.
6. When a game match ends and updates DynamoDB, DynamoDB Streams trigger `MatchAnalyticLambda` to process and stream telemetry data directly to S3/analytics stores without incurring RTT latency overhead on active players.
