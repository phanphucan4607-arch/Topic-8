## 🎯 Mục đích của bài thực hành (Migration & Multi-Framework Hosting)

Mục đích cốt lõi của bài này là Di cư hệ thống (Migration) từ một môi trường quản trị hiện đại (CyberPanel) sang một môi trường quản trị cổ điển, hiệu năng cao (VestaCP) mà vẫn đảm bảo tính toàn vẹn của dữ liệu và dịch vụ.

Cụ thể, bài toán này giải quyết 4 vấn đề lớn
#### 1. Khả năng tương thích đa nền tảng (Multi-Framework Support)

Chứng minh rằng một máy chủ (VPS) có thể chạy song song nhiều loại mã nguồn khác nhau

    WordPress: Đại diện cho CMS (Content Management System) chạy trên cấu trúc thư mục phẳng.

    Laravel: Đại diện cho Framework PHP hiện đại, yêu cầu cấu trúc bảo mật khắt khe (thư mục /public).

#### 2. Quản trị vòng đời chứng chỉ bảo mật (SSL Lifecycle)

Mục đích là sử dụng Certbot (một công cụ độc lập của Let's Encrypt) để quản lý SSL thay vì phụ thuộc hoàn toàn vào bảng điều khiển (Control Panel). Điều này giúp ông chủ động trong việc gia hạn và bảo mật đường truyền (HTTPS) cho mọi website trên server.

#### 3. Tối ưu hóa hiệu năng với PHP 8.1

Bài toán yêu cầu nâng cấp môi trường thực thi (Runtime) từ các bản PHP cũ lên PHP 8.1. Mục đích là để tận dụng tối đa tốc độ xử lý, tính năng mới của ngôn ngữ và đảm bảo các bản vá lỗi bảo mật mới nhất cho Laravel và WordPress.

#### 4. Kỹ năng quản trị hệ thống Linux chuyên sâu

Thay vì dùng các nút bấm "mì ăn liền" trên CyberPanel, việc chuyển sang VestaCP buộc ông phải can thiệp vào

    Virtual Host: Tự tay sửa file cấu hình Apache/Nginx.

    Permissions: Hiểu sâu về Owner/Group (quyền sở hữu file) trên Linux.

    Database Management: Di chuyển dữ liệu giữa các môi trường khác nhau mà không làm mất mát thông tin.

### 🟢 Giai đoạn 1.1: Cài đặt VestaCP
**Kết nối với máy chủ của bạn dưới dạng root qua SSH**

ssh root@your.server

**Tải kịch bản cài đặt**

curl -O https://vestacp.com/pub/vst-install.sh

♪ **Chạy đi**

bash vst-install.sh

<img width="917" height="699" alt="image" src="https://github.com/user-attachments/assets/dde9e790-1309-4e4e-9b15-784ff0a91025" />

 https://221.132.21.141:8083

    username: admin
    
    password: gt40ZZvo8q

### 🟢 Giai đoạn 1.2: Cài đặt PHP 8.1
Mặc dù VestaCP đã cài xong, nhưng bản PHP đi kèm thường là bản cũ. Ông chạy các lệnh này để đưa nó lên 8.1

# 1. Thêm kho lưu trữ PHP (Dành cho Ubuntu 18.04)
apt-get install software-properties-common -y
add-apt-repository ppa:ondrej/php -y
apt-get update

# 2. Cài đặt PHP 8.1 và các gói cần thiết
```
apt-get install php8.1 php8.1-common php8.1-mysql php8.1-xml php8.1-xmlrpc php8.1-curl php8.1-

gd php8.1-imagick php8.1-cli php8.1-dev php8.1-imap php8.1-mbstring php8.1-opcache php8.1-soap

php8.1-zip php8.1-intl php8.1-bcmath -y

 3. Kiểm tra lại phiên bản

php -v
```
