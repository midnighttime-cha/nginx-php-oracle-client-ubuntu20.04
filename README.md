# วิธีติดตั้ง Instant Client 12.2 สำหรับ nginx + php5.6 บน Uubuntu 20.04

- [Ubuntu + nginx + php5.6 + oci8](https://github.com/midnighttime-cha/nginx-php-oracle-client-ubuntu20.04/blob/main/php5.6.md)
- [Ubuntu + nginx + php5.6 + oci8](https://github.com/midnighttime-cha/nginx-php-oracle-client-ubuntu20.04/blob/main/php8.1.md)

- จัดการ Version ของ PHP
```bash
sudo update-alternatives --config php
```
หมายเหตุ: ตรงช่อง selectionจะมีตัวเลขและเครื่องหมาย * ข้างหน้า แสดงว่าเป็น version ปัจจุบัน ให้ทำการกรอกตัวเลข version ที่ต้องการเปลี่ยนแล้วกดปุ่ม Enter

## Reference
1. [How to install OCI8 on Ubuntu 14.04 and PHP 5.6](http://www.syahzul.com/2016/04/06/how-to-install-oci8-on-ubuntu-14-04-and-php-5-6/)
2. [วิธีติดตั้ง PHP หลาย Version ให้ใช้งานกับ NGINX บน Ubuntu 20.04](https://github.com/midnighttime-cha/nginx-multiple-php)
