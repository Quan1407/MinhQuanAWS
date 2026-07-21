---
title: "Cognito & DynamoDB Setup"
date: 2026-07-21
weight: 2
chapter: false
pre: " <b> 5.1.2. </b> "
---

# 5.1.2. Amazon Cognito & DynamoDB Tables Setup

In this section, we will configure **Amazon Cognito User Pool & Identity Pool** to manage player authentication and grant temporary S3 asset access. Afterwards, we will create two **Amazon DynamoDB** tables for match queues (`MatchmakingQueue`) and active game sessions (`ActiveMatches`).

---

### Part 1: Provisioning Amazon Cognito User Pool
1. In the AWS Console search bar, type **Cognito** and select **Amazon Cognito**.

![Search Cognito](/images/5-Workshop/img_A/image3.png)

![Amazon Cognito Console](/images/5-Workshop/img_A/image4.png)

2. Select **Single-page application (SPA)** and set the application name to `FightingGame`.

![Configure SPA Application](/images/5-Workshop/img_A/image7.png)

3. Under **Username**, check **Enable Self-registration** to allow players to register.
4. Under **Required attributes for sign-up**, select **email**.

![Configure Registration Attributes](/images/5-Workshop/img_A/image8.png)

5. Click **Create user directory**. Once created:
   * **User pool ID**: `ap-southeast-1_phYoaMUPC`
   * Under **App clients**, select `FightingGame` to view the **Client ID** (e.g., `73ipqvvo7h3u0j3elfqlj23jo3`).

![View User Pool ID](/images/5-Workshop/img_A/image11.png)

![View App Client ID](/images/5-Workshop/img_A/image12.png)

6. Click **Edit** under App client `FightingGame`, enable `ALLOW_USER_PASSWORD_AUTH`, and click **Save changes**.

![Enable ALLOW_USER_PASSWORD_AUTH](/images/5-Workshop/img_A/image14.png)

---

### Part 2: Provisioning Amazon Cognito Identity Pool
1. Return to the main Cognito console, navigate to **Identity pools**, and click **Create identity pool**.

![Create Identity Pool](/images/5-Workshop/img_A/image17.png)

2. Select **Authenticated access** and choose **Amazon Cognito user pool** as the provider. Click **Next**.

![Configure Authenticated Access](/images/5-Workshop/img_A/image19.png)

3. Under **Configure permissions**, choose **Create a new IAM Role**, set the IAM role name to `FightingGameAuthenticatedRole`, and click **Next**.

![Create IAM Role](/images/5-Workshop/img_A/image20.png)

4. Enter your **User pool ID** and **App Client ID** to connect the Identity Provider. Click **Next**.

![Connect Identity Provider](/images/5-Workshop/img_A/image21.png)

5. Set the Identity Pool Name to `FightingGameIdentityPool` and click **Create identity pool**.
   * **Identity pool ID**: `ap-southeast-1:a5d743b9-e4a4-45d2-9cb1-9d214cee574c`

![View Identity Pool ID](/images/5-Workshop/img_A/image24.png)

---

### Part 3: Provisioning Amazon DynamoDB Tables

We will create two DynamoDB tables to track matchmaking states:

#### Table 1: `MatchmakingQueue`
1. Navigate to **DynamoDB** and click **Create table**.
2. Configure settings:
   * **Table name**: `MatchmakingQueue`
   * **Partition key**: `playerId` (Data type: **String**)
   * **Sort key**: Leave empty
   * **Table settings**: Keep **Default settings**
3. Scroll down and click **Create table**.

![Create MatchmakingQueue Table](/images/5-Workshop/img_A/image27.png)

#### Table 2: `ActiveMatches`
1. Click **Create table** again.
2. Configure settings:
   * **Table name**: `ActiveMatches`
   * **Partition key**: `playerId` (Data type: **String**)
   * **Sort key**: Leave empty
   * **Table settings**: Keep **Default settings**
3. Click **Create table**.

![Create ActiveMatches Table](/images/5-Workshop/img_A/image28.png)

4. Verify table list: Both `MatchmakingQueue` and `ActiveMatches` display **Active** status.

![Active DynamoDB Tables](/images/5-Workshop/img_A/image29.png)
