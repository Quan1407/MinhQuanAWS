---
title: "Workshop"
date: 2026-07-21
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Step-by-Step Guide: Building a Serverless & Event-Driven Game Backend on AWS

#### Workshop Overview

In this section, we will construct the hands-on workshop **Building a Serverless & Event-Driven Game Backend on AWS**, structured under Chapter **5.1** and detailed sub-sections **5.1.x**.

#### Objectives:
1.  **Player Authentication**: Configure Amazon Cognito User Pools and Identity Pools.
2.  **Match State Storage**: Provision Amazon DynamoDB tables `MatchmakingQueue` and `ActiveMatches`.
3.  **Matchmaking Pipeline**: Implement AWS Lambda Matchmaker and API Gateway REST API with Cognito Authorizers and CORS.
4.  **Game Fleet & GitOps**: Create EC2 Game Servers, custom AMIs, Launch Templates for EC2 Spot Fleets with ASG Warm Pools, and GitHub OIDC with AWS CodeDeploy.
5.  **Asynchronous Analytics**: Enable DynamoDB Streams and Async Lambda.

---

#### Agenda:

1. [5.1. Building a Serverless & Event-Driven Game Backend on AWS](5.1-serverless-game-backend/)
   * [5.1.1. Prerequisites & Region Setup](5.1-serverless-game-backend/5.1.1-prerequiste/)
   * [5.1.2. Amazon Cognito & DynamoDB Tables Setup](5.1-serverless-game-backend/5.1.2-cognito-dynamodb/)
   * [5.1.3. Lambda Matchmaker & API Gateway REST API Deployment](5.1-serverless-game-backend/5.1.3-matchmaker-api/)
   * [5.1.4. EC2 Spot Fleet, Launch Template & GitOps CodeDeploy Setup](5.1-serverless-game-backend/5.1.4-ec2-fleet-gitops/)
   * [5.1.5. Asynchronous Analytics with DynamoDB Streams & Lambda](5.1-serverless-game-backend/5.1.5-async-analytics/)
   * [5.1.6. Resource Cleanup](5.1-serverless-game-backend/5.1.6-cleanup/)
