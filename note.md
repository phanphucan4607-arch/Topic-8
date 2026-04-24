**Cấp chứng chỉ tự động lại cho wordress và laravel**

+ wordress
<img width="782" height="617" alt="image" src="https://github.com/user-attachments/assets/43e58b56-54e8-484b-805a-4677c68c37eb" />


+ laravel

<img width="820" height="641" alt="image" src="https://github.com/user-attachments/assets/850ea06e-0655-44e6-be7b-4b2072ff25cb" />

**đường dẫn chứa Cert của Certbot**

+ của laravel: /etc/letsencrypt/live/laravel.phucan.vietnix.tech/

<img width="1194" height="319" alt="image" src="https://github.com/user-attachments/assets/3e07e169-3ed6-45c9-b3b5-7706887af39e" />



+ Cấp cho wordress 
```
certbot certonly --webroot -w /home/admin/web/wp.phucan.vietnix.tech/public_html -d wp.phucan.vietnix.tech
```

+ Cấp cho laravel
```
certbot certonly --webroot -w /home/admin/web/laravel.phucan.vietnix.tech/public_html/public -d laravel.phucan.vietnix.tech
```

🛠️  **Lệnh Backup**

 đuôi thành .bak_24_04, cho cả 2 trang (WP và Laravel)

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
# 1. Tạo thư mục chứa trên máy
mkdir -p ~/Downloads/SSL_FULL_BACKUP_24_04
cd ~/Downloads/SSL_FULL_BACKUP_24_04

# 2. Tải toàn bộ file SSL (cả file mới và file .bak) về máy
scp root@221.132.21.141:/home/admin/conf/web/ssl.* .
```
