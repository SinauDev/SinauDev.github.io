---
layout: post
title: Konversi .htaccess di Nginx
author: ali
category: server
tags: [gnu-linux,archlinux,nginx,apache]
summary: Bagi Anda pengguna Apache, mungkin sudah kenal dan familiar dengan berkas `.htaccess` yang mana salah satu fungsinya adalah membuat _pretty URL_ atau menghilangkan `index.php` dari URL di peramban. Pada saat penulis migrasi dari Apache ke Nginx, penulis sedikit mengalami kendala yakni salah satunya mengenai berkas `.htaccess` ini. Ya, seperti yang telah kita ketahui bahwa `.htaccess` hanya didesin untuk pengguna Apache. Jadi bagi pengguna Nginx `.htaccess` ini tidaklah berfungsi sama sekali.
---

# Daftar Isi

* daftar isi
{:toc}

# Latar Belakang Masalah

Bagi Anda pengguna Apache, mungkin sudah kenal dan familiar dengan berkas `.htaccess` yang mana salah satu fungsinya adalah membuat _pretty URL_ atau menghilangkan `index.php` dari URL di peramban. Pada saat penulis migrasi dari Apache ke Nginx, penulis sedikit mengalami kendala yakni salah satunya mengenai berkas `.htaccess` ini. Ya, seperti yang telah kita ketahui bahwa `.htaccess` hanya didesin untuk pengguna Apache. Jadi bagi pengguna Nginx `.htaccess` ini tidaklah berfungsi sama sekali.

Untuk mengantisipasi hal tersebut, kita harus mengkonversi `.htaccess` ke dalam berkas konfigurasi di setiap _virtual host_ pada Nginx. Pada artikel kali ini, penulis akan membahas mengenai hal tersebut dengan beberapa cara yang penulis lakukan selama menggunakan Nginx pada VPS [situsali.com](https://situsali.com) ini.

Ada dua cara yang dapat Anda lakukan, pertama dengan cara **manual** (penulis lebih merekomendasi ini); kemudian yang kedua dengan cara **otomatis**, menggunakan sejenis _tool converter_.

# Metode Manual

## Studi Kasus Pretty URL

Kita asumsikan dahulu misalnya di `.htaccess` dari Web seperti berikut:

```
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^barang/(.*)$ index.php?barang=$1 [L]
```
Kita terjemahkan dahulu yakni, setiap kita _request_ di peramban dengan `/barang/nama_barang_nya` itu artinya sama saja kita me-_request_ pada `/index.php?barang=$1`. Dan jika yang di-_request_ tersebut ternyata adalah sebuah berkas atau direktori, maka arahkan langsung ke berkas dan direktori tersebut.

Gambaranya seperti berikut:

[![Apache](https://situsali.com/wp-content/uploads/2016/08/situsali-apache-htaccess.png)](https://situsali.com/wp-content/uploads/2016/08/situsali-apache-htaccess.png)

Nah untuk mengkonversi ke Nginx caranya sangat simpel yakni:

```nginx
root   /srv/http/tokoku;
index index.php index.html index.htm;
 
location / {
     autoindex on;
}
 
location /barang {
    if (!-f $request_filename) {
         rewrite ^/barang/(.*)$ /index.php?barang=$1;
    }
}
```
### Penjelasan Skrip

Pada skrip di atas kita melihat `root` dengan nilai `/srv/http/tokoku`. Artinya skrip tersebut hanya berlaku pada skrip PHP yang terletak pada direktori `/srv/http/tokoku`. Jadi jika Anda akses _localhost_ hal itu langsung ke direktori tersebut. Seperti gambar di bawah ini:

[![Nginx](https://situsali.com/wp-content/uploads/2016/08/situsali-nginx-htaccess-2.png)](https://situsali.com/wp-content/uploads/2016/08/situsali-nginx-htaccess-2.png)

Lalu bagaimana jika ingin seperti Apache di atas (pada _screenshot_ di atas) ? Yang mana kita mengaksesnya dari subdirektori `http://localhost/tokoku` ? Untuk melakukan hal itu cukup mudah, tinggal Anda rubah saja skrip di atas menjadi seperti berikut:

```nginx
root   /srv/http;
 
location /tokoku/barang {
    if (!-f $request_filename) {
         rewrite ^/tokoku/barang/(.*)$ tokoku/index.php?barang=$1;
    }
}
```
Dengan mengganti `root` direktori ke `/srv/http` dan menambahkan _localtion_nya dengan `/tokoku/`. Kemudian perhatikan kembali skrip awal. Kita melihat adanya skrip:

```nginx
location / {
     autoindex on;
}
```
Itu artinya di setiap `root` jika ada direktori maka tampilkanlah sebagai direktori (_Directory Listing_). Kemudian skrip:

```nginx
location /barang {
    if (!-f $request_filename) {
         rewrite ^/barang/(.*)$ /index.php?barang=$1;
    }
}
```
Kita melihat adanya `if (!-f $request_filename)` ini artinya kita cek terlebih dahulu apakah _request_ dari peramban `/barang` itu berupa argumen atau direktori? Jika berupa argumen maka kita panggil argumen `?barang=`.

Mudah bukan?

## Studi Kasus _Redirect_

Lanjut ke contoh kasus _redirect_. Katakanlah Anda ingin semua domain Anda yang **tanpa** `www` di-redirect-kan ke `www.domainAnda.com`. Contoh pada `.htaccess` seperti skrip berikut:

```
RewriteEngine on
RewriteCond %{HTTP_HOST} ^domainku.com [NC]
RewriteRule ^(.*)$ http://www.domainku.com/$1 [L,R=301,NC]
```
Maka cukup kita terjemahkan ke pengaturan Nginx sebagai berikut:

```nginx
server {
    listen      80;
    server_name domainku.com;
    return      301 http://www.domainku.com$request_uri;
}
 
server {
    listen      80;
    server_name www.domainku.com;
}
```
### Penjelasan Skrip

Kita menggunakan dua buah `class` _server_ yang mana kedua `class` tersebut memanggil `port 80`. Jika dalam `port` tersebut ada yg me-_request domain_ **tanpa** `www`, maka kita beri kode `return 301` yang artinya memaksa _requester_ untuk masuk harus dengan _domain_ dengan `www`.

## Studi Kasus pada Drupal

Sekarang kita masuk ke salah satu CMS (_Content Management System_) yakni **Drupal**. Kita melihat berkas `.htaccess` nya sebagai berikut:

```
<FilesMatch "\.(engine|inc|info|install|make|module|profile|test|po|sh|.*sql|theme|tpl(\.php)?|xtmpl)$|^(\..*|Entries.*|Repository|Root|Tag|Template)$">
    Order allow,deny
</FilesMatch>
 
RewriteEngine on
RewriteBase /
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_URI} !=/favicon.ico
RewriteRule ^ index.php [L]
```
Sebelum diterjemahkan ke pengaturan Nginx pahami dahulu `.htaccess` di atas. Pada awal skrip tertulis semua berkas yang mengandung kata `engine`,`inc`,`info` dan sebagainya kita tidak boleh mengaksesnya (_deny_). Dan skrip di bawahnya itu standar dan mudah yakni hanya menghilangkan `index.php` dari URL pada peramban.

Untuk terjemahan ke Nginx adalah sebagai berikut:

```nginx
location ~* \.(engine|inc|info|install|make|module|profile|test|po|sh|.*sql|theme|tpl(\.php)?|xtmpl)$ {
    deny all;
}
 
location ~* ^/(\..*|Entries.*|Repository|Root|Tag|Template)$ {
    deny all;
}
 
location / {
    try_files $uri /index.php$is_args$args;
 
    ## Cara lainnya:
    #if (!-f $request_filename) {
    #   rewrite ^(.*)$ /index.php;
    #}
}
 
location /favicon.ico {
}
```
### Penjelasan Skrip

Hampir sama dengan skrip sebelumnya dan jika kita perhatikan masih mirip dengan Regex dari `.htaccess`. Yakni untuk beberapa berkas yang mengandung kata `engine`,`inc`,`info` dan sebagainya kita **blokir**. Langsung saja penulis asumsikan Anda sudah mengerti karena kasusnya mirip dengan skrip sebelumnya.  Kita lanjut dengan melihat skrip berikut di bawah:

```nginx
location / {
    try_files $uri /index.php$is_args$args;
 
    ## Cara lainnya:
    #if (!-f $request_filename) {
    #   rewrite ^(.*)$ /index.php;
    #}
}
```
Dalam `root` direktori terdapat `try_files`. Ini artinya Nginx mencoba menerjemahkan jika isi dari variabel `$uri` maka terjemahkan ke dalam argumen yang diambil dari _global variable_ `$_GET`. Untuk mengetahui variabel-variabel Nginx Anda bisa mempelajari dari tautan di bawah ini:

<http://nginx.org/en/docs/http/ngx_http_core_module.html#variables>

## Studi Kasus pada Joomla

Sekarang kita beralih ke Joomla. Kita lihat dahulu berkas `.htaccess` nya yakni:

```
RewriteBase /
RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]
RewriteCond %{REQUEST_URI} !^/index\.php
RewriteCond %{REQUEST_URI} /component/|(/[^.]*|\.(php|html?|feed|pdf|vcf|raw))$ [NC]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule .* index.php [L]
```
Jika diperhatikan skrip di atas akan sangat sederhana untuk kita terjemahkan ke pengaturan Nginx, yakni cukup:

```nginx
location / {
    try_files $uri /index.php$is_args$args;
}
```
### Penjelasan Skrip

Mengapa skrip terjemahan atau konversinya hanya sedikit? Sedangkan kita melihat dari `.htaccess` di atas skripnya lebih banyak? Kita mengabaikan skrip pada baris ke-2 dan ke-4 karena dasarnya dengan skrip `try_files` kita sudah dapat melakukan hal itu.

## Studi Kasus pada WordPress

Nah ini yang paling penting bagi Anda pengguna CMS WordPress dan memang WordPress ini CMS yang palingan banyak digunakan dalam pembuatan situs, dan [situsali.com](https://situsali.com) juga menggunakan WordPress, dan penulis juga pengguna Nginx dalam VPS situs ini.

Kita melihat `.htaccess` pada WP sangatlah sederhana yakni hanya menghilangkan `index.php` saja. Adapun skrip `.htaccess` nya adalah sebagai berikut:

```
RewriteEngine On
RewriteBase /
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /index.php [L]
```
erlihat sangat mudah untuk kita terjemahkan ke Nginx, cukup gunakan `try_files` saja. Adapun skripnya sama seperti skrip yang dilakukan pada Joomla, yakni:

```nginx
location / {
    try_files $uri /index.php$is_args$args;
}
```
### Penjelasan Skrip

Tidak perlu ada penjelasan dalam skrip WP, karena dasarnya sama seperti skrip-skrip di atas.

# Metode Otomatis

Jika Anda masih kesulitan menerjemahkan .htaccess ke Nginx karena beberapa faktor seperti kurangnya pengetahuan tentang `.htaccess` (mungkin ini akan penulis bahas pada artikel selanjutnya); dan masalah Regex. Tidak perlu khawatir, Anda dapat menggunakan _tool_, yakni:

<http://winginx.com/en/htaccess>

Dengan tool di atas, kita tidak perlu repot lagi menerjemahkan `.htaccess` karena secara otomatis tool di atas dapat melakukannya. Akan tetapi, yang penulis tekankan di sini adalah, bahwa _tool_ di atas **tidak ada jaminan dapat 100% kompatibel** dengan Nginx, ada kiranya Anda tes terlebih dahulu sebelum menggunakannya. Juga skrip di atas kita harus _online_ dalam mengaksesnya, maka dari awal pembahasan penulis merekomendasikan gunakan cara manual.

Semoga tulisan ini bermafaat 😉

# Sumber

* <https://situsali.com/konversi-htaccess-ke-nginx/>
* <https://www.nginx.com/blog/converting-apache-to-nginx-rewrite-rules/>
* <http://nginx.org/en/docs/http/ngx_http_rewrite_module.html>
* <http://winginx.com/en/docs/rewrites>

