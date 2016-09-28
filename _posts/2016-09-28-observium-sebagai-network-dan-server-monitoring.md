---
layout: post
date: 2016-09-28 21:50:00 +0700
title: Observium Sebagai Network dan Server Monitoring
author: aan
category: server
tags: [sysadmin,network,monitoring]
summary: "Melakukan monitoring terhadap server adalah sudah menjadi pekerjaan utama untuk seorang Sysadmin. Karena akan sangat fatal sekali bila kita bahkan tidak tahu apa yang terjadi pada kondisi server yang kita miliki . Beberapa dari kita mungkin sudah pernah mendengar program sejenis `nagios`, `cacti`, `munin` dan sebagainya. Yang dimana memang powerful untuk melakukan monitoring terhadap server tanpa perlu khawatir lagi, karena kita hanya cukup bersantai mengecek data pada halaman monitoring yang sudah disediakan dan menunggu notifikasi email masuk bila ada yang bermasalah dalam server.

Pada artikel ini saya tidak akan membahas ketiga program yang sudah disebutkan diatas, tapi saya akan membahas tentang membangun network dan server monitoring dengan `Observium`."
---

# Daftar Isi

* Daftar Isi
{:toc}

## Latar Belakang

Melakukan monitoring terhadap server adalah sudah menjadi pekerjaan utama untuk seorang Sysadmin. Karena akan sangat fatal sekali bila kita bahkan tidak tahu apa yang terjadi pada kondisi server yang kita miliki . Beberapa dari kita mungkin sudah pernah mendengar program sejenis `nagios`, `cacti`, `munin` dan sebagainya. Yang dimana memang powerful untuk melakukan monitoring terhadap server tanpa perlu khawatir lagi, karena kita hanya cukup bersantai mengecek data pada halaman monitoring yang sudah disediakan dan menunggu notifikasi email masuk bila ada yang bermasalah dalam server.

Pada artikel ini saya tidak akan membahas ketiga program yang sudah disebutkan diatas, tapi saya akan membahas tentang membangun network dan server monitoring dengan `Observium`.

## Kenapa Observium?

Observium dapat mengumpulkan semua data server melalui SNMP seperti running proses, syslog, temperature dan lain-lain yang nantinya akan ditampilkan melalui web interface dan juga menggunakan RRDtool sebagai media untuk melakukan logging dan graphing. Observium mendukung banyak perangkat dengan lebih dari 267 OS tipe yang didukung autodetection dan graph sensor. Observium pun mendukung Alcatel AIP, Cisco CDP, Foundry FDP, LLDP, Juniper dan lain-lain.

Saya pikir Observium lebih lengkap dan mudah digunakan dibandingkan dengan program yang sudah disebutkan sebelumnya. Observium dibagi menjadi dua, yaitu : Observium Server dan Observium Client.

## Software Requirement

* Apache 2.2, 2.4 atau yang terbaru
* fping
* MySQL 5.1 (5.5+ sangat direkomendasikan)
* Net-SNMP 5.4+ (5.7+ is sangat direkomendasikan)
* RRDtool 1.3+ (1.5+ is sangat direkomendasikan)
* Graphviz
* PHP 5.4+ (5.6+ is sangat direkomendasikan)

Sangat disarankan agar Observium berjalan di OS terbaru untuk mendapatkan paket yang direkomendasikan. Pada artikel ini saya menggunakan CentOS 7 sebagai sarana untuk Observium. Tapi, bisa disesuaikan dengan masing-masing OS.

## Instalasi Observium Server

Seperti yang sudah disebutkan sebelumnya bahwa Observium dibagi menjadi Server dan Client, yang nantinya server akan menjadi pusat dari semua client yang terhubung. Observium yang akan digunakan adalah versi Community Edition. Bedanya, Community Edition hanya mendapatkan update 6 bulan sekali, untuk Professional Edition akan mendapatkan update per-hari selama ada update terbaru.

Ingat, selalu lakukan update paket yang berada pada server terlebih dahulu:

```
yum update
```

***

### Memasang EPEL Repositori

Karena beberapa paket membutuhkan repositori EPEL, maka kita diharuskan untuk memasang repositori tersebut terlebih dahulu:

```
rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
```

Lakukan instalasi terhadap paket yang dibutuhkan:

```
yum install wget.x86_64 httpd.x86_64 php.x86_64 php-mysql.x86_64 php-gd.x86_64 php-posix \
    php-mcrypt.x86_64 php-pear.noarch cronie.x86_64 net-snmp.x86_64 net-snmp-utils.x86_64 fping.x86_64 \
    mariadb-server.x86_64 mariadb.x86_64 MySQL-python.x86_64 rrdtool.x86_64 subversion.x86_64 jwhois.x86_64 \
    ipmitool.x86_64 graphviz.x86_64 ImageMagick.x86_64
```

Note: Silahkan hapus nama paket yang sudah terpasang agar tidak instalasi lagi

### Unduh Observium Community Edition

Buat direktori untuk Observium terlebih dahulu:

```
mkdir -p /opt/observium && cd /opt
```

Unduh dan buka arsip Observium:

```
wget http://www.observium.org/observium-community-latest.tar.gz
tar zxvf observium-community-latest.tar.gz
```

### MySQL Database

Jalankan MySQL/MariaDB dan lakukan konfigurasi agar bisa berjalan saat proses startup:

```
systemctl enable mariadb
systemctl start mariadb
```

Login ke MySQL konsol menggunakan user root

```
mysql -u root -p
```

Tapi bila MySQL baru terpasang dan belum terkonfigurasi maka lakukan perintah berikut:

```
mysql_secure_installation
```

Silahkan lewati langkah diatas bila MySQL sudah terpasang dan sudah terkonfigurasi.

Langkah selanjutnya, buat database dan berikan hak akses:

```
CREATE DATABASE observium DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
GRANT ALL PRIVILEGES ON observium.* TO 'observium'@'localhost' IDENTIFIED BY '<observium db password>';
exit;
```

### Konfigurasi Observium

Pastikan kita berada di direktori */opt/observium* atau direktori tempat observium sebelumnya. Salin berkas konfigurasi dan sesuaikan dengan yang kita miliki:

```
cp config.php.default config.php
nano config.php
```

Akan tampak seperti berikut:

```
<?php

## Check http://www.observium.org/docs/config_options/ for documentation of possible settings

// Database config ---  This MUST be configured
$config['db_extension'] = 'mysqli';
$config['db_host']      = 'localhost';
$config['db_user']      = 'USERNAME';
$config['db_pass']      = 'PASSWORD';
$config['db_name']      = 'observium';

// Base directory
#$config['install_dir'] = "/opt/observium";

// Default community list to use when adding/discovering
$config['snmp']['community'] = array("public");

// Authentication Model
$config['auth_mechanism'] = "mysql";    // default, other options: ldap, http-auth, please see documentation for config help

// Enable alerter (not available in CE)
// $config['poller-wrapper']['alerter'] = TRUE;

// Set up a default alerter (email to a single address)
$config['email']['default']        = "user@your-domain";
$config['email']['from']           = "Observium <observium@your-domain>";
$config['email']['default_only']   = TRUE;

// End config.php
```

Pada MySQL 5.7, STRICT Mode secara bawaan telah aktif. Bila melihat pada dokumentasi Observium, kita diharuskan untuk menonaktifkan STRICT mode. Untuk mengetahui apakah STRICT mode aktif atau tidak bisa mengecek variabel `STRICT_TRANS_TABLES` :

```
mysql> SELECT @@GLOBAL.sql_mode;
+--------------------------------------------+
| @@GLOBAL.sql_mode                          |
+--------------------------------------------+
| STRICT_TRANS_TABLES,NO_ENGINE_SUBSTITUTION |
+--------------------------------------------+
```

Hasil diatas menunjukkan bahwa STRICT mode dalam keadaan aktif, bila dalam keadaan nonaktif maka tidak akan mengeluarkan hasil dari query tersebut. Untuk menonaktifkan nya bisa dengan merubah berkas my.ini atau my.cnf:

```
sql-mode = "STRICT_TRANS_TABLES,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION"
```

Menjadi kosong

```
sql-mode=""
```

Kemudian restart servis MySQL.

### Setup Observium Database

Untuk melakukan setup terhadap MySQL database yang sudah kita buat sebelumnya agar terisi schema dan data dengan perintah berikut:

```
./discovery.php -u
```

```
  ___   _                              _
 / _ \ | |__   ___   ___  _ __ __   __(_) _   _  _ __ ___
| | | || '_ \ / __| / _ \| '__|\ \ / /| || | | || '_ ` _ \
| |_| || |_) |\__ \|  __/| |    \ V / | || |_| || | | | | |
 \___/ |_.__/ |___/ \___||_|     \_/  |_| \__,_||_| |_| |_|
                    Observium Community Edition 0.16.1.7533
                                   http://www.observium.org

Install initial database schema ... done.
-- Updating database/file schema
252 -> 253 ... (db) done.
253 -> 254 ... (db) done.
254 -> 255 ... (db) done.
255 -> 256 ... (php)
256 -> 257 ... (php)
257 -> 258 ... (php)
258 -> 259 ... (db) done.
259 -> 260 ... (php)
260 -> 261 ... (db) done.
261 -> 262 ... (php)
262 -> 263 ... (db) done.
263 -> 264 ... (db) done.
264 -> 265 ... (db) done.
265 -> 266 ... (db) done.
-- Done.
```

### Fping

Karena Fping berada pada lokasi yang berbeda, maka kita perlu untuk memberitahukan Observium dimana letak tersebut:

```
[root@localhost observium]# which fping
/usr/sbin/fping
```

Maka kita tambahkan seperti berikut pada berkas config.php:

```
$config['fping'] = "/usr/sbin/fping";
```

### SELinux

Disarankan untuk menonaktifkan SELinux agar Observium bisa berjalan dengan semestinya, tapi bila sudah paham untuk maintenance SELinux bisa disesuaikan. Nonaktifkan SELinux dengan perintah berikut:

```
setenforce 0
```

Dan juga nonaktifkan secara permanen SELinux dengan merubah berkas /etc/selinux/config untuk bagian SELINUX menjadi permissive:

```
SELINUX=permissive
```

### Setup RRDtool dan Apache

Buat sebuah direktori untuk menyimpan RRD data:

```
mkdir rrd
chown apache:apache rrd
```

Saya asumsikan bahwa server yang sedang dipakai adalah server kosong yang belum terisi apa-apa. Bila bukan, silahkan disesuaikan. Tambahkan  konfigurasi untuk Apache pada berkas /etc/httpd/conf/httpd.conf dibaris akhir seperti berikut:

```
<VirtualHost *:80>
   DocumentRoot /opt/observium/html/
   ServerName  observium.domain.com
   CustomLog /opt/observium/logs/access_log combined
   ErrorLog /opt/observium/logs/error_log
   <Directory "/opt/observium/html/">
     AllowOverride All
     Options FollowSymLinks MultiViews
     Require all granted
   </Directory>
</VirtualHost>
```

Buat direktori untuk log apache:

```
mkdir /opt/observium/logs
chown apache:apache /opt/observium/logs
```

Tambahkan user dengan level 10 sebagai admin:

```
./adduser.php <username> <password> <level>
```

```
Observium CE 0.16.1.7533
Add User

User admin added successfully.
```

Tambahkan perangkat yang akan kita monitoring pertama kali:

```
./add_device.php <hostname> <community> v2c
./discovery.php -h all
./poller.php -h all
```

Note: Langkah ini bisa dilewati bila kita belum melakukan instalasi terhadap Observium Client.

### Setup Cron

Untuk memudahkan dan agar data yang dikirimkan up-to-date, kita perlu memasang crontab. Tambahkan cron jobs pada berkas /etc/cron.d/observium dengan isi seperti berikut:

```
# Run a complete discovery of all devices once every 6 hours
33  */6   * * *   root    /opt/observium/discovery.php -h all >> /dev/null 2>&1

# Run automated discovery of newly added devices every 5 minutes
*/5 *     * * *   root    /opt/observium/discovery.php -h new >> /dev/null 2>&1

# Run multithreaded poller wrapper every 5 minutes
*/5 *     * * *   root    /opt/observium/poller-wrapper.py 8 >> /dev/null 2>&1

# Run housekeeping script daily for syslog, eventlog and alert log
13 5 * * * root /opt/observium/housekeeping.php -ysel

# Run housekeeping script daily for rrds, ports, orphaned entries in the database and performance data
47 4 * * * root /opt/observium/housekeeping.php -yrptb
```

Dan kemudian reload crontab:

```
systemctl reload crond
```

Tahap terakhir adalah menjalankan web server:

```
systemctl enable httpd
systemctl start httpd
```

Jika firewall aktif, maka allow http dengan perintah berikut:

```
firewall-cmd --permanent --zone=public --add-service=http
firewall-cmd --reload
```

Untuk mencobanya, kita cukup buka alamat http://observiumip . Maka akan tampil seperti berikut:

![ ](/img/observium-login.png  "Observium Login ")


Masukkan user dan password seperti yang sudah kita buat sebelumnya, maka akan tampil halaman utama. Untuk tahap Observium Server sudah selesai, selanjutnya kita beralih ke Observium Client agar bisa melakukan monitoring.

![Observium Halaman Utama](/img/observium-halaman-utama.png  "Observium Halaman Utama")

## Instalasi Observium Client

Karena data yang diambil berasal dari SNMP, maka kita harus melakukan instalasi paket SNMP. Kita bisa menjadikan localhost sebagai server dan client hanya untuk melakukan uji coba.

```
yum install net-snmp
```

Pastikan konfigurasi yang berada pada berkas /etc/sysconfig/snmpd seperti berikut:

```
OPTIONS="-Lsd -Lf /dev/null -p /var/run/snmpd.pid"
```

Ganti isi dari berkas bawaan /etc/snmp/snmpd.conf menjadi seperti ini:

```
com2sec readonly default <community>
group MyROGroup v1 readonly
group MyROGroup v2c readonly
group MyROGroup usm readonly
view all included .1 80
access MyROGroup "" any noauth exact all none none
syslocation <LOCATION>
syscontact <CONTACT>
```

Note: Apa yang ada dalam <> disesuaikan dengan masing-masing

Setelah selesai, maka jalankan servis snmpd:

```
systemctl enable snmpd
systemctl start snmpd
```

Dan allow firewall:

```
firewall-cmd --permanent --add-port=161/udp
firewall-cmd --reload
```

Proses Observium client sudah selesai.

## Menambahkan Perangkat Observium Client

Setelah Observium server dan client selesai,  langkah selanjutnya adalah menambahkan observium client kedalam observium server. Ada dua cara untuk menambahkan observium client, yaitu: Melalui CLI dan Web.

* CLI

```
./add_device.php localhost community v2c
```

Bila sukses, akan tampil seperti berikut:

```
Observium CE 0.16.1.7533
Add Device(s)

Try to add localhost:
Trying v2c community community ...
Now discovering localhost (id = 2)
#####  localhost [2]  #####

 o OS Type              linux
 o OS Group             unix
 o SNMP Version         v2c
 o Last discovery       
 o Last duration         seconds

#####  Module Start: ports  #####

 o Caching OIDs         ifDescr ifAlias ifName ifType ifOperStatus
 o Caching DB           0 ports
 o Discovering ports     lo(1)[3] enp0s3(2)[4]
+---------+--------+---------+------------------+-------------+
| ifDescr | ifName | ifAlias | ifType           | Oper Status |
+---------+--------+---------+------------------+-------------+
| lo      | lo     | ...     | softwareLoopback | up          |
| enp0s3  | enp0s3 | ...     | ethernetCsmacd   | up          |
+---------+--------+---------+------------------+-------------+


 o Duration             0.0703s

#####  localhost [2] completed discovery modules at 2016-09-27 17:20:47  #####

 o Discovery time       0.072 seconds

#####  localhost [2]  #####

 o OS Type              linux
 o OS Group             unix
 o SNMP Version         v2c
 o Last discovery       
 o Last duration         seconds

#####  Module Start: ipv4-addresses  #####

IPv4 Addresses : +N+

 o Duration             0.0346s

#####  localhost [2] completed discovery modules at 2016-09-27 17:20:47  #####

 o Discovery time       0.035 seconds

#####  localhost [2]  #####

 o OS Type              linux
 o OS Group             unix
 o SNMP Version         v2c
 o Last discovery       
 o Last duration         seconds

#####  Module Start: ipv6-addresses  #####

IPv6 Addresses : +

 o Duration             0.0378s

#####  localhost [2] completed discovery modules at 2016-09-27 17:20:47  #####

 o Discovery time       0.039 seconds

Added device localhost (2).

Devices success: 1.

```

* Web

Klik menu Devices -> Add Device

![ ](/img/observium-add-device.png  "Observium Add Device")

Note: Perlu dingat bahwa Observium ini hanya mendukung domain/subdomain saja, jadi jika tidak mempunyai domain bisa ubah `etc/hosts/` terlebih dahulu

Berikut ini akan saya lampirkan tampilan Observium:

* Observium Device Detail

![ ](/img/observium-devices-detail.png  "Observium Devices Detail")

* Observium EventLog

![ ](/img/Observium-EventLog.png  "Observium EventLog")

* Observium Health Memory

![ ](/img/Observium-Health-memory.png  "Observium Health Memory")

## Penutup

Seperti yang sudah saya sebutkan sebelumnya, bahwa Observium ini sangatlah powerful dan cocok untuk Sysadmin dalam monitoring network ataupun server. Dengan kemudahan dan fitur yang lengkap bisa menjadi pengganti server atau network monitoring yang biasa kita pakai. Silahkan eksplorasi lebih dalam bila ingin mencobanya :) . Semoga artikel ini bermanfaat :). Kritik ataupun koreksi silahkan ke [telegram.me/aancw](telegram.me/aancw)

Note: Dokumen ini dibuat menggunakan Markdown syntax dan diconvert menjadi PDF menggunakan Remarkable <3

## Unduh Artikel dalam versi PDF

Versi PDF dari artikel ini bisa diunduh disini -> [Observium Sebagai Network dan Server Monitoring](/berkas/Observium Sebagai Network dan Server Monitoring.pdf)

## Referensi
* www.observium.org/supported_devices/
* http://www.observium.org/docs/hardware_scaling/
* Introduction to SNMP -> https://nsrc.org/workshops/2012/apricot-nmm/materials/snmp.pdf
* http://srsystemadminscripts.blogspot.co.id/2016/01/observium-how-to-add-linux-client-to.html
* https://mvcp007.blogspot.co.id/2015/12/how-to-install-and-configure-snmp-on.html
