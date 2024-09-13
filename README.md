# วิธีติดตั้ง Instant Client 12.2 สำหรับ nginx + php5.6 บน Uubuntu 20.04

## 1. Install Instant Client 12.2

### ทำการ Download files จากเว็บไซต์ต่อไปนี้ [Download](https://www.oracle.com/database/technologies/instant-client/linux-x86-64-downloads.html)

- Dowload: instantclient-basic-linux.x64-12.2.0.4.0.zip
- Dowload: instantclient-sqlplus-linux.x64-12.2.0.4.0.zip
- Dowload: instantclient-sdk-linux.x64-12.2.0.4.0.zip 

### สร้าง Directory ต่อไปนี้ และทำการย้ายไฟล์ Oracle client ที่ทำการ Download มาแล้วเก็บไว้ใน Directory ดังกล่าว
```bash
mkdir -p /opt/oracle
mv instantclient-*-linux.x64-12.2.0.4.0.zip /opt/oracle/
```

### ทำการติดตั้ง Package upzip ด้วยคำสั่ง และทำการ upzip ไฟล์. Oracle Client ทั้งหมด
```bash
apt install unzip
cd /opt/oracle/
unzip instantclient-basic-linux.x64-12.2.0.4.0.zip
unzip instantclient-sqlplus-linux.x64-12.2.0.4.0.zip
unzip instantclient-sdk-linux.x64-12.2.0.4.0.zip
...
# Output: /opt/oracle/instantclient_12_2
```

### ทำการสร้าง .bash_profile 
```bash
nano ~/.bash_profile

export ORACLE_HOME=/opt/oracle/instantclient_12_2
export DYLD_LIBRARY_PATH=$ORACLE_HOME
export PATH=$PATH:$DYLD_LIBRARY_PATH
export SQL_PATH=$ORACLE_HOME
export PATH=$PATH:$ORACLE_HOME
export NLS_LANG="AMERICAN_AMERICA.UTF8"
```

### Run .bash_profile
```bash
source ~/.bash_profile
```

### ทำการสร้าง shotcut link ต่อไปนี้
```bash
ln -s /opt/oracle/instantclient_12_2/libclntsh.so.12.1 /opt/oracle/instantclient_12_2/libclntsh.so
ln -s /opt/oracle/instantclient_12_2/libocci.so.12.1 /opt/oracle/instantclient_12_2/libocci.so	
```

### เพิ่ม instance config
```bash
echo /opt/oracle/instantclient_12_2 > /etc/ld.so.conf.d/oracle-instantclient.conf

ldconfig
```

## 2. ติดตั้ง Nginx + php

### ติดตั้ง NGINX
ทำการอัพเทเวอร์ชั่นของ Ubuntu และติดตั้ง NGINX ได้เลย
```bash
apt update
apt install nginx
```
จากนั้นทำการตรวจสอบรายการ Application ใน Firewall ด้วย
```bash
ufw app list
```
<b>ผลลัพธ์</b>
```
Available applications:
  Nginx Full
  Nginx HTTP
  Nginx HTTPS
  OpenSSH
```
ระบบจะแสดงรายการ application ต่างๆออกมา เราสามารถ firewall allow ด้วยวิธี
```bash
ufw allow 'Nginx Full'
ufw allow 'Nginx HTTP'
ufw allow 'Nginx HTTPS'
ufw allow 'OpenSSH'
```
อย่าลืม Allow OpenSSH ด้วยนะจ๊ะเดี่ยวจะ Remote ไม่ได้ถ้าเราเปิดใช้งาน Firewall ตรวจสอบว่า application ที่เราทำการ allow ไว้ด้วยคำสั่ง
```bash
ufw status
```
<b>ผลลัพธ์</b>
```
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
Nginx HTTP                 ALLOW       Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)
Nginx HTTP (v6)            ALLOW       Anywhere (v6)
```
ถ้าระบบแสดงผลเป็น
```
Status: inactive
```
แสดงว่า firewall ของคุณยังไม่เปิดใช้งานให้คุณใช้คำสั่งต่อไปนี้เพื่อเปิดใช้งาน firewall
```bash
ufw enable
```
ถ้าเราไม่ทราบ หรือไม่มี Domain name ให้ใช้คำสั่ง
```bash
ip addr show eth0 | grep inet | awk '{ print $2; }' | sed 's/\/.*$//'
```
ระบบจะแสดงผลดังนี้
```
99.38.118.190
10.3.0.16
fe00::905a:faff:faa2:20c7
```
จากนั้นให้ทดสอบการทำงานของ NGINX เปิด URL: http://99.38.118.190

![NGINX Wellcome](https://storage.kaikannook.com/image/showimage/common/blog/be384df195c3258cb34e1010b2051faeb0.png)

ถ้าขึ้นหน้าต่างแบบนี้ถือว่าการติดตั้ง NGINX เสร็จสมบูรณ์

## ติดตั้ง PHP version 5.6
เช่นเคยให้ทำการอัพเดท ubuntu กันก่อน
```bash
apt update
```
จากนั้นตั้งค่า Repository ของ PHP กันครับด้วยคำสั่ง
```bash
sudo add-apt-repository ppa:ondrej/php && sudo apt update
```
จากนั้นติดตั้ง PHP5.6 และ extension กันได้เลย
```bash
apt install -y php5.6 php5.6-fpm
apt -y install php5.6-mysql
apt -y install php5.6-curl
apt -y install php5.6-gd
apt -y install php5.6-intl
apt -y install php-pear
apt -y install php-imagick
apt -y install php5.6-imap
apt -y install php5.6-mcrypt
apt -y install php5.6-memcache
apt -y install php5.6-pspell
apt -y install php5.6-recode
apt -y install php5.6-sqlite3
apt -y install php5.6-tidy
apt -y install php5.6-xml
apt -y install php5.6-xmlrpc
apt -y install php5.6-xsl
apt -y install php5.6-mbstring
apt -y install php5.6-gettext
apt -y install php5.6-dev
apt -y install build-essential
apt -y install libaio1

sudo update-alternatives --config php
```

### ตรวจสอบว่าได้ลง Extension อะไรไปแล้วบ้าง
```sh
dpkg --get-selections | grep -i php
```

## ติดตั้ง OCI8 ด้วย pecl

### ติดตั้ง oci8
```bash
pecl channel-update pecl.php.net
pecl install oci8-2.0.12 # Use 'pecl install oci8-2.0.12' to install for PHP 5.2 - PHP 5.6.
```

### กรอก ORACLE_HOME
Please provide the path to the ORACLE_HOME directory. Use 'instantclient,/path/to/instant/client/lib' if you're compiling with Oracle Instant Client [autodetect] :
พิมพ์ตามนี้
```bash
instantclient,/opt/oracle/instantclient_12_2
```

### แก้ไขไฟล์ php.ini
```bash
echo "extension = oci8.so" >> /etc/php/5.6/fpm/php.ini
echo "extension = oci8.so" >> /etc/php/5.6/cli/php.ini
```

### ทำการ restart service
```bash
service php5.6-fpm restart
service nginx restart
```

### หากมีปัญหา `Package 'libaio1' has no installation candidate`
```
wget http://archive.ubuntu.com/ubuntu/pool/main/liba/libaio/libaio1_0.3.112-5_amd64.deb
sudo dpkg -i libaio1_0.3.112-5_amd64.deb
```

### ตรวจสอบ OCI8 status
```bash
php --ri oci8
```
....
```bash
OCI8 Support => enabled
OCI8 DTrace Support => disabled
OCI8 Version => 2.0.8
Revision => $Id: f04114d4d62cffes4cdc2ed3b7f22f9c2c3a5016 $
Oracle Run-time Client Library Version => 12.1.0.2.0
Oracle Compile-time Instant Client Version => 12.1

Directive => Local Value => Master Value
oci8.max_persistent => -1 => -1
oci8.persistent_timeout => -1 => -1
oci8.ping_interval => 60 => 60
oci8.privileged_connect => Off => Off
oci8.statement_cache_size => 20 => 20
oci8.default_prefetch => 100 => 100
oci8.old_oci_close_semantics => Off => Off
oci8.connection_class => no value => no value
oci8.events => Off => Off
```

## 3. ตั้งค่า NGINX ให้ใช้งานกับ PHP
สร้าง folder เฉพาะของ website ที่คุณต้องการ
```bash
sudo mkdir /var/www/your_domain
```
ทำการตั้งค่าสิทธิ์การเข้าถึงให้ folder ที่เราสร้างขึ้นมา
```bash
sudo chown -R $USER:$USER /var/www/your_domain
```
สร้างไฟล์ตั้งค่าของ website ของคุณ
```bash
sudo nano /etc/nginx/sites-available/your_domain.conf
```
ทำการตั้งค่า php socket ตาม version ที่คุณต้องการ โดยไฟล์ socket จะเก็บใน `/var/run/php/` สามารถตั้งค่าตามตัวอย่างด้านล่าง
```
# Fiile: /etc/nginx/sites-available/your_domain.conf

server {
  listen 80;
  server_name www.mywebsite.com subdomain.mywebsite.com;
  client_max_body_size 100M;

  location / {
    root /var/www/mywebsite;
    index index.php;
    try_files $uri $uri/ /index.php;

    location ~ \.php$ {
      include snippets/fastcgi-php.conf;
      fastcgi_pass unix:/var/run/php/php5.6-fpm.sock;
      fastcgi_param SCRIPT_FILENAME $document_root/index.php;
    }

    location ~ /\.ht {
      deny all;
    }
  }
}
```
ทำการสร้าง shortcut link ไว้ใน `/etc/nginx/sites-enabled/` ด้วยคำสั่ง
```bash
sudo ln -s /etc/nginx/sites-available/your_domain.conf /etc/nginx/sites-enabled/
```
ตรวจสอบความถูกต้องของการตั้งค่า NGINX
```bash
sudo nginx -t
```
ทำการ Restart service ของ NGINX
```bash
sudo systemctl reload nginx
```

## Reference
1. [How to install OCI8 on Ubuntu 14.04 and PHP 5.6](http://www.syahzul.com/2016/04/06/how-to-install-oci8-on-ubuntu-14-04-and-php-5-6/)
2. [วิธีติดตั้ง PHP หลาย Version ให้ใช้งานกับ NGINX บน Ubuntu 20.04](https://github.com/midnighttime-cha/nginx-multiple-php)
