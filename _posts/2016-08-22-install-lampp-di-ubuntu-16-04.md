---
layout: post
title: Install Apache, Mariadb, PHP dan phpMyAdmin di Ubuntu 16.04 (Disertai Video)
category: server
tags: [ubuntu, apache]
author: ali
---

# Daftar Isi

* Daftar Isi
{:toc}

# Selayang Pandang

Tutorial kali ini, saya menyajikan mengenai tentang bagaimana cara memasang LAMP (Linux, Apache, Mariadb, PHP) dan phpMyAdmin di Ubuntu 16.04.

Sebenarnya, pembahasan mengenai cara penginstalan LAMP di GNU/Linux distro Ubuntu memang sudah banyak sekali, jika kita mencarinya di mesin pencarian `Google`. Meski demikian, penulis tetap menuliskan di sini. Tujuannya adalah mempermudah pencarian rujukan di saat penulis menulis artikel lainnya yang masih berhubungan.

# Install LAMP dan phpMyAdmin

Lakukan instalasi seluruhnya apache, mariadb, php dan phpmyadmin dengan cara berikut:

```
sudo apt install apache2 php libapache2-mod-php mariadb-server php7.0 php7.0-cli php7.0-common php7.0-curl php7.0-gd php7.0-mysql php7.0-mbstring php7.0-mcrypt php-gettext phpmyadmin
```

Begitu selesai proses pemasangan, silahkan cek di peramban (_browser_) Anda dengan memasukan URL `localhost`:
