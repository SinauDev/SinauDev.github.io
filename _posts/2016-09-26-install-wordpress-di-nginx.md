---
layout: post
date: 2016-09-26 11:17:42 +0700
title: Install Wordpress di Nginx
author: ali
category: server
tags: [php,nginx,gnu-linux]
summary: "Menggunakan Nginx sebagai _web server_ memiliki keunggulan tersendiri dibandingkan dengan menggunakan Apache. Salah satu keunggulan yang paling menonjol adalah ada pada performa dan kecepatan yang diberikan, dan sangat cocok bagi pengguna _server_ dengan _resource_ yang kecil seperti kapasitas RAM kurang dari 1 GB. 
<br/><br/>
Tulisan kali ini kita akan membahas mengenai cara pemasangan Wordpress di Nginx yang disertai dukungan _pretty_ URL pada postingan di Wordpress tersebut, seperti menghilangkan `index.php` pada URL."
---

# Latar Belakang

Menggunakan Nginx sebagai _web server_ memiliki keunggulan tersendiri dibandingkan dengan menggunakan Apache. Salah satu keunggulan yang paling menonjol adalah ada pada performa dan kecepatan yang diberikan, dan sangat cocok bagi pengguna _server_ dengan _resource_ yang kecil seperti kapasitas RAM kurang dari 1 GB.

Meskipun Nginx cepat bukan berarti **tidak bagus** dibandingkan Apache, malahan hampir bisa disejajarkan dengannya. Seperti yang kita tahu, bahwa Apache memiliki keunggulan yang terletak pada banyaknya _module_ yang ia didukung, dan Apache juga sudah lebih dahulu ada dibandingkan dengan Nginx.

Tidak dipungkiri lagi setiap kelebihan pasti ada kekurangan, begitu juga dengan Nginx. Salah satu kekurangan yang dimiliki Nginx adalah tidak ada dukungan `.htaccess` (baca: [Konversi .htaccess ke Nginx](/server/konversi-htaccess-nginx/)), dengan demikian kita akan kesulitan dalam hal pembuatan _pretty_ URL seperti menghilangkan `index.php` dan menghilangkan `GET URL Paramenter`.

Tulisan kali ini kita akan membahas mengenai cara pemasangan Wordpress di Nginx yang disertai dukungan _pretty_ URL pada postingan di Wordpress tersebut, seperti menghilangkan `index.php` pada URL.

# Daftar Isi

* Daftar Isi
{:toc}

# Apa itu Wordpress

Wordpress adalah aplikasi sumber terbuka (_open source_) yang dibangun menggunakan bahasa pemerograman PHP. Ia merupakan salah satu CMS (_Content Management System_) yang cukup populer dipakai sebagai pembuatan _website_.

Pada awalnya Wordpress digunakan untuk media _blog_, kini Wordpress telah berkembang tak hanya sekadar untuk situs pembuatan _blog_, melainkan juga untuk berbagai macam situs seperti _company profile_, _e-commerce_, dan forum.

# Persyaratan

Untuk memulai melakukan tutorial ini, ada beberapa persyaratan yang harus Anda miliki.

1. Memasang Nginx, PHP dan MariaDB/MySQL. Bisa Anda lihat pada tulisan saya sebelumnya yakni: [Memasang Nginx, PHP dan Mariadb di Arch Linux](/server/install-nginx-di-arch-linux/).
2. Server atau VPS setidaknya memiliki kapasitas RAM paling kecil 128 MB, akan tetapi saya menyarankan untuk lebih dari itu. Saya sendiri menggunakan VPS dengan RAM 512 MB.
3. Masang `Wget`.

# Tahap Unduhan

Pertama-tama kita unduh terlebih dahulu Wordpress-nya:

```bash
$ wget https://wordpress.org/latest.tar.gz
```

Kemudian ekstrak:

```bash
$ tar xvzf latest.tar.gz
```

Lalu langsung saja kita pindahkan misalnya di direktori _default_ Nginx yakni di:

```
/usr/share/nginx/html
```
Direktori tersebut berlaku untuk berbagai distro GNU/Linux.

```bash
$ sudo mv wordpress /usr/share/nginx/html
```

# Tahap Konfigurasi

Karena sebelumnya kita memindahkan satu direktori Wordpress ke `/usr/share/nginx/html`. Jadi kita perlu mengkonfigurasi _virtual host_ dari berkas yang bernama `default` yakni:

## Pengguna Debian/Ubuntu

```bash
$ sudo vim /etc/nginx/sites-available/default
```

## Pengguna Centos/Fedora/RHEL dan Arch Linux

Kebanyakan pengguna distro Non Debian/Ubuntu konfigurasi _virtual host_ terletak pada:

```bash
$ sudo vim /etc/nginx/nginx.conf
```

Ganti `root` direktori dengan `/usr/share/nginx/html/wordpress;` Juga `index` tambahkan `index.php`. Seperti berikut:

```
...

root /usr/share/nginx/html/wordpress;
index index.php index.html index.htm;

...
```

Kemudian `location / {}` tambahkan ini:

```
...

location / {
  try_files $uri /index.php$is_args$args;
}

...
````

Skrip di atas agar Wordpress Anda dapat menggunakan _pretty_ URL.

Kemudian tambahkan ini, agar Nginx dapat menjalankan PHP:

```
location ~ \.php$ {
	fastcgi_split_path_info ^(.+\.php)(/.+)$;
	
	# Pengguna Ubuntu 14.04
	# fastcgi_pass unix:/var/run/php5-fpm.sock;
	
	# Pengguna Ubuntu 16.04
	# fastcgi_pass unix:/var/run/php7.0-fpm.sock;
	
	# Pengguna Arch Linux
	# fastcgi_pass unix:/var/run/php-fpm/php-fpm.sock;
	
	# Jika Anda tidak tahu, coba cek di direktori /var/run/
	# ls -l /var/run/
	# fastcgi_pass unix:/var/run/ ...
	
	fastcgi_index index.php;
	include fastcgi_params;
}
```
Perhatikan kode di atas untuk kode `fastcgi_pass`, saya tulis untuk berbagai distro. Sengaja saya beri _comment_ (tanda pagar `#`) karena di sesuaikan dengan distro yang Anda gunakan. Ada sedikit perbedaan letak _socket_ dari `php-fpm` ini. Sesuaikan dengan distro yang Anda gunakan. Umumnya semua berada pada direktori `/var/run`. Jika sudah benar, maka uncomment salah satu dari kode di atas.

Kemudian ubah semua _permission_ **direktori 755** sedangkan **berkas 644**, yakni dengan cara berikut:

```bash
$ cd /usr/share/nginx/html/wordpress
$ sudo find . -type d -exec chmod 0755 {} \;
$ sudo find . -type f -exec chmod 0644 {} \;
```

Kemudian jangan lupa pula `owner` nya ubah ke `www-data` (bagi pengguna Debian/Ubuntu) dan `http` (bagi pengguna Arch Linux/RHEL/Fedora/Centos ). Saya asumsikan Anda hanya pengguna Wordpress sendiri. Dalam artian lain, tidak ada user lain selain Anda yang menggunakan Wordpress.

```bash
$ sudo chown http:http -R /usr/share/nginx/html/wordpress/*
```

# Tahap Pembuatan Database

Sebelum beralih ke tahap instalasi Anda harus membuat databasesnya terlebih dahulu, kita gunakan Mariadb/MySQL.
Pertama-tama login dahulu MariaDB/MySQL:

```bash
$ sudo mysql -u root -p'Password Anda'
```

Dan kita buat databasenya:

```sql
mysql > create database wordpress_db;
```

Kita buatkan `username` juga:

```sql
mysql > create user wp_user_admin@localhost identified by 'PasswordAnda';
mysql > grant all privileges on wordpress_db.* to wp_user_admin@localhost;
mysql > flush privileges;
```

Lalu keluar dengan mengetik perintah:

```sql
mysql > \q
```

# Tahap Instalasi Wordpress

Sekarang baru kita memasuki tahap pamasangan WP. Langsung saja buka _domain_ atau `IP` _address_ Anda di peramban (_browser_):

![wordpress](https://statis.situsali.comhttps://statis.situsali.com/imgs/situsali-wp-nginx-install-1.png)

Klik `Let's Go`. Kemdian,

![wordpress](https://statis.situsali.com/imgs/situsali-wp-nginx-install-2.png)

Masukan `nama database`, `username`, dan `password` yang telah kita buat sebelumnya. Kemudian masalah `table prefix`, bebas Anda isi apa saja asalkan tambahkan _underscore_ (garis bawah) setelahnya. Lalu Klik `Submit`. Dan,

![wordpress](https://statis.situsali.com/imgs/situsali-wp-nginx-install-3.png)

Langsung saja, klik `Run the Install`.

![wordpress](https://statis.situsali.com/imgs/situsali-wp-nginx-install-4.png)

Masukan informasi yang dibutuhkan di atas. Jika sudah, langsung klik `Install Wordpress`.

![wordpress](https://statis.situsali.com/imgs/situsali-wp-nginx-install-5.png)

Jika sudah seperti gambar di atas, Anda bisa langsung `log In`. Dan hasilnya akan seperti gambar di bawah ini:

![wordpress](https://statis.situsali.com/imgs/situsali-wp-nginx-install-6.png)

Jika berhasil Anda akan diarahkan ke halaman administrasi.

## Tambahan

Untuk mempercantik WP kita, bisa buat prety URL dari `Settings` -> `Permalink` dan pilih:

![wordpress](https://statis.situsali.com/imgs/situsali-wp-nginx-pretty-url-1.png)

Disesuaikan dengan situs Anda. Penulis lebih memilih `post name`, karena menurut penulis ini pilihan yang baik untuk situs yang tidak terlalu sering di-_update_. Jika situs Anda seperti situs berita baiknya pilih day and name. Karena dengan demikian terlihat artikel Anda apakah sudah lama atau belum.

![wordpress](https://statis.situsali.com/imgs/situsali-wp-nginx-pretty-url-2.png)

Semoga bermanfaat 😄
