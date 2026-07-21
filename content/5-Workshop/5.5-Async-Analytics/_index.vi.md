---
title: "Asynchronous Analytics"
date: 2026-07-21
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

# 5.5. Xử lý Asynchronous Analytics với DynamoDB Streams & Lambda

Trong chương này, chúng ta sẽ cấu hình **DynamoDB Streams** trên bảng DynamoDB và viết hàm **MatchAnalytic Lambda** để thu thập log, xử lý thống kê và phân tích kết quả sau trận đấu một cách hoàn toàn bất đồng bộ (Asynchronous Event Processing), không gây bất kỳ độ trễ nào cho luồng ghép trận.

---

### Danh sách các bài học chi tiết:

* **[5.5.1. Kích hoạt DynamoDB Streams](5.5.1-dynamodb-streams/)**
* **[5.5.2. Tạo & Kết nối MatchAnalytic Lambda](5.5.2-match-analytic-lambda/)**
