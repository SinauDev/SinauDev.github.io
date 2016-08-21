---
layout: post
title: Install Nginx, PHP dan Mariadb di Arch Linux
category: server
tags: [archlinux, nginx]
author: ali
summary: Tutorial kali ini penulis akan membahas tentang bagaimana cara memasang Nginx dan integrasi PHP dan Mariadb biasa disingkat LEMP (Linux, Nginx, Mariadb, PHP), khusus untuk pengguna Arch Linux, dengan metode yang penulis pakai saat penulis menggunakan Ubuntu/Debian _Server_ di VPS.
---

# Daftar Isi

* Daftar Isi
{:toc}

# Selayang Pandang
Tutorial kali ini penulis akan membahas tentang bagaimana cara memasang Nginx dan integrasi PHP dan Mariadb biasa disingkat LEMP, khusus untuk pengguna Arch Linux, dengan metode yang penulis pakai saat penulis menggunakan Ubuntu/Debian Server di VPS.

Di artikel ini penulis tidak menjelaskan secara mendetail apa itu Nginx dan perbandingan Nginx dengan _web server_ lainnya, karena fokus tulisan ini adalah tentang cara pemasangan (_install_) pada GNU/Linux distro Arch Linux.

Pada intinya Nginx adalah _web server_ sama halnya seperti Apache. Hanya saja kelebihan Nginx dibandingan Apache adalah (penulis ambil poin terpenting saja) pertama lebih ringan dan sangat cocok untuk Anda pengguna VPS dengan kapasitas RAM kecil; dan yang kedua pengaturan pada berkas konfigurasinya sangatlah mudah lagi sangat simpel satu konfigurasi untuk tiap-tiap virtual host, baik itu konfigurasi masalah port, direktori, eksekusi PHP, pembatasan hak akses, direktori listing dan lain-lain.

# Pemasangan Nginx, PHP dan Mariadb

```
sudo pacman -S nginx php php-fpm mariadb
```

## Module PHP Tambahan

Jika butuh tambahan _modul_e php, biasanya dipakai untuk dukungan CMS (_Content Management System_) atau PHP framework.

```
sudo pacman -S composer php-gd php-imap php-intl php-mcrypt php-apcu php-apcu-bc php-cgi php-dblib php-embed php-enchant php-fpm php-odbc php-pgsql php-phpdbg php-pspell php-snmp php-sqlite php-tidy php-xsl php-geoip php-memcache php-memcached php-mongodb
```

# Pengaturan


## Mariadb

Sebelum mengatur Nginx dengan PHP, pertama-tama kita atur dahulu Mariadb.

```
sudo mysql_install_db --user=mysql --basedir=/usr --datadir=/var/lib/mysql
```
Lalu aktifkan Mariadb nya dari Systemd.

```
sudo systemctl start mysqld
```
Jangan lupa lakukan pembuatan _password_ `root` dan pengaturan lainnya:

```
sudo mysql_secure_installation
```

## Virtual Host di Nginx

Selanjutnya buat pengaturan Nginx, di sini penulis mengikuti pengaturan dari cara yang dilakukan pada _server_ Debian (_Debian way_) yakni adanya dua buah direktori `sites-available` yang berfungsi menampung seluruh `virtual host`, dan `sites-enabled` jika `virtual host` tersebut ingin diaktifkan di Nginx.

Kita buat direktori `sites-available` dan `sites-enabled` di `/etc/nginx`:

```
sudo mkdir /etc/nginx/{sites-available,sites-enabled}
```

Untuk menjaga-jaga jangan sampai ada konfigurasi yang salah, lakukan pencandangan (_backup_) dari konfigurasi bawaannya (_default_).

```
sudo cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.bak
```
Kemudian, kita sunting berkas `nginx.conf`:

```
sudo nano /etc/nginx/nginx.conf
```
Isikan konfigurasi seperti berikut berikut:

```
#user html;
worker_processes  1;
 
#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;
 
#pid        logs/nginx.pid;
 
 
events {
    worker_connections  1024;
}
 
 
http {
    include       mime.types;
    default_type  application/octet-stream;
 
    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';
 
    #access_log  logs/access.log  main;
 
    sendfile        on;
    #tcp_nopush     on;
 
    #keepalive_timeout  0;
    keepalive_timeout  65;
 
    #gzip  on;
    
    ## Virtual Hosts
    include /etc/nginx/conf.d/*.conf;
        include /etc/nginx/sites-enabled/*;
 
}
```

Kemudian kita buat berkas _virtual host_ `default`:

```
sudo nano /etc/nginx/sites-available/default
```
Lalu isikan dengan skrip di bawah ini. Pastikan `server_name` disesuaikan dengan nama _domain_ Anda. Juga untuk `root` sesuaikan dengan direktori yang Anda inginkan.

```
server {
        listen       80 default_server;
    listen [::]:80 default_server ipv6only=on;
 
        server_name  localhost;
 
        #charset koi8-r;
 
        #access_log  logs/host.access.log  main;
        root /usr/share/nginx/html;
        index index.html index.htm;
 
        location / {
        }
 
        #error_page  404              /404.html;
 
        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   /usr/share/nginx/html;
        }
 
        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}
 
        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}
 
        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }
 
 
    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;
 
    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}
 
 
    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;
 
    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;
 
    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;
 
    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;
 
    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}
```

**Pehartian** Untuk membuat _virtual host_ lainnya hilangkan nilai `default_server` pada `listen`.

Jika sudah, kita buat symbolic link untuk mengaktifkan virtual host di atas.

```
sudo ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled/
```
Lalu _restart_ Nginx.

```
sudo systemctl restart nginx
```

##  Integritas Nginx dengan PHP

Pada konfigurasi di atas, kita hanya dapat menjalankan Nginx untuk berkas `HTML` saja. Untuk dapat mengeksekusi `PHP` diperlukan `php-fpm` diaktifkan di setiap pengaturan _virtual host_.

Pertama-tama kita perlu menyunting `php.ini`:

```
sudo nano /etc/php/php.ini
```
Cari kata `cgi.fix_pathinfo`, ubah menjadi bernilai **nol**:

```
cgi.fix_pathinfo 0
```
Lihat gambar di bawah ini:
[![alt cgi.fix_pathinfo](https://situsali.com/wp-content/uploads/2016/08/situsali-nginx-php-1.png "cgi fix path info")](https://situsali.com/wp-content/uploads/2016/08/situsali-nginx-php-1.png)

Kemudian sunting _socket listen_ pada `php-fpm`.

```
sudo nano /etc/php/php-fpm.d/www.conf
```
Cari `listen =` dan ganti menjadi seperti berikut:

```
listen = /var/run/php-fpm/php-fpm.sock
```
Lihat gambar di bawah ini:
[![alt socket](https://situsali.com/wp-content/uploads/2016/08/situsali-nginx-php-2.png "socket listen")](https://situsali.com/wp-content/uploads/2016/08/situsali-nginx-php-2.png)

Setelah itu _restart_ `php-fpm`.

```
sudo nano /etc/nginx/sites-available/default
```
Dan tambahkan skrip berikut:

```
index index.php index.html index.htm;
    
location ~ \.php$ {
        try_files $uri =404;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass unix:/var/run/php-fpm/php-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
}
```
Lihat gambar di bawah ini:
[![alt PHP Nginx](https://situsali.com/wp-content/uploads/2016/08/situsali-nginx-php-3.png "PHP Nginx")](https://situsali.com/wp-content/uploads/2016/08/situsali-nginx-php-3.png)

Untuk memastikan bahwa konfigurasi Anda sudah benar atau belum dapat melakukan pengecekan Nginx dengan cara berikut:

```
sudo nginx -t
```
Jika sudah seperti tulisan di bawah ini, berarti Nginx Anda sudah benar:

```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```
Langsung saja _restart_ Nginx.

```
sudo systemctl restart nginx
```

# Pengecekan

Untuk memastikan bahwa Nginx Anda sudah benar-benar dapat menjalankan PHP atau belum, Anda dapat membuat berkas `index.php` di:

```	
sudo nano /usr/share/nginx/html/index.php
```
Lalu isi dengan skrip di bawah ini.

```php
<?php
phpinfo();
```
Lihat gambar di bawah ini:
[![alt PHP Nginx](https://situsali.com/wp-content/uploads/2016/08/situsali-nginx-php-4.png "PHP Nginx")](https://situsali.com/wp-content/uploads/2016/08/situsali-nginx-php-4.png)

Jika sudah seperti gambar di atas, artinya Anda telah sukses. Semoga bermanfaat. 😉

# Sumber

Bersumber dari <https://situsali.com/memasang-nginx-php-dan-mariadb-di-arch-linux/> Tulisan ini langsung ditulis oleh pemilik [situsali.com](https://situsali.com), ditulis kembali di sini dengan sedikit penyuntingan.

