---
title: "Tổng quan về Workshop"
date: 2026-07-21
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

# Tổng quan bài Workshop: Xây dựng Serverless Game Backend trên AWS

### Giới thiệu
Trong môi trường phát triển game hiện đại (đặc biệt là thể loại Live-Service Games), chi phí hạ tầng máy chủ và khả năng tự động mở rộng (scalability) là hai yếu tố sống còn. Mô hình truyền thống duy trì cụm máy chủ game chạy 24/7 gây lãng phí chi phí cực kỳ lớn trong những khoảng thời gian thấp điểm khi số lượng người chơi giảm.

Bài workshop này sẽ hướng dẫn bạn chi tiết từng bước xây dựng một hệ thống **Serverless & Event-Driven Game Backend** hoàn chỉnh trên AWS.

![Khai quát mô hình kiến trúc](/images/2-Proposal/serverless_game_backend_architecture.png)

### Nội dung kiến trúc chính:
*   **Xác thực người chơi (Player Authentication)**: Sử dụng **Amazon Cognito User Pool** để cấp phát JWT Token và **Amazon Cognito Identity Pool** để trao Temporary IAM Credentials scoped theo prefix trên Amazon S3.
*   **Lưu trữ hàng đợi & Trận đấu**: Sử dụng **Amazon DynamoDB Single Table** với 2 bảng `MatchmakingQueue` (hàng đợi ghép trận) và `ActiveMatches` (các trận đấu đang diễn ra).
*   **Hệ thống ghép trận (Matchmaking API)**: Xây dựng hàm **AWS Lambda Matchmaker** đặt trong Private Subnet, kết nối DynamoDB và EC2 API qua **VPC Endpoints**, tiếp nhận request qua **Amazon API Gateway** (REST API) tích hợp **Cognito Authorizer** và **AWS WAF**.
*   **Fleet máy chủ Game (EC2 Spot Fleet & ASG)**: Khởi tạo cụm máy chủ game trên **Amazon EC2 Spot Instances (Graviton ARM64)** tích hợp **Auto Scaling Group Warm Pool**, tự động tải game bundle từ S3 khi khởi động.
*   **Tự động hóa triển khai (GitOps CI/CD)**: Tích hợp **GitHub Actions OIDC** và **AWS CodeDeploy** để tự động cập nhật bản build, patch và game server bundle mà không gây gián đoạn dịch vụ (Zero-downtime).
*   **Xử lý bất đồng bộ sau trận (Async Post-Match Analytics)**: Sử dụng **DynamoDB Streams** để kích hoạt **Async Lambda** thu thập log và kết quả trận đấu sau khi kết thúc.

---

### Thời lượng ước tính:
*   **Thời gian thực hiện**: 60 - 90 phút.
*   **Cấp độ**: Intermediate / Advanced.
*   **Chi phí phát sinh**: Thấp (nằm trong Free Tier hoặc chi phí cực nhỏ cho EC2 Spot và Lambda/DynamoDB).
