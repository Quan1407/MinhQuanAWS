---
title: "EC2 Spot Fleet & GitOps CodeDeploy"
date: 2026-07-21
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

# Provisioning EC2 Spot Fleet, Launch Template & GitOps CodeDeploy

In this section, we will create a sample **Amazon EC2 (Ubuntu 24.04 LTS)** Game Server, bake a custom AMI, set up a **Launch Template** for an EC2 Spot Fleet with an **ASG Warm Pool**, configure **S3 Static Website Hosting**, and implement a **GitOps CI/CD pipeline** using **GitHub OIDC** and **AWS CodeDeploy**.

---

### Part 1: Provisioning Sample EC2 Game Server & Baking AMI

1. Navigate to **Amazon EC2** and click **Launch instance**.
2. Configure settings:
   * **Name**: `FightingGameServer`
   * **AMI**: Ubuntu Server 24.04 LTS (64-bit ARM / x86)
   * **Instance type**: `t3.small` or `t3.medium`
   * **Key pair**: Select or create a key pair
   * **Storage**: 8 GB gp3
   * **Security Group**: Allow SSH (Port 22) and Game Server Ports (Port 3000/UDP/TCP).

![Launch EC2 FightingGameServer](/images/5-Workshop/img_A/image88.png)

3. Click **Launch instance**. Configure Node.js Game Server software on the instance.

![Configure Node.js Game Server](/images/5-Workshop/img_A/image91.png)

4. **Bake Custom AMI**:
   * Select instance `FightingGameServer`, navigate to **Actions** -> **Image and templates** -> **Create image**.
   * Name the image `FightingGameServerAMI` and click **Create image**.

![Bake AMI from Instance](/images/5-Workshop/img_B/image2.png)

---

### Part 2: Creating Launch Template & Auto Scaling Group Warm Pool

1. Navigate to **Launch Templates** -> **Create launch template**.
2. Select AMI `FightingGameServerAMI`, enable **Spot Instance**, and attach IAM Instance Profile `FightingGameServerInstanceRole` allowing S3 asset fetching on boot.

![Create Spot Launch Template](/images/5-Workshop/img_B/image3.png)

3. Create an **Auto Scaling Group (ASG)** using the Launch Template and enable the **Warm Pool** feature to maintain pre-initialized, warm instances ready for instant spin-up upon matchmaking requests.

![Configure ASG Warm Pool](/images/5-Workshop/img_B/image4.png)

---

### Part 3: Provisioning S3 Bucket & Static Website Hosting

1. Navigate to **Amazon S3** and click **Create bucket**.
2. Set bucket name (e.g., `fighting-game-assets-singapore`).
3. Enable **Static website hosting** and configure **Bucket Policy** (public read for client assets) and **CORS Policy** to permit browser API requests.

![Configure S3 Static Website Hosting & CORS](/images/5-Workshop/img_B/image6.png)

---

### Part 4: Configuring GitHub OIDC & AWS CodeDeploy GitOps Pipeline

1. **Add GitHub Provider to AWS IAM (No long-lived access keys)**:
   * Navigate to **IAM** -> **Identity providers** -> **Add provider**.
   * Select **OpenID Connect**, enter Provider URL: `https://token.actions.githubusercontent.com` and Audience: `sts.amazonaws.com`.

![Add GitHub OIDC Provider](/images/5-Workshop/img_B/image7.png)

2. **Install AWS CodeDeploy Agent on EC2 Game Server**:
   Execute the CodeDeploy installation script on Ubuntu 24.04 LTS:

```bash
# 1. Install prerequisites
sudo apt-get update && sudo apt-get install -y ruby-full ruby-webrick wget gdebi-core

# 2. Download raw .deb package directly
cd /tmp
wget https://aws-codedeploy-ap-southeast-1.s3.ap-southeast-1.amazonaws.com/releases/codedeploy-agent_1.8.1-26_all.deb

# 3. Unpack, fix Ruby dependency declaration, and repack
dpkg-deb -R codedeploy-agent_1.8.1-26_all.deb /tmp/codedeploy-extracted
sed -i "s/ruby3.2/ruby3.3/g" /tmp/codedeploy-extracted/DEBIAN/control
dpkg-deb -b /tmp/codedeploy-extracted /tmp/codedeploy-agent_fixed.deb

# 4. Install patched package and start service
sudo dpkg -i /tmp/codedeploy-agent_fixed.deb
sudo systemctl enable codedeploy-agent
sudo systemctl start codedeploy-agent
sudo systemctl status codedeploy-agent
```

![CodeDeploy Agent Installation Success](/images/5-Workshop/img_B/image8.png)

3. **Provision CodeDeploy Application & Deployment Group**:
   * Navigate to **AWS CodeDeploy** -> **Applications** -> **Create application**.
   * Application name: `FightingGameServerApp`, Compute platform: **EC2/On-premises**.
   * Create **Deployment group**, attach CodeDeploy IAM role, and map to the EC2 Spot Fleet.

![Create CodeDeploy Deployment Group](/images/5-Workshop/img_B/image9.png)

4. Pushing code to GitHub triggers GitHub Actions to execute CodeDeploy jobs, deploying new builds to the game fleet with zero downtime.

![Successful CodeDeploy Job](/images/5-Workshop/img_B/image10.png)
