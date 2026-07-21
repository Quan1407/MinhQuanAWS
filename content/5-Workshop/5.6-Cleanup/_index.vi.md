---
title: "Asynchronous Analytics"
date: 2026-07-21
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

# Xử lý Asynchronous Analytics với DynamoDB Streams & Lambda

Trong phần này, chúng ta sẽ cấu hình **DynamoDB Streams** trên bảng DynamoDB và viết hàm **MatchAnalytic Lambda** để thu thập log, xử lý thống kê và phân tích kết quả sau trận đấu một cách hoàn toàn bất đồng bộ (Asynchronous Event Processing), không gây bất kỳ độ trễ nào cho luồng ghép trận.

---

### Bước 1: Kích hoạt DynamoDB Streams
1. Truy cập dịch vụ **Amazon DynamoDB** -> **Tables** -> chọn bảng `ActiveMatches`.
2. Chọn tab **Exports and streams** -> chọn **DynamoDB stream details**.
3. Bấm **Turn on**, chọn **New and old images** (Lưu lại cả trạng thái trước và sau khi thay đổi). Bấm **Turn on stream**.

---

### Bước 2: Tạo IAM Role cho MatchAnalytic Lambda
1. Truy cập **AWS IAM** -> **Roles** -> chọn **Create role**.
2. Chọn Trusted Entity: **AWS service** -> **Lambda**.
3. Đặt tên Role: `MatchAnalyticLambdaRole`.
4. Tạo Policy đính kèm cho phép Lambda đọc dữ liệu từ DynamoDB Stream:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "dynamodb:GetRecords",
        "dynamodb:GetShardIterator",
        "dynamodb:DescribeStream",
        "dynamodb:ListStreams"
      ],
      "Resource": "arn:aws:dynamodb:ap-southeast-1:*:table/ActiveMatches/stream/*"
    }
  ]
}
```

5. Bấm **Create role**.

---

### Bước 3: Tạo & Kết nối MatchAnalytic Lambda
1. Truy cập **AWS Lambda** -> **Create function**.
2. Đặt tên hàm: `MatchAnalyticLambda`, chọn IAM Role `MatchAnalyticLambdaRole`.
3. Nhấn **Add trigger**, chọn **DynamoDB**.
4. Chọn DynamoDB table: `ActiveMatches`, Batch size: `100`, Starting position: **Latest**. Bấm **Add**.

![Tạo MatchAnalytic Lambda & DynamoDB Stream Trigger](/images/5-Workshop/img_B/image8.png)

5. Dán mã nguồn xử lý phân tích log và kết quả trận đấu vào tab **Code** và chọn **Deploy**.
6. Khi một trận đấu kết thúc và kết quả được ghi vào DynamoDB, DynamoDB Stream tự động kích hoạt `MatchAnalyticLambda` thu thập và đẩy log sang Data Lake/Analytics Store mà không ảnh hưởng tới độ trễ ghép trận RTT.