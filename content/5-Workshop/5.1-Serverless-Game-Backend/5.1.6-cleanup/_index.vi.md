---
title: "Dọn dẹp tài nguyên"
date: 2026-07-21
weight: 6
chapter: false
pre: " <b> 5.1.6. </b> "
---

# 5.1.6. Dọn dẹp Tài nguyên (Resource Cleanup)

Để tránh phát sinh chi phí duy trì không mong muốn trên tài khoản AWS sau khi hoàn thành bài thực hành, bạn cần thực hiện dọn dẹp và xóa toàn bộ các dịch vụ đã khởi tạo theo các bước hướng dẫn chi tiết dưới đây:

---

### Bước 1: Dọn dẹp Amazon Cognito

1. Truy cập dịch vụ **Amazon Cognito** -> chọn **User pools**.
2. Chọn **User pool** đã tạo (Ví dụ: `ap-southeast-1_phYoaMUPC`), chọn **Delete** và xác nhận xóa.

![Xóa Cognito User Pool](/images/5-Workshop/cleanup/image1.png)

![Xác nhận xóa Cognito User Pool](/images/5-Workshop/cleanup/image2.png)

3. Chuyển sang mục **Identity pools**, chọn **Identity pool** đã tạo (Ví dụ: `FightingGameIdentityPool`), chọn **Delete** và xác nhận xóa.

![Xóa Cognito Identity Pool](/images/5-Workshop/cleanup/image3.png)

![Xác nhận xóa Cognito Identity Pool](/images/5-Workshop/cleanup/image4.png)

---

### Bước 2: Dọn dẹp Amazon DynamoDB

1. Truy cập dịch vụ **Amazon DynamoDB** -> chọn **Tables**.
2. Chọn tất cả các bảng đã tạo trong bài thực hành (`MatchmakingQueue` và `ActiveMatches`), nhấn nút **Delete** và xác nhận xóa toàn bộ bảng.

![Chọn các bảng DynamoDB để xóa](/images/5-Workshop/cleanup/image5.png)

![Xác nhận xóa các bảng DynamoDB](/images/5-Workshop/cleanup/image6.png)

---

### Bước 3: Dọn dẹp AWS Lambda Functions

1. Truy cập dịch vụ **AWS Lambda** -> chọn **Functions**.
2. Chọn tất cả các hàm Lambda đã khởi tạo (`FightingGameMatchmaker` và `MatchAnalyticLambda`), nhấn **Actions** -> chọn **Delete** và xác nhận xóa.

![Chọn các hàm Lambda để xóa](/images/5-Workshop/cleanup/image7.png)

![Xác nhận xóa các hàm Lambda](/images/5-Workshop/cleanup/image8.png)

---

### Bước 4: Dọn dẹp Amazon API Gateway

1. Truy cập dịch vụ **Amazon API Gateway** -> chọn **APIs**.
2. Chọn API đã khởi tạo (`FightingGameAPI`), chọn **Delete** và nhập xác nhận để xóa hoàn toàn API Gateway.

![Chọn API Gateway để xóa](/images/5-Workshop/cleanup/image9.png)

![Xác nhận xóa API Gateway](/images/5-Workshop/cleanup/image10.png)

---

### Bước 5: Dọn dẹp Amazon CloudFront Distribution

1. Truy cập dịch vụ **Amazon CloudFront** -> chọn **Distributions**.
2. Chọn Distribution đã khởi tạo cho ứng dụng, nhấn **Disable** để vô hiệu hóa phân phối.

![Vô hiệu hóa CloudFront Distribution](/images/5-Workshop/cleanup/image11.png)

![Xác nhận vô hiệu hóa CloudFront Distribution](/images/5-Workshop/cleanup/image12.png)

3. **Lưu ý**: Đợi khoảng 5 - 10 phút để quá trình vô hiệu hóa (Disabled) hoàn tất trên toàn mạng lưới Edge Location của AWS. Sau đó, nút **Delete** sẽ sáng lên và bạn nhấn **Delete** để xóa Distribution.

![Xóa CloudFront Distribution sau khi vô hiệu hóa](/images/5-Workshop/cleanup/image13.png)

---

### Bước 6: Dọn dẹp AWS WAF & Shield

1. Truy cập dịch vụ **AWS WAF & Shield** -> chọn **Web ACLs**.
2. Chọn Web ACL đã tạo (Ví dụ: `CreatedByCloudFront-56a8180e`), nhấn **Actions** -> chọn **Manage resources**.
3. Thực hiện **Disassociate** để hủy liên kết Web ACL khỏi CloudFront Distribution, sau đó nhấn **Delete** để xóa hẳn Web ACL.

![Quản lý liên kết Web ACL](/images/5-Workshop/cleanup/image14.png)

![Xóa Web ACL trong AWS WAF](/images/5-Workshop/cleanup/image15.png)

---

> [!TIP]
> Việc dọn dẹp toàn bộ tài nguyên theo các bước trên đảm bảo tài khoản AWS của bạn không phát sinh bất kỳ khoản phí ngoài ý muốn nào sau bài thực hành.
