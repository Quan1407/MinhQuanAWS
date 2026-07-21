---
title: "Building Serverless Game Backend"
date: 2026-07-21
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

# 5.1. Building a Serverless & Event-Driven Game Backend on AWS

### Chapter 5.1 Overview
Chapter **5.1** provides step-by-step instructions to provision the full cloud infrastructure for a Live-Service Game Backend on AWS. The detailed hands-on lab modules are organized under **5.1.x** sub-chapters below:

![Architecture Overview](/images/2-Proposal/serverless_game_backend_architecture.png)

---

### Hands-on Lab Agenda (5.1.x):

* **[5.1.1. Prerequisites & Region Setup](5.1.1-prerequiste/)**: Console login and Singapore `ap-southeast-1` region selection.
* **[5.1.2. Amazon Cognito & DynamoDB Tables Setup](5.1.2-cognito-dynamodb/)**: Provision User Pools, Identity Pools, and `MatchmakingQueue`/`ActiveMatches` tables.
* **[5.1.3. Lambda Matchmaker & API Gateway REST API Deployment](5.1.3-matchmaker-api/)**: Implement Lambda matchmaking handler, API Gateway REST API, Cognito Authorizer, and CORS.
* **[5.1.4. EC2 Spot Fleet, Launch Template & GitOps CodeDeploy Setup](5.1.4-ec2-fleet-gitops/)**: Create EC2 Game Servers, custom AMIs, Spot Launch Templates with ASG Warm Pools, S3 Static Websites, and GitHub OIDC with CodeDeploy.
* **[5.1.5. Asynchronous Analytics with DynamoDB Streams & Lambda](5.1.5-async-analytics/)**: Enable DynamoDB Streams and Async Lambda workers for post-match telemetry.
* **[5.1.6. Resource Cleanup](5.1.6-cleanup/)**: Teardown instructions to prevent unnecessary AWS charges.
