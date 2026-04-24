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
Bước 1: Cài đặt thư viện nền (Dependencies)
```
apt-get update

apt-get install -y build-essential libxml2-dev libssl-dev libsqlite3-dev \

libcurl4-openssl-dev libpng-dev libjpeg-dev libonig-dev libzip-dev \

libreadline-dev libicu-dev
```
Bước 2: Tải và giải nén mã nguồn
```
wget https://www.php.net/distributions/php-8.1.27.tar.gz

tar -xvf php-8.1.27.tar.gz

cd php-8.1.27
```


Bước 3: Cấu hình các module (Configure)
Đây là bước quan trọng nhất để PHP có đủ "chức năng" chạy Laravel và WordPress.
```
./configure --prefix=/usr/local/php81 \
--with-config-file-path=/usr/local/php81 \
--enable-mbstring \
--enable-fpm \
--with-mysqli=mysqlnd \
--with-pdo-mysql=mysqlnd \
--with-openssl \
--with-curl \
--with-zlib \
--enable-gd \
--with-zip \
--enable-bcmath \
--enable-intl
```

Bước 4: Biên dịch và cài đặt (Make)
```
make -j$(nproc)
make install
```
Bước 5: Kích hoạt lệnh chạy nhanh (Symlink)
Bash

ln -s /usr/local/php81/bin/php /usr/bin/php81

🔍 Lệnh kiểm tra cuối cùng

php81 -v
<img width="922" height="177" alt="image" src="https://github.com/user-attachments/assets/abd1dfb5-cd57-4214-a9cf-f63804128459" />


#### 🟡 Giai đoạn 2: Tạo "Nhà" (Web) và "Bể chứa" (Database)
1. Tạo 2 website nơi chứa code 

<img width="1229" height="608" alt="image" src="https://github.com/user-attachments/assets/6f163a6e-c3de-491b-91ab-24ecfaa21ebe" />

2. Tạo 2 Database (Nơi chứa dữ liệu SQL)

<img width="1229" height="608" alt="image" src="https://github.com/user-attachments/assets/925fe91d-f9bf-4d96-bebf-92d5a88ffd70" />

+ vào phpmyadmn import data
+ cho wordress
  <img width="1822" height="657" alt="image" src="https://github.com/user-attachments/assets/b87f3042-b6fd-45f5-9ca8-af5356e8a890" />

+ cho lara
  <img width="1822" height="657" alt="image" src="https://github.com/user-attachments/assets/f2ffd61a-c4bc-44a5-9d22-2a7fadd36874" />

# 🚀 Giai đoạn 3: "Bốc" Source Code vào Server (Dùng Terminal)

```
# Đối với Laravel:
scp /đường/dẫn/laravel_clean.tar.gz root@221.132.21.141:/home/admin/tmp/

# Đối với WordPress:
scp /đường/dẫn/wp_source.tar.gz root@221.132.21.141:/home/admin/tmp/
```
```
2. Di chuyển & Giải nén (Làm tại SSH VPS)
# 1. Truy cập vào thư mục web (Thay đổi theo domain tương ứng)
cd /home/admin/web/laravel.phucan.vietnix.tech/public_html/

# 2. Hốt file từ tmp về đây
mv /home/admin/tmp/*.tar.gz .

# 3. Giải nén (Xóa sạch file index.html mặc định trước nếu có)
rm -f index.html
tar -xzvf *.tar.gz

# 4. Dọn dẹp xác file nén
rm -f *.tar.gz
   ``` 

 ```
3. Thiết lập "Quyền sinh tử" (Phân quyền Folder)
Đây là bước quan trọng nhất để Web không bị lỗi 403 Forbidden hoặc 500 Internal Server Error.
A. Đối với Laravel:

# Trả quyền sở hữu cho admin
chown -R admin:admin /home/admin/web/laravel.phucan.vietnix.tech/public_html

# Cấp quyền ghi đặc biệt (Bắt buộc)

chmod -R 775 storage

chmod -R 775 bootstrap/cache

B. Đối với WordPress:

# Trả quyền sở hữu cho admin
chown -R admin:admin /home/admin/web/phucan.vietnix.tech/public_html

# Cấp quyền để upload ảnh và cài plugin không bị hỏi mật khẩu FTP

chmod -R 755 /home/admin/web/phucan.vietnix.tech/public_html

chmod -R 775 wp-content/uploads
```
```
4. cấu hình biến môi trường
Với Laravel:

nano .env
# Sửa: DB_DATABASE, DB_USERNAME, DB_PASSWORD, APP_URL


Với WordPress:
Bash

nano wp-config.php
# Sửa: DB_NAME, DB_USER, DB_PASSWORD
```

**5. Khởi động lại hệ thống**
```
systemctl restart apache2

systemctl restart nginx

systemctl restart php81-fpm  # (Hoặc bản PHP đang dùng)
```

#### 🚀 Giai đoạn 4: Cấu hình Web Server trỏ vào thư mục /public
Sau khi đã bốc code lên và phân quyền xong, ta cần thực hiện bước này để Apache và Nginx biết đường dẫn chính xác vào "tim" của Laravel và wordress.
```
1. Sửa cấu hình Apache (DocumentRoot)

Apache là thằng xử lý file PHP phía sau. Ông cần sửa cả file chạy thường và file SSL.

    File thường: /home/admin/conf/web/domain.com.apache2.conf

    File SSL: /home/admin/conf/web/domain.com.apache2.ssl.conf

Nội dung cần sửa: Tìm dòng DocumentRoot và thẻ <Directory>, thêm /public vào cuối đường dẫn.

    Ví dụ: /home/admin/web/domain.com/public_html

    Thành: /home/admin/web/domain.com/public_html/public
```
```
2. Sửa cấu hình Nginx (Root)

Nginx đóng vai trò là "người gác cổng" phía trước. Ông cũng cần sửa 2 file tương tự.

    File thường: /home/admin/conf/web/domain.com.nginx.conf

    File SSL: /home/admin/conf/web/domain.com.nginx.ssl.conf

Nội dung cần sửa: Tìm dòng root, thêm /public vào cuối.

    Ví dụ: root /home/admin/web/domain.com/public_html;

    Thành: root /home/admin/web/domain.com/public_html/public;
```

```
# Kiểm tra xem có gõ sai cú pháp không (nếu không báo lỗi gì là OK)
nginx -t

apache2ctl -t

# Restart để áp dụng
```

#### trường hợp khi làm gặp looix và cách fix 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/259df2bb-0b40-4e34-a6d5-b625699b8b78" />


Chính vì VestaCP của mình  không có cái nút ép HTTPS tự động đó, nên cái "liều thuốc" tui kê cho ông đưa vào file wp-config.php là bắt buộc 100% phải có để trị dứt điểm cái bệnh "Vòng lặp 301".

```

nano wp-config.php

Bước 3: Dán "Kháng sinh liều cao"
Xóa cái đoạn if (isset... cũ đi (nếu có), dán 4 dòng chốt hạ này ngay dưới dòng <?php:
PHP

$_SERVER['HTTPS'] = 'on';
$_SERVER['SERVER_PORT'] = 443;
$_SERVER['HTTP_X_FORWARDED_PROTO'] = 'https';

define('WP_HOME','https://wp.phucan.vietnix.tech');
define('WP_SITEURL','https://wp.phucan.vietnix.tech');
````
systemctl restart apache2

systemctl restart nginx

lỗi php
<img width="1822" height="657" alt="image" src="https://github.com/user-attachments/assets/a139c2cf-2afd-4ff1-9e46-12638f427a6f" />


