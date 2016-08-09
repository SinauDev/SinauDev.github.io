---
layout: post
title: Sekilas Tentang Dockerfile - [2]
author: aan
category: linux
tags: [Docker, DevOps]
---

## Sekilas Tentang Dockerfile

Setelah kita mengetahui seluk-beluk Docker pada seri sebelumnya, maka pada seri ini akan membahas tentang Dockerfile dan penggunaannya. Dockerfile adalah sebuah berkas utama dalam Docker yang memuat perintah-perintah atau skrip yang akan dilakukan ketika sebuah _image_ dibuat. Dockerfile tak lain merupakan sebuah berkas yang berisikan perintah seperti kita melakukan instalasi terhadap suatu aplikasi ataupun konfigurasi. Contoh, bila kita ingin melakukan instalasi _web server_ seperti Apache, hal pertama yang harus kita lakukan adalah melakukan pembaruan terhadap repositori dengan menggunakan `apt-get update` atau `yum update` dan sejenisnya yang bisa disesuaikan dengan sistem operasi masing-masing. Setelah itu, kita akan melakukan instalasi Apache dengan menggunakan `apt-get install apache2` atau `yum install httpd`.

Itu adalah jenis perintah yang akan kita temukan pada sebuah Dockerfile. Sesuatu seperti _command_, memanggil skrip lain, mengatur _enviroment_ pada sistem operasi, menambahkan berkas, mengatur hak akses dan itu semua bisa dilakukan dengan Dockerfile. Seperti yang sudah dijelaskan pada seri sebelumnya, bahwa dengan Docker kita bisa memilih sistem operasi yang akan kita pakai sesuai dengan kebutuhan. Pada contoh ini kita akan menggunakan CentOS sebagai bahan uji coba. Namun, pada seri ini hanya akan menjelaskan sekilas tentang Dockerfile dan untuk pengoperasiannya akan dijelaskan pada seri selanjutnya. Berikut adalah contoh Dockerfile untuk melakukan instalasi dan menjalankan apache2/httpd pada CentOS:

```
FROM centos:latest
MAINTAINER http://www.centos.org
LABEL Vendor="CentOS"
LABEL License=GPLv2
LABEL Version=2.4.6-31

RUN yum -y update && yum clean all
RUN yum -y install httpd && yum clean all

EXPOSE 80
CMD ["/usr/sbin/apachectl", "-D", "FOREGROUND"]
```
Dockerfile diambil dari : [https://github.com/CentOS/CentOS-Dockerfiles/blob/master/httpd/centos7/Dockerfile](https://github.com/CentOS/CentOS-Dockerfiles/blob/master/httpd/centos7/Dockerfile)

Ada beberapa hal yang mungkin akan sering kita temukan pada Dockerfile. Pada baris pertama menjelaskan sistem operasi yang kita pakai saat menjalankan Docker, pada contoh kali ini kita akan menggunakan CentOS versi terbaru yaitu CentOS 7. Bila ingin menggunakan CentOS 6 bisa menggantinya dengan `FROM centos:centos6` atau bila ingin menggunakan Ubuntu bisa dengan `FROM ubuntu:latest`. Itu semua tergantung dari kebutuhan dan keinginan kita. Pada baris kedua menjelaskan tentang siapa yang mengelola(baca: maintainer) Dockerfile tersebut. Bila kita ingin mencatumkan informasi pribadi bisa mengganti informasi tersebut sesuai dengan yang kita inginkan, biasanya mencatumkan nama dan email agar mudah untuk dihubungi ketika Dockerfile tersebut mengalami masalah. Pada License menyatakan lisensi yang dipakai oleh aplikasi tersebut dan Version menyatakan versi yang akan kita pasang.

Untuk baris selanjutnya menjelaskan untuk melakukan pembaruan terhadap sistem operasi dan melakukan pembersihan terhadap _package_ yang memang tidak digunakan. Dalam Dockerfile lebih disarankan untuk menggabungkan proses menjadi satu dibandingkan dengan memisahkan nya. Maka dari itu kita menggunakan `&&` untuk menggabungkan proses, sama seperti kita menjalankan perintah di GNU/Linux. Pada baris selanjutnya melakukan instalasi apache2/httpd dan membersihkan _package_ ketika selesai dipasang. `EXPOSE` memberitahukan bahwa Docker tersebut menggunakan port tertentu untuk mengaktifkan jaringan antara proses yang sedang berjalan dengan jaringan yang ada, sederhana nya Docker melakukan _binding port_.

Ada pun baris terakhir merupakan perintah yang akan berjalan ketika _container_ dijalankan. Perintah ini hanya boleh dideklarasikan sekali saja, karena Dockerfile hanya akan melakukan eksekusi pada perintah CMD terakhir. Ini dilakukan untuk menjaga agar dalam satu _container_ hanya berjalan satu proses.

Kita pun mendefinisikan bahwa proses tersebut berjalan secara Foreground agar apache tetap berjalan secara foreground. Kenapa kita menggunakan `/usr/sbin/apachectl` dibandingkan dengan `service start httpd`? Padahal kan lebih mudah dan terbiasa menggunakan `systemctl start httpd` bukan?. Kita tidak bisa melakukan hal tersebut, bila kita melakukannya maka container akan berjalan dan kemudian berhenti. Itu dikarenakan tidak ada yang bisa dijalankan oleh _container_. Kita diharuskan melakukan deklarasi perintah seperti `ENV` untuk memberitahukan variabel _system enviroment_ yang dipakai. Untuk penjelasan perintah Dockerfile dan praktik akan kita bahas pada seri selanjutnya.

Referensi:

* [https://aancw.gitbooks.io/docker-101-id/content/contents/Dockerfile.html](https://aancw.gitbooks.io/docker-101-id/content/contents/Dockerfile.html)
* [https://docs.docker.com/engine/reference/builder/](https://docs.docker.com/engine/reference/builder/)
