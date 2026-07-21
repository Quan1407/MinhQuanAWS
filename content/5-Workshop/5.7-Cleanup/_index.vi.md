---
title: "Dọn dẹp tài nguyên"
date: 2026-07-21
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

# Dọn dẹp Tài nguyên (Resource Cleanup)

Để tránh phát sinh chi phí duy trì trên tài khoản AWS sau khi hoàn thành bài thực hành, bạn cần thực hiện dọn dẹp các tài nguyên theo thứ tự dưới đây:

---

### Các bước dọn dẹp:

1. **Xóa EC2 Auto Scaling Group & Instances**:
   * Truy cập **Amazon EC2** -> **Auto Scaling Groups** -> chọn ASG đã tạo và nhấn **Delete**.
   * Chờ ASG hủy toàn bộ máy chủ EC2 Spot và Warm Pool.
   * Vào **Launch Templates** và xóa Launch Template.

2. **Xóa Amazon API Gateway**:
   * Truy cập **API Gateway** -> chọn `FightingGameAPI` -> chọn **Actions** -> **Delete**.

3. **Xóa AWS Lambda Functions**:
   * Truy cập **AWS Lambda** -> chọn các hàm `FightingGameMatchmaker` và `MatchAnalyticLambda` -> nhấn **Delete**.

4. **Xóa Amazon DynamoDB Tables**:
   * Truy cập **DynamoDB** -> **Tables** -> chọn `MatchmakingQueue` và `ActiveMatches` -> chọn **Delete table**.

5. **Xóa Amazon Cognito User Pool & Identity Pool**:
   * Truy cập **Cognito** -> **Identity pools** -> xóa `FightingGameIdentityPool`.
   * Vào **User pools** -> xóa User Pool `ap-southeast-1_phYoaMUPC`.

6. **Xóa Amazon S3 Bucket**:
   * Truy cập **S3** -> chọn Bucket lưu trữ asset -> chọn **Empty** để xóa sạch dữ liệu, sau đó nhấn **Delete**.

7. **Xóa IAM Roles & CodeDeploy Applications**:
   * Xóa CodeDeploy application `FightingGameServerApp`.
   * Vào **IAM** -> **Roles** -> xóa các IAM Roles đã tạo trong bài lab.
