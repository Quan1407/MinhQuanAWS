---
title: "Xây dựng Serverless Game Backend"
date: 2026-07-21
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

# 5.1. Xây dựng Serverless & Event-Driven Game Backend trên AWS

### Giới thiệu Chương 5.1
Chương **5.1** hướng dẫn chi tiết toàn bộ quy trình xây dựng hạ tầng cho hệ thống Game Live-Service trên AWS. Toàn bộ các bước thực hành nhỏ được chia thành các bài **5.1.x** như dưới đây:

![Khai quát mô hình kiến trúc](/images/2-Proposal/serverless_game_backend_architecture.png)

---

### Danh sách các bước thực hành 5.1.x:

* **[5.1.1. Chuẩn bị môi trường & Thiết lập Region](5.1.1-prerequiste/)**: Hướng dẫn đăng nhập Console và chọn chuyển vùng Singapore `ap-southeast-1`.
* **[5.1.2. Khởi tạo Amazon Cognito & DynamoDB Tables](5.1.2-cognito-dynamodb/)**: Cấu hình User Pool, Identity Pool và tạo 2 bảng `MatchmakingQueue`, `ActiveMatches`.
* **[5.1.3. Xây dựng Lambda Matchmaker & API Gateway REST API](5.1.3-matchmaker-api/)**: Viết hàm Lambda ghép trận, tạo REST API Gateway, thêm Cognito Authorizer và kích hoạt CORS.
* **[5.1.4. Cấu hình EC2 Spot Fleet, Launch Template & GitOps CodeDeploy](5.1.4-ec2-fleet-gitops/)**: Tạo EC2 Game Server, AMI, Launch Template Spot với ASG Warm Pool, cấu hình S3 Static Website và GitHub OIDC với CodeDeploy.
* **[5.1.5. Xử lý Asynchronous Analytics với DynamoDB Streams & Lambda](5.1.5-async-analytics/)**: Kích hoạt DynamoDB Streams và Async Lambda thu thập dữ liệu sau trận.
* **[5.1.6. Dọn dẹp tài nguyên](5.1.6-cleanup/)**: Hướng dẫn xóa các tài nguyên để tránh phát sinh chi phí.
