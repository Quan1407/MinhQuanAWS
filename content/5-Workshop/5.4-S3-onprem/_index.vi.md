---
title: "Lambda Matchmaker & API Gateway"
date: 2026-07-21
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

# Xây dựng Lambda Matchmaker & API Gateway REST API

Trong phần này, chúng ta sẽ tạo hàm **AWS Lambda Matchmaker** (`FightingGameMatchmaker`) chứa logic xử lý hàng đợi ghép trận (`POST /join` và `GET /check`), phân quyền IAM Policy cho Lambda truy cập DynamoDB, và exposed API thông qua **Amazon API Gateway** với **Cognito Authorizer** và **CORS**.

---

### Phần 1: Khởi tạo Hàm AWS Lambda Matchmaker
1. Truy cập dịch vụ **AWS Lambda** và chọn **Create a function**.
2. Chọn **Author from scratch**, nhập tên hàm: `FightingGameMatchmaker` và chọn Runtime Node.js.

![Tạo hàm Lambda Matchmaker](/images/5-Workshop/img_A/image31.png)

3. Tại tab **Configuration** -> chọn **Permissions** -> nhấn vào liên kết IAM Role Name để mở giao diện IAM Management Console.

![Cấu hình Role cho Lambda](/images/5-Workshop/img_A/image33.png)

4. Chọn **Add permissions** -> chọn **Create inline policy**.
5. Nhập JSON Policy cấp quyền đọc/ghi trên các bảng DynamoDB `MatchmakingQueue` và `ActiveMatches`:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "dynamodb:PutItem",
        "dynamodb:GetItem",
        "dynamodb:DeleteItem",
        "dynamodb:Scan"
      ],
      "Resource": [
        "arn:aws:dynamodb:ap-southeast-1:*:table/MatchmakingQueue",
        "arn:aws:dynamodb:ap-southeast-1:*:table/ActiveMatches"
      ]
    }
  ]
}
```

6. Đặt tên Policy và nhấn **Create policy**. Kiểm tra lại IAM Role đã gắn thành công Policy.

![Policy đã gắn vào Lambda](/images/5-Workshop/img_A/image40.png)

7. Quay lại giao diện Lambda, chọn tab **Configuration** -> **Environment variables** -> chọn **Edit** để thêm các biến môi trường:
   * `QUEUE_TABLE`: `MatchmakingQueue`
   * `MATCH_TABLE`: `ActiveMatches`

![Thêm biến môi trường Environment Variables](/images/5-Workshop/img_A/image41.png)

8. Chọn tab **Code**, dán mã nguồn xử lý logic ghép trận và chọn **Deploy**.
9. Tiến hành kiểm thử (Test) hàm Lambda với dữ liệu mẫu cho Player 1 và Player 2. Kết quả trả về `200 OK` và người chơi được đưa vào hàng đợi thành công.

![Kiểm thử thành công Player 1](/images/5-Workshop/img_A/image46.png)

![Kiểm thử thành công Player 2 và ghép phòng thành công](/images/5-Workshop/img_A/image50.png)

---

### Phần 2: Khởi tạo & Cấu hình Amazon API Gateway

1. Truy cập dịch vụ **Amazon API Gateway** và chọn **Create API**.
2. Chọn **REST API** và nhấn **Build**.

![Tạo REST API](/images/5-Workshop/img_A/image55.png)

3. Điền thông tin API Name: `FightingGameAPI`, Endpoint Type: **Regional** và chọn **Create API**.

![Tạo REST API thành công](/images/5-Workshop/img_A/image58.png)

4. **Tạo Resource `/join`**:
   * Nhấn **Create resource**, nhập Resource Name: `join`.
   * Tạo method **POST** tại resource `/join`, chọn **Lambda Function** và liên kết với hàm `FightingGameMatchmaker`.

![Tạo Resource /join](/images/5-Workshop/img_A/image60.png)

![Liên kết Method POST với Lambda](/images/5-Workshop/img_A/image65.png)

5. **Tạo Resource `/check`**:
   * Chọn đường dẫn gốc `/`, chọn **Create resource**, nhập Resource Name: `check`.
   * Tạo method **GET** tại resource `/check`, liên kết với hàm `FightingGameMatchmaker`.

![Tạo Method GET /check thành công](/images/5-Workshop/img_A/image70.png)

6. **Tạo Cognito Authorizer**:
   * Trên thanh điều hướng bên trái, chọn **Authorizers** -> chọn **Create authorizer**.
   * Đặt tên: `FightinggameCognitoAuthorizer`, chọn Type: **Cognito**, chọn User Pool `ap-southeast-1_phYoaMUPC` đã tạo ở Bước 5.3, nhập Token Source: `Authorization`. Nhấn **Create authorizer**.

![Tạo Cognito Authorizer](/images/5-Workshop/img_A/image71.png)

7. **Gắn Authorizer vào các Method**:
   * Chọn Method **POST** tại `/join`, nhấn **Edit**, mục **Authorization** chọn `FightinggameCognitoAuthorizer` và bấm **Save**.
   * Thực hiện tương tự cho Method **GET** tại `/check`.

![Gắn Cognito Authorizer vào API](/images/5-Workshop/img_A/image74.png)

8. **Bật CORS (Enable CORS)**:
   * Chọn resource `/join`, chọn **Enable CORS**, tích chọn **Default 4xx, 5xx** và **POST**.
   * Thực hiện tương tự cho resource `/check` với method **GET**. Chọn **Save**.

![Bật CORS thành công](/images/5-Workshop/img_A/image81.png)

9. **Deploy API**:
   * Chọn đường dẫn gốc `/` của cây Resource, chọn **Deploy API**.
   * Stage name: `prod`. Nhấn **Deploy**.
   * Lưu lại địa chỉ **Invoke URL** (Ví dụ: `https://6whg1d5qca.execute-api.ap-southeast-1.amazonaws.com/prod`).

![Deploy API thành công](/images/5-Workshop/img_A/image85.png)
