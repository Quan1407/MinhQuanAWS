---
title: "Lambda Matchmaker & API Gateway"
date: 2026-07-21
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

# Deploying AWS Lambda Matchmaker & API Gateway REST API

In this step, we will create the **AWS Lambda Matchmaker** function (`FightingGameMatchmaker`) handling the matchmaking logic (`POST /join` and `GET /check`), grant appropriate IAM DynamoDB permissions, and expose the endpoints via **Amazon API Gateway** protected by a **Cognito Authorizer** and **CORS**.

---

### Part 1: Provisioning AWS Lambda Matchmaker
1. Navigate to **AWS Lambda** and click **Create a function**.
2. Select **Author from scratch**, set Function Name to `FightingGameMatchmaker`, and select the Node.js Runtime.

![Create Lambda Function](/images/5-Workshop/img_A/image31.png)

3. Under **Configuration** -> **Permissions** -> click the IAM Role Name link to open the IAM Console.

![Configure Lambda Role](/images/5-Workshop/img_A/image33.png)

4. Click **Add permissions** -> **Create inline policy**.
5. Paste the JSON policy granting DynamoDB read/write access to `MatchmakingQueue` and `ActiveMatches`:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "dynamodb:PutItem",
        "dynamodb:GetItem",
        "dynamodb:DeleteItem",
        "dynamodb:Scan"
      ],
      "Resource": [
        "arn:aws:dynamodb:ap-southeast-1:*:table/MatchmakingQueue",
        "arn:aws:dynamodb:ap-southeast-1:*:table/ActiveMatches"
      ]
    }
  ]
}
```

6. Name the policy and click **Create policy**. Verify that the policy is attached.

![Policy Attached to Lambda Role](/images/5-Workshop/img_A/image40.png)

7. Back in the Lambda Console, navigate to **Configuration** -> **Environment variables** -> **Edit** and add:
   * `QUEUE_TABLE`: `MatchmakingQueue`
   * `MATCH_TABLE`: `ActiveMatches`

![Set Environment Variables](/images/5-Workshop/img_A/image41.png)

8. Navigate to **Code**, paste the matchmaking logic handler, and click **Deploy**.
9. Test the function using sample payloads for Player 1 and Player 2. Verify that `200 OK` is returned and player queue entries are processed.

![Player 1 Test Success](/images/5-Workshop/img_A/image46.png)

![Player 2 Test Success](/images/5-Workshop/img_A/image50.png)

---

### Part 2: Provisioning Amazon API Gateway

1. Navigate to **Amazon API Gateway** and click **Create API**.
2. Select **REST API** and click **Build**.

![Create REST API](/images/5-Workshop/img_A/image55.png)

3. Set API Name to `FightingGameAPI`, Endpoint Type to **Regional**, and click **Create API**.

![REST API Created](/images/5-Workshop/img_A/image58.png)

4. **Create `/join` Resource**:
   * Click **Create resource**, set Resource Name to `join`.
   * Create a **POST** method on `/join`, set Integration Type to **Lambda Function**, and link to `FightingGameMatchmaker`.

![Create /join Resource](/images/5-Workshop/img_A/image60.png)

![Link POST Method to Lambda](/images/5-Workshop/img_A/image65.png)

5. **Create `/check` Resource**:
   * Select root `/`, click **Create resource**, set Resource Name to `check`.
   * Create a **GET** method on `/check` linked to `FightingGameMatchmaker`.

![GET /check Created](/images/5-Workshop/img_A/image70.png)

6. **Create Cognito Authorizer**:
   * On the left sidebar, click **Authorizers** -> **Create authorizer**.
   * Set Name to `FightinggameCognitoAuthorizer`, Type to **Cognito**, select your User Pool, and set Token Source to `Authorization`. Click **Create authorizer**.

![Create Cognito Authorizer](/images/5-Workshop/img_A/image71.png)

7. **Attach Authorizer to Methods**:
   * Under `/join` (POST), click **Edit**, set **Authorization** to `FightinggameCognitoAuthorizer`, and click **Save**.
   * Repeat for `/check` (GET).

![Attach Cognito Authorizer](/images/5-Workshop/img_A/image74.png)

8. **Enable CORS**:
   * Select `/join`, click **Enable CORS**, check **Default 4xx, 5xx** and **POST**.
   * Repeat for `/check` (GET). Click **Save**.

![CORS Enabled](/images/5-Workshop/img_A/image81.png)

9. **Deploy API**:
   * Select root `/`, click **Deploy API**.
   * Set Stage name to `prod` and click **Deploy**.
   * Copy the generated **Invoke URL** (e.g., `https://6whg1d5qca.execute-api.ap-southeast-1.amazonaws.com/prod`).

![API Deployed Successfully](/images/5-Workshop/img_A/image85.png)
