---
title: "EC2 Spot Fleet & GitOps CodeDeploy"
date: 2026-07-21
weight: 4
chapter: false
pre: " <b> 5.1.4. </b> "
---

# 5.1.4. Cấu hình EC2 Spot Fleet, Launch Template & GitOps CodeDeploy

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
