---
title: "Workshop Overview"
date: 2026-07-21
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

# Workshop Overview: Building a Serverless Game Backend on AWS

### Introduction
In modern live-service game development, infrastructure cost optimization and seamless scalability are critical factors for success. Traditional architectures that maintain 24/7 dedicated game server fleets incur massive idle costs during off-peak hours when active player counts drop.

This workshop provides a comprehensive step-by-step guide to constructing a production-ready **Serverless & Event-Driven Game Backend** on AWS.

![Architecture Diagram](/images/2-Proposal/serverless_game_backend_architecture.png)

### Key Architecture Components:
*   **Player Authentication**: Uses **Amazon Cognito User Pools** for JWT authentication and **Amazon Cognito Identity Pools** to grant scoped temporary IAM credentials for direct asset downloads from S3.
*   **Match Queue & State Storage**: Leverages **Amazon DynamoDB** with two optimized tables: `MatchmakingQueue` and `ActiveMatches`.
*   **Matchmaking API Pipeline**: Implements an **AWS Lambda Matchmaker** inside a Private Subnet, connecting to DynamoDB and EC2 APIs via **VPC Endpoints**, exposed through **Amazon API Gateway** (REST API) secured by **Cognito Authorizers** and **AWS WAF**.
*   **Game Server Fleet & ASG**: Provisions game instances using **Amazon EC2 Spot Fleets (Graviton ARM64)** with **Auto Scaling Group Warm Pools**, pulling server bundles on boot from S3.
*   **Automated GitOps CI/CD**: Integrates **GitHub Actions OIDC** and **AWS CodeDeploy** for zero-downtime automated deployments of code, patches, and game server bundles.
*   **Asynchronous Post-Match Analytics**: Uses **DynamoDB Streams** to trigger **Async Lambda** functions for capturing post-match metrics without adding latency to matchmaking.

---

### Estimated Workshop Metrics:
*   **Duration**: 60 - 90 minutes.
*   **Difficulty Level**: Intermediate / Advanced.
*   **Estimated Cost**: Minimal (covered under AWS Free Tier or < $0.50 for EC2 Spot and Lambda/DynamoDB usage).