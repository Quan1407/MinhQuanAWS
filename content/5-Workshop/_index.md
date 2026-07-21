---
title: "Workshop"
date: 2026-07-21
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Step-by-Step Guide: Building a Serverless & Event-Driven Game Backend on AWS

#### Workshop Overview

In this hands-on workshop, we will step-by-step deploy a complete cloud infrastructure for a **Live-Service Game Backend** on AWS. The system utilizes a Serverless & Event-Driven architecture, maximizing cost savings by only spinning up EC2 Spot instances during active game sessions.

#### Workshop Objectives:
1.  **Player Authentication**: Configure Amazon Cognito User Pool for user authentication and Amazon Cognito Identity Pool to delegate scoped temporary IAM credentials for direct S3 asset/patch downloads.
2.  **Match State Storage**: Provision Amazon DynamoDB tables (`MatchmakingQueue` and `ActiveMatches`) optimized for high-throughput match queues.
3.  **Matchmaking Pipeline**: Develop the AWS Lambda Matchmaker logic to handle `POST /join` and `GET /check` endpoints, exposed via API Gateway REST API secured by Cognito Authorizers and CORS.
4.  **Game Server Fleet & GitOps Automation**: Provision an EC2 Ubuntu 24.04 LTS Game Server, bake custom AMIs, configure Launch Templates for EC2 Spot Fleets with ASG Warm Pools, and set up GitHub OIDC with AWS CodeDeploy for automated CI/CD pipelines.
5.  **Asynchronous Post-Match Analytics**: Enable DynamoDB Streams and implement an Async Lambda worker to process post-match logs without adding latency to matchmaking.

---

#### Workshop Agenda:

1. [5.1. Workshop Overview](5.1-workshop-overview/)
2. [5.2. Prerequisites & Region Setup](5.2-prerequiste/)
3. [5.3. Amazon Cognito & DynamoDB Tables Setup](5.3-cognito-dynamodb/)
4. [5.4. Lambda Matchmaker & API Gateway REST API Deployment](5.4-matchmaker-api/)
5. [5.5. EC2 Spot Fleet, Launch Template & GitOps CodeDeploy Setup](5.5-ec2-fleet-gitops/)
6. [5.6. Asynchronous Post-Match Analytics with DynamoDB Streams](5.6-async-analytics/)
7. [5.7. Resource Cleanup](5.7-cleanup/)