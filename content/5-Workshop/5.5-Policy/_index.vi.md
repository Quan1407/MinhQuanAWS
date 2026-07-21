---
title: "EC2 Spot Fleet & GitOps CodeDeploy"
date: 2026-07-21
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

# Cấu hình EC2 Spot Fleet, Launch Template & GitOps CodeDeploy

Trong phần này, chúng ta sẽ tạo máy chủ game mẫu trên **Amazon EC2 (Ubuntu 24.04 LTS)**, đóng gói AMI, tạo **Launch Template** cho EC2 Spot Fleet với **ASG Warm Pool**, cấu hình **S3 Bucket Static Website**, và thiết lập luồng **GitOps CI/CD** với **GitHub OIDC** và **AWS CodeDeploy**.

---

### Phần 1: Khởi tạo EC2 Game Server Mẫu & Đóng gói AMI

1. Truy cập dịch vụ **Amazon EC2** và chọn **Launch instance**.
2. Nhập các thông tin khởi tạo:
   * **Name**: `FightingGameServer`
   * **AMI**: Ubuntu Server 24.04 LTS (64-bit ARM / x86)
   * **Instance type**: `t3.small` hoặc `t3.medium`
   * **Key pair**: Tạo mới hoặc dùng Key Pair có sẵn
   * **Storage**: 8 GB gp3
   * **Security Group**: Tạo mới Security Group cho phép SSH (Port 22) và Cổng kết nối Game (Port 3000/UDP/TCP).

![Khởi tạo EC2 FightingGameServer](/images/5-Workshop/img_A/image88.png)

3. Nhấn **Launch instance**. Cấu hình Node.js Game Server trên Ubuntu instance.

![Cấu hình Node.js Game Server](/images/5-Workshop/img_A/image91.png)

4. **Đóng gói AMI từ Instance**:
   * Chọn instance `FightingGameServer`, vào **Actions** -> **Image and templates** -> chọn **Create image**.
   * Nhập Image name: `FightingGameServerAMI` và bấm **Create image**.

![Bake AMI từ EC2 Instance](/images/5-Workshop/img_B/image2.png)

---

### Phần 2: Tạo Launch Template & Auto Scaling Group Warm Pool

1. Vào **Launch Templates** -> chọn **Create launch template**.
2. Chọn AMI `FightingGameServerAMI`, chọn **Spot Instance**, cấu hình IAM Instance Profile `FightingGameServerInstanceRole` để cho phép EC2 gọi S3 pull server bundle lúc boot.

![Tạo Launch Template Spot](/images/5-Workshop/img_B/image3.png)

3. Tạo **Auto Scaling Group (ASG)** từ Launch Template vừa tạo và bật tính năng **Warm Pool** để giữ các máy chủ game ở trạng thái sẵn sàng spin-up ngay lập tức khi Matchmaker yêu cầu.

![Cấu hình ASG Warm Pool](/images/5-Workshop/img_B/image4.png)

---

### Phần 3: Khởi tạo Amazon S3 Bucket & Static Hosting

1. Truy cập dịch vụ **Amazon S3** và chọn **Create bucket**.
2. Nhập tên Bucket (Ví dụ: `fighting-game-assets-singapore`).
3. Bật tính năng **Static website hosting** và cấu hình **Bucket policy** (phân quyền public read cho client asset) cũng như **CORS Policy** để giao diện web truy cập API không bị chặn.

![Cấu hình S3 Static Website Hosting & CORS](/images/5-Workshop/img_B/image6.png)

---

### Phần 4: Cấu hình GitHub OIDC & AWS CodeDeploy GitOps Pipeline

1. **Thêm GitHub Provider vào AWS IAM (No long-lived access keys)**:
   * Vào **IAM** -> **Identity providers** -> chọn **Add provider**.
   * Chọn **OpenID Connect**, nhập Provider URL: `https://token.actions.githubusercontent.com` và Audience: `sts.amazonaws.com`.

![Thêm GitHub OIDC Provider](/images/5-Workshop/img_B/image7.png)

2. **Cài đặt AWS CodeDeploy Agent trên EC2 Game Server**:
   Thực thi kịch bản cài đặt CodeDeploy Agent trên Ubuntu 24.04 LTS:

```bash
# 1. Install prerequisites
sudo apt-get update && sudo apt-get install -y ruby-full ruby-webrick wget gdebi-core

# 2. Download raw .deb package directly
cd /tmp
wget https://aws-codedeploy-ap-southeast-1.s3.ap-southeast-1.amazonaws.com/releases/codedeploy-agent_1.8.1-26_all.deb

# 3. Unpack, fix Ruby dependency declaration, and repack
dpkg-deb -R codedeploy-agent_1.8.1-26_all.deb /tmp/codedeploy-extracted
sed -i "s/ruby3.2/ruby3.3/g" /tmp/codedeploy-extracted/DEBIAN/control
dpkg-deb -b /tmp/codedeploy-extracted /tmp/codedeploy-agent_fixed.deb

# 4. Install patched package and start service
sudo dpkg -i /tmp/codedeploy-agent_fixed.deb
sudo systemctl enable codedeploy-agent
sudo systemctl start codedeploy-agent
sudo systemctl status codedeploy-agent
```

![Cài đặt thành công CodeDeploy Agent](/images/5-Workshop/img_B/image8.png)

3. **Khởi tạo CodeDeploy Application & Deployment Group**:
   * Truy cập **AWS CodeDeploy** -> **Applications** -> chọn **Create application**.
   * Application name: `FightingGameServerApp`, Compute platform: **EC2/On-premises**.
   * Tạo **Deployment group**, chọn IAM Role cho CodeDeploy và gán với EC2 Spot Fleet.

![Khởi tạo CodeDeploy Deployment Group](/images/5-Workshop/img_B/image9.png)

4. Khi lập trình viên push mã nguồn lên GitHub, GitHub Actions tự động kích hoạt CodeDeploy Job triển khai bản build mới lên Fleet máy chủ game thành công.

![CodeDeploy Job triển khai thành công](/images/5-Workshop/img_B/image10.png)

Khi bạn tạo một Interface Endpoint  hoặc cổng, bạn có thể đính kèm một chính sách điểm cuối để kiểm soát quyền truy cập vào dịch vụ mà bạn đang kết nối. Chính sách VPC Endpoint là chính sách tài nguyên IAM mà bạn đính kèm vào điểm cuối. Nếu bạn không đính kèm chính sách khi tạo điểm cuối, thì AWS sẽ đính kèm chính sách mặc định cho bạn để cho phép toàn quyền truy cập vào dịch vụ thông qua điểm cuối.

Bạn có thể tạo chính sách chỉ hạn chế quyền truy cập vào các S3 bucket cụ thể. Điều này hữu ích nếu bạn chỉ muốn một số Bộ chứa S3 nhất định có thể truy cập được thông qua điểm cuối.


![endpoint diagram](/images/5-Workshop/5.5-Policy/s3-bucket-policy.png)

#### Kết nối tới EC2 và xác minh kết nối tới S3. 

1. Bắt đầu một phiên AWS Session Manager mới trên máy chủ có tên là Test-Gateway-Endpoint. Từ phiên này, xác minh rằng bạn có thể liệt kê nội dung của bucket mà bạn đã tạo trong Phần 1: Truy cập S3 từ VPC.

```
aws s3 ls s3://<your-bucket-name>
```
![test](/images/5-Workshop/5.5-Policy/test1.png)

Nội dung của bucket bao gồm hai tệp có dung lượng 1GB đã được tải lên trước đó.

2. Tạo một bucket S3 mới; tuân thủ mẫu đặt tên mà bạn đã sử dụng trong Phần 1, nhưng thêm '-2' vào tên. Để các trường khác là mặc định và nhấp vào **Create**.

![create bucket](/images/5-Workshop/5.5-Policy/create-bucket.png)

3. Tạo bucket thành công.

![Success](/images/5-Workshop/5.5-Policy/create-bucket-success.png)

Policy mặc định cho phép truy cập vào tất cả các S3 Buckets thông qua VPC endpoint.

4. Trong giao diện **Edit Policy**, sao chép và dán theo policy sau, thay thế yourbucketname-2 với tên bucket thứ hai của bạn. Policy này sẽ cho phép truy cập đến bucket mới thông qua VPC endpoint, nhưng không cho phép truy cập đến các bucket còn lại. Chọn **Save** để kích hoạt policy.


```
{
  "Id": "Policy1631305502445",
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Stmt1631305501021",
      "Action": "s3:*",
      "Effect": "Allow",
      "Resource": [
      				"arn:aws:s3:::yourbucketname-2",
       				"arn:aws:s3:::yourbucketname-2/*"
       ],
      "Principal": "*"
    }
  ]
}
```

![custom policy](/images/5-Workshop/5.5-Policy/policy2.png)

Cấu hình policy thành công.

![success](/images/5-Workshop/5.5-Policy/success.png)

5. Từ session của bạn trên Test-Gateway-Endpoint instance, kiểm tra truy cập đến S3 bucket bạn tạo ở bước đầu

```
aws s3 ls s3://<yourbucketname>
```

Câu lệnh trả về lỗi bởi vì truy cập vào S3 bucket không có quyền trong VPC endpoint policy.

![error](/images/5-Workshop/5.5-Policy/error.png)

6. Trở lại home directory của bạn trên EC2 instance ```cd~```

+ Tạo file ```fallocate -l 1G test-bucket2.xyz ```
+ Sao chép file lên bucket thứ  2 ```aws s3 cp test-bucket2.xyz s3://<your-2nd-bucket-name>```

![success](/images/5-Workshop/5.5-Policy/test2.png)

Thao tác này được cho phép bởi VPC endpoint policy.

![success](/images/5-Workshop/5.5-Policy/test2-success.png)

Sau đó chúng ta kiểm tra truy cập vào S3 bucket đầu tiên

 ```aws s3 cp test-bucket2.xyz s3://<your-1st-bucket-name>```

 ![fail](/images/5-Workshop/5.5-Policy/test2-fail.png)

 Câu lệnh xảy ra lỗi bởi vì bucket không có quyền truy cập bởi VPC endpoint policy.

Trong phần này, bạn đã tạo chính sách VPC Endpoint cho Amazon S3 và sử dụng AWS CLI để kiểm tra chính sách. Các hoạt động AWS CLI liên quan đến bucket S3 ban đầu của bạn thất bại vì bạn áp dụng một chính sách chỉ cho phép truy cập đến bucket thứ hai mà bạn đã tạo. Các hoạt động AWS CLI nhắm vào bucket thứ hai của bạn thành công vì chính sách cho phép chúng. Những chính sách này có thể hữu ích trong các tình huống khi bạn cần kiểm soát quyền truy cập vào tài nguyên thông qua VPC Endpoint.
