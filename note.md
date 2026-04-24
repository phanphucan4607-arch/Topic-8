**Cấp chứng chỉ tự động lại cho wordress và laravel**

+ wordress
<img width="782" height="617" alt="image" src="https://github.com/user-attachments/assets/43e58b56-54e8-484b-805a-4677c68c37eb" />


+ laravel

<img width="820" height="641" alt="image" src="https://github.com/user-attachments/assets/850ea06e-0655-44e6-be7b-4b2072ff25cb" />

**đường dẫn chứa Cert của Certbot**



🛠️  **Lệnh Backup**

 đuôi thành .bak_24_04, đại ca quất bộ lệnh này cho cả 2 trang (WP và Laravel)

```
 cd /home/admin/conf/web/

# Backup cho WordPress
mv ssl.wp.phucan.vietnix.tech.crt ssl.wp.phucan.vietnix.tech.crt.bak_24_04
mv ssl.wp.phucan.vietnix.tech.key ssl.wp.phucan.vietnix.tech.key.bak_24_04
mv ssl.wp.phucan.vietnix.tech.ca ssl.wp.phucan.vietnix.tech.ca.bak_24_04

# Backup cho Laravel
mv ssl.laravel.phucan.vietnix.tech.crt ssl.laravel.phucan.vietnix.tech.crt.bak_24_04
mv ssl.laravel.phucan.vietnix.tech.key ssl.laravel.phucan.vietnix.tech.key.bak_24_04
mv ssl.laravel.phucan.vietnix.tech.ca ssl.laravel.phucan.vietnix.tech.ca.bak_24_04
```
**Mở Terminal ở máy cá nhân Ubuntu**

```
# 1. Tạo thư mục chứa trên máy ông
mkdir -p ~/Downloads/SSL_FULL_BACKUP_24_04
cd ~/Downloads/SSL_FULL_BACKUP_24_04

# 2. Tải toàn bộ file SSL (cả file mới và file .bak) về máy
scp root@221.132.21.141:/home/admin/conf/web/ssl.* .
```
