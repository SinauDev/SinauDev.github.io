---
layout: post
title: Install Apache,Mariadb,PHP dan phpMyAdmin di Ubuntu 16.04 (Disertai Video)
author: ali
category: server
tags: [ubuntu, apache]
summary: Tutorial kali ini, penulis menyajikan mengenai tentang bagaimana cara memasang (_install_) LAMP (Linux, Apache, Mariadb, PHP) dan phpMyAdmin di Ubuntu 16.04.
---

# Daftar Isi

* Daftar Isi
{:toc}

# Selayang Pandang

Tutorial kali ini, saya menyajikan mengenai tentang bagaimana cara memasang LAMP (Linux, Apache, Mariadb, PHP) dan phpMyAdmin di Ubuntu 16.04.

Sebenarnya, pembahasan mengenai cara penginstalan LAMP di GNU/Linux distro Ubuntu memang sudah banyak sekali, jika kita mencarinya di mesin pencarian `Google`. Meski demikian, penulis tetap menuliskan di sini. Tujuannya adalah mempermudah pencarian rujukan di saat penulis menulis artikel lainnya yang masih berhubungan 😆.

# Install LAMP dan phpMyAdmin

Lakukan instalasi seluruhnya apache, mariadb, php dan phpmyadmin dengan cara berikut:

```
sudo apt install apache2 php libapache2-mod-php mariadb-server php7.0 php7.0-cli php7.0-common php7.0-curl php7.0-gd php7.0-mysql php7.0-mbstring php7.0-mcrypt php-gettext phpmyadmin
```

Begitu selesai proses pemasangan, silahkan cek di peramban (_browser_) Anda dengan memasukan URL `localhost`:

<http://localhost/>

Lihat seperti gambar di bawah ini:
![alt lamp](https://situsali.com/wp-content/uploads/2016/06/apache-on.png "LAMP")

# Pengaturan

## Mariadb

Pertama-tama atur dahulu MariaDB kita yakni mengeset _password_ pada akun `root`.

```
sudo mysql_secure_installation
```

Jika terjadi galat (_error_) pada phpMyAdmin yang mana tidak dapat masuk dengan akun `root`. Anda bisa mengikuti cara di sini <https://situsali.com/mengatasi-access-denied-for-user-rootlocalhost-pada-phpmyadmin/>

## Apache

Pertama-tama candangkan (_backup_) dahulu berkas `default` dari _virtual host_ dengan cara berikut:

```
cd /etc/apache2/sites-available/
sudo cp 000-default.conf 000-default.conf.bak
```
Lalu ubah nama berkas `000-default.conf` dengan nama misalnya `lokal.conf`.

```
sudo mv 000-default.conf lokal.conf
```
Dan sunting berkas `lokal.conf` seperti kode di bawah ini:

```apache
<VirtualHost *:80>
 
ServerAdmin webmaster@localhost
DocumentRoot /code/web
 
<Directory /code/web>
    Options +Indexes +FollowSymLinks
    AllowOverride All
    Order allow,deny
    allow from all
</Directory>
 
</VirtualHost>
```
Perhatikan skrip di atas pada baris **ke-4** yakni `DocumentRoot /code/web`. Kode itu disesuaikan dengan alamat direktori Anda, jika ingin dirubah misalnya ke `/opt` tinggal ganti saja. 

Skrip di atas sudah mendukung `directory Index` berserta `.htaccess` jadi bisa langsung Anda gunakan untuk keperluan `PHP framework` ataupun CMS (_Content Management System_).

Agar skrip di atas dapat berjalan dengan baik perlu disunting pula pada berkas `apache2.conf` yakni:

```
sudo gedit /etc/apache2/apache2.conf
```
Lalu tambahkan skrip berikut:

```apache
<Directory /code/web/>
    Options Indexes FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>
```

Setelah itu kita _disable_ `000-default` dan jadikan `lokal.conf` sebagai _default_ lakukan perintah berikut:

```
sudo a2dissite 000-default
sudo a2ensite lokal
```
Kemudian _restart_ Apache Anda:

```	
sudo systemctl restart apache2
```

# Vidoe Tutorial

Untuk mempermudah Anda bisa lihat video tutorial yang telah penulis buat berikut, durasi 14 menit 52 detik.

<iframe width="730" height="410" src="https://www.youtube.com/embed/E2jeXkfPX64" frameborder="0" allowfullscreen></iframe>

# Sumber

<https://situsali.com/install-lamp-linux-apache-mariadb-php-dan-phpmyadmin-di-ubuntu-16-04-disertai-video/>

