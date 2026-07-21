---
title: "Khởi tạo Cognito & DynamoDB Tables"
date: 2026-07-21
weight: 2
chapter: false
pre: " <b> 5.1.2. </b> "
---

# 5.1.2. Khởi tạo Amazon Cognito & DynamoDB Tables

Trong phần này, chúng ta sẽ cấu hình **Amazon Cognito User Pool & Identity Pool** để xử lý xác thực người chơi và cấp phát quyền tải asset/patch từ S3. Sau đó, chúng ta sẽ tạo 2 bảng **Amazon DynamoDB** để phục vụ hàng đợi ghép trận (`MatchmakingQueue`) và các trận đấu đang hoạt động (`ActiveMatches`).

---

### Phần 1: Khởi tạo Amazon Cognito User Pool
1. Tại thanh tìm kiếm trên AWS Console, gõ **Cognito** và chọn **Amazon Cognito**.

![Tìm kiếm Cognito](/images/5-Workshop/img_A/image3.png)

![Trang Amazon Cognito](/images/5-Workshop/img_A/image4.png)

2. Chọn **Single-page application (SPA)** và đổi tên ứng dụng thành `FightingGame`.

![Cấu hình ứng dụng SPA](/images/5-Workshop/img_A/image7.png)

3. Tại mục **Username**, tích chọn **Enable Self-registration** để cho phép người dùng tự tạo tài khoản.
4. Tại mục **Required attributes for sign-up**, chọn **email** (Email thích hợp cho mô hình demo và phục hồi mật khẩu).

![Cấu hình thuộc tính đăng ký](/images/5-Workshop/img_A/image8.png)

5. Chọn **Create user directory**. Sau khi khởi tạo thành công:
   * **User pool ID**: `ap-southeast-1_phYoaMUPC`
   * Trong **App clients**, chọn `FightingGame` để xem **Client ID** (Ví dụ: `73ipqvvo7h3u0j3elfqlj23jo3`).

![Thông tin User Pool ID](/images/5-Workshop/img_A/image11.png)

![Xem App Client ID](/images/5-Workshop/img_A/image12.png)

6. Nhấn **Edit** tại App client `FightingGame` và tích chọn phương thức xác thực: `ALLOW_USER_PASSWORD_AUTH`. Sau đó chọn **Save changes**.

![Cấu hình ALLOW_USER_PASSWORD_AUTH](/images/5-Workshop/img_A/image14.png)

---

### Phần 2: Khởi tạo Amazon Cognito Identity Pool
1. Quay lại trang Cognito chính và chuyển sang tab **Identity pools**, chọn **Create identity pool**.

![Tạo Identity Pool](/images/5-Workshop/img_A/image17.png)

2. Chọn **Authenticated access** và chọn nguồn **Amazon Cognito user pool**. Chọn **Next**.

![Cấu hình Authenticated Access](/images/5-Workshop/img_A/image19.png)

3. Tại mục **Configure permissions**, chọn **Create a new IAM Role**, đặt tên IAM role là `FightingGameAuthenticatedRole`. Chọn **Next**.

![Tạo IAM Role cho Identity Pool](/images/5-Workshop/img_A/image20.png)

4. Nhập **User pool ID** và **App Client ID** đã tạo ở Phần 1 để kết nối Identity Provider. Chọn **Next**.

![Kết nối Identity Provider](/images/5-Workshop/img_A/image21.png)

5. Nhập tên Identity pool: `FightingGameIdentityPool`. Kiểm tra lại cấu hình và nhấn **Create identity pool**.
   * **Identity pool ID**: `ap-southeast-1:a5d743b9-e4a4-45d2-9cb1-9d214cee574c`

![Thông tin Identity Pool ID](/images/5-Workshop/img_A/image24.png)

---

### Phần 3: Khởi tạo các Bảng Amazon DynamoDB

Chúng ta sẽ tạo 2 bảng DynamoDB để quản lý trạng thái ghép trận:

#### Bảng 1: `MatchmakingQueue` (Hàng đợi ghép trận)
1. Truy cập dịch vụ **DynamoDB** và chọn **Create table**.
2. Nhập các thông tin:
   * **Table name**: `MatchmakingQueue`
   * **Partition key**: `playerId` (Kiểu dữ liệu: **String**)
   * **Sort key**: Để trống
   * **Table settings**: Giữ nguyên **Default settings**
3. Kéo xuống cuối trang và nhấn **Create table**.

![Tạo bảng MatchmakingQueue](/images/5-Workshop/img_A/image27.png)

#### Bảng 2: `ActiveMatches` (Các trận đấu đang diễn ra)
1. Tiếp tục chọn **Create table**.
2. Nhập các thông tin:
   * **Table name**: `ActiveMatches`
   * **Partition key**: `playerId` (Kiểu dữ liệu: **String**)
   * **Sort key**: Để trống
   * **Table settings**: Giữ nguyên **Default settings**
3. Nhấn **Create table**.

![Tạo bảng ActiveMatches](/images/5-Workshop/img_A/image28.png)

4. Kiểm tra trang danh sách bảng: Cả 2 bảng `MatchmakingQueue` và `ActiveMatches` hiển thị trạng thái **Active**.

![Danh sách 2 bảng DynamoDB Active](/images/5-Workshop/img_A/image29.png)
