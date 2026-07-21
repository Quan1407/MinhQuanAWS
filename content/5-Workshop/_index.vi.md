---
title: "Workshop"
date: 2026-07-21
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Hướng dẫn chi tiết Xây dựng Serverless & Event-Driven Game Backend trên AWS

#### Tổng quan bài Thực hành (Workshop)

Trong phần này, chúng ta sẽ thực hiện xây dựng bài Workshop về **Xây dựng Serverless & Event-Driven Game Backend trên AWS** được cấu trúc theo chương **5.1** và các phần chi tiết **5.1.x**.

#### Mục tiêu đạt được:
1.  **Xác thực người chơi (Player Authentication)**: Khởi tạo Amazon Cognito User Pool & Identity Pool.
2.  **Lưu trữ hàng đợi & Trận đấu**: Khởi tạo 2 bảng DynamoDB `MatchmakingQueue` và `ActiveMatches`.
3.  **Hệ thống ghép trận (Matchmaking Pipeline)**: Viết mã nguồn AWS Lambda Matchmaker, tạo REST API Gateway tích hợp Cognito Authorizer & CORS.
4.  **Tự động hóa Fleet máy chủ Game (EC2 Spot Fleet & GitOps)**: Khởi tạo EC2 Game Server, AMI, Launch Template cho EC2 Spot Fleet với ASG Warm Pool, cấu hình GitHub OIDC & AWS CodeDeploy.
5.  **Xử lý phân tích sau trận đấu (Asynchronous Analytics)**: Kích hoạt DynamoDB Streams và Async Lambda.

---

#### Danh sách nội dung thực hành:

1. [5.1. Xây dựng Serverless & Event-Driven Game Backend trên AWS](5.1-serverless-game-backend/)
   * [5.1.1. Chuẩn bị môi trường & Thiết lập Region](5.1-serverless-game-backend/5.1.1-prerequiste/)
   * [5.1.2. Khởi tạo Amazon Cognito & DynamoDB Tables](5.1-serverless-game-backend/5.1.2-cognito-dynamodb/)
   * [5.1.3. Xây dựng Lambda Matchmaker & API Gateway REST API](5.1-serverless-game-backend/5.1.3-matchmaker-api/)
   * [5.1.4. Cấu hình EC2 Spot Fleet, Launch Template & GitOps CodeDeploy](5.1-serverless-game-backend/5.1.4-ec2-fleet-gitops/)
   * [5.1.5. Xử lý Asynchronous Analytics với DynamoDB Streams & Lambda](5.1-serverless-game-backend/5.1.5-async-analytics/)
   * [5.1.6. Dọn dẹp tài nguyên](5.1-serverless-game-backend/5.1.6-cleanup/)
