---
title: "Chuẩn bị môi trường & Chọn Region"
date: 2026-07-21
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

# 5.1. Chuẩn bị Môi trường & Thiết lập Region

### Yêu cầu tiên quyết:
1.  **Tài khoản AWS (AWS Account)** có quyền quản trị (Administrator Access) hoặc IAM User có đầy đủ quyền thao tác trên Cognito, DynamoDB, Lambda, API Gateway, EC2, S3, CodeDeploy và IAM.
2.  **Trình duyệt web** (Google Chrome, Firefox, Safari hoặc Microsoft Edge).
3.  **Client/Kịch bản kiểm thử**: Postman, cURL hoặc ứng dụng Node.js để gửi request API.

---

### Bước 1: Đăng nhập Console và Chuyển Region
1. Truy cập [AWS Management Console](https://console.aws.amazon.com/) và đăng nhập tài khoản của bạn.
2. Trên góc trên bên phải thanh điều hướng, chọn Region **Asia Pacific (Singapore) - ap-southeast-1**.

![Chuyển Region sang Singapore](/images/5-Workshop/img_A/image1.png)

![Xác nhận Region Singapore](/images/5-Workshop/img_A/image2.png)

> [!NOTE]
> Tất cả các tài nguyên trong bài thực hành này (Cognito, DynamoDB, Lambda, API Gateway, EC2) sẽ được tạo thống nhất tại Region `ap-southeast-1` (Singapore) để đảm bảo độ trễ thấp nhất và tính nhất quán cho các đường liên kết mạng.
