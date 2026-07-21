---
title: "Workshop"
date: 2026-07-21
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Hướng dẫn chi tiết Xây dựng Serverless & Event-Driven Game Backend trên AWS

#### Tổng quan bài Thực hành (Workshop)

Trong bài thực hành này, chúng ta sẽ từng bước triển khai hạ tầng đám mây hoàn chỉnh cho một hệ thống **Game Live-Service** trên AWS. Hệ thống vận hành theo cơ chế Serverless & Event-Driven, giúp tối ưu chi phí hạ tầng tối đa nhờ chỉ khởi chạy tài nguyên máy chủ EC2 Spot khi có phiên trận đấu thực tế.

#### Mục tiêu đạt được:
1.  **Xác thực người chơi (Player Authentication)**: Khởi tạo Amazon Cognito User Pool để xác thực tài khoản và Amazon Cognito Identity Pool để cấp phát IAM Temporary Credentials cho client tải asset/patch từ S3.
2.  **Lưu trữ trạng thái trận đấu (Match State Storage)**: Khởi tạo các bảng DynamoDB (`MatchmakingQueue` và `ActiveMatches`) theo thiết kế tối ưu cho lưu lượng cao.
3.  **Hệ thống ghép trận (Matchmaking Pipeline)**: Viết mã nguồn AWS Lambda Matchmaker xử lý request `POST /join` và `GET /check`, tạo REST API Gateway được bảo vệ bởi Cognito Authorizer và kích hoạt CORS.
4.  **Tự động hóa Fleet máy chủ Game (Game Server Fleet & GitOps)**: Khởi tạo EC2 Ubuntu 24.04 LTS Game Server, tạo AMI, Launch Template cho EC2 Spot Fleet và ASG Warm Pool; cấu hình GitHub OIDC Provider và AWS CodeDeploy cho luồng CI/CD tự động.
5.  **Xử lý phân tích sau trận đấu (Asynchronous Analytics)**: Kích hoạt DynamoDB Streams và viết Async Lambda thu thập dữ liệu sau trận mà không ảnh hưởng tới độ trễ ghép trận.

---

#### Danh sách các bước triển khai:

1. [5.1. Tổng quan bài Workshop](5.1-workshop-overview/)
2. [5.2. Chuẩn bị môi trường & Chọn Region](5.2-prerequiste/)
3. [5.3. Khởi tạo Amazon Cognito & DynamoDB Tables](5.3-cognito-dynamodb/)
4. [5.4. Xây dựng Lambda Matchmaker & API Gateway REST API](5.4-matchmaker-api/)
5. [5.5. Cấu hình EC2 Spot Fleet, Launch Template & GitOps CodeDeploy](5.5-ec2-fleet-gitops/)
6. [5.6. Xử lý Asynchronous Post-Match với DynamoDB Streams & Analytics Lambda](5.6-async-analytics/)
7. [5.7. Dọn dẹp tài nguyên](5.7-cleanup/)