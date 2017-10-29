---
layout: post
date: 2017-10-30 00:22:00 +0700
title: Perkenalan Laradock untuk Pengembangan Website Laravel
author: baddwin
category: programming
tags: [web,framework,php,laravel,docker,laradock]
summary: "Pengembangan website atau _webapp_ menggunakan _framework_ Laravel sepertinya masih demikian populer. Tulisan ini menjelaskan cara lain untuk _development_ Laravel menggunakan teknologi Docker, atau lebih tepatnya Laradock"
--- 

## Daftar Isi

* Daftar Isi
{:toc}

## Latar Belakang

Pengembangan website atau _webapp_ menggunakan _framework_ Laravel sepertinya masih demikian populer. Karena belum ada artikel baru mengenai Laravel, penulis memberanikan diri untuk menyumbang satu artikel lagi. Tulisan ini menjelaskan cara lain untuk _development_ Laravel menggunakan teknologi Docker, atau lebih tepatnya Laradock.

## Docker dan Laradock

Docker adalah... silakan ketik kata kunci "docker adalah" di Google. Maaf bercanda.
Di SinauDev sudah ada artikel tentang [Perkenalan Docker](/linux/perkenalan_docker/) dan [Sekilas tentang Dockerfile](/linux/sekilas-tentang-dockerfile/).
Jadi, silakan bacalah artikel-artikel tersebut.

Penulis sendiri memahami Docker merupakan alternatif dari Virtualbox atau VMWare, yang lebih _resource friendly_ alias hemat sumber daya. 
Pembaca yang sudah akrab dengan Laravel pasti kenal juga dengan Vagrant, kan? 
Nah, karena laptop penulis tidak cukup kuat untuk Virtualbox, dicarilah alternatif yang lebih _affordable_ demi kenyamanan meng-kode, sehingga sampai pada pilihan Laradock.

Laradock sendiri adalah "_A Docker PHP development environment that facilitates running PHP Apps on Docker_". 
Definisi tersebut diambil dari _repository_ [Laradock di Github](https://github.com/laradock/laradock).
Jadi, Laradock ini merupakan kumpulan _Dockerfile_ untuk membuat _container_ yang dibutuhkan untuk _development_ Laravel.
Kita pasti butuh, sebut saja PHP dengan versi tertentu, Apache atau NGINX, MySQL atau Postgres atau MongoDB, dll. 

Oke, kira-kira paham ya? Lanjut.

## Set up lingkungan kerja

Syarat yang perlu dipenuhi untuk meng-kode Laravel dengan Laradock antara lain:

1. Docker
2. docker-compose
3. Sebuah projek Laravel
4. Git

dan koneksi internet (yang kencang) untuk mengunduh _image_ Docker yang dibutuhkan.

### Install Docker

Di sistem operasi Linux (atau GNU/Linux deh), Docker sudah tersedia di repository aplikasi (seharusnya demikian). 
Kita hanya perlu install dari repository distro Linux yang kita pakai. 
Misalnya penulis pakai Fedora, maka tinggal memerintahkan `sudo dnf install docker`, Enter. 
Tapi, bagi pengguna Fedora, perlu diperhatikan, bahwa Docker dari repo utama Fedora _tidak bisa digunakan_, entah kenapa. 
Maka, kita (pengguna Fedora) perlu mengunduh [Docker dari situs resminya](https://store.docker.com/editions/community/docker-ce-server-fedora), dan menginstall _package_ `docker-ce`.
Caranya agak ribet, jadi silakan baca di laman [dokumentasi resminya](https://docs.docker.com/engine/installation/linux/docker-ce/fedora/).

Bagi yang menggunakan Windows atau Mac, silakan unduh Docker dari [laman unduhan resminya](https://www.docker.com/get-docker).

### Install docker-compose

Untuk instalasi **docker-compose**, terutama pengguna Linux, silakan mengacu pada [laman resminya](https://docs.docker.com/compose/install/#install-compose). 
Versi Docker untuk Windows dan Mac sepertinya sudah menyertakan **docker-compose** dalam _installer_-nya.

## Set up projek Laravel dan Laradock

Tentukan atau buat projek Laravel yang akan memanfaatkan Laradock ini.
Tentukan pula apakah Laradock akan diletakkan di luar folder projek, atau di dalam sebagai Git submodul.
Ini hanya akan berpengaruh pada konfigurasi Laradock nantinya.

### Laradock di luar projek

Misalkan, kita punya projek Laravel di `/home/aku/Laravel`, sedangkan Laradock akan ditempatkan di sebelahnya dengan nama folder Laradock.
Maka, selanjutnya kita clone [repo Git Laradock](https://github.com/laradock/laradock) di folder baru di sebelah projek Laravel.
Jadi, dengan perintah Terminal dari `/home/aku`:

```
git clone https://github.com/laradock/laradock.git Laradock
```

Kemudian, yang perlu kita lakukan adalah membuat konfigurasi `.env`.
Cukup salin file **env-example** menjadi **.env**, dan kita hanya perlu mengubah parameter `APPLICATION`. Isikan nilainya dengan lokasi / nama folder Laravel tadi. 
Dalam hal ini, projek kita ada di `/home/aku/Laravel`. Kita bisa mengisikannya path lengkap tersebut, atau cukup `../Laravel`.

### Laradock di dalam projek

Misalkan, kita ingin Laradock menjadi submodul projek, maka kita clone Laradock ke `/home/aku/Laravel/Laradock` dengan perintah `git submodule`, dijalankan di ddalam folder projek Laravel:
```
git submodule add https://github.com/Laradock/laradock.git Laradock
```

Perintah `git submodule` dipakai, dengan asumsi bahwa projek Laravel sendiri sudah di-track dengan Git. Jika tidak, cukup ganti *submodule* dengan *clone* seperti cara di bagian sebelumnya.

Konfigurasi **.env** juga tinggal mengubah value path folder projek menjadi cukup `../`. Artinya Laradock akan membaca folder atas, yaitu folder tempat Laradock berada.

## Menjalankan Container

Untuk menjalankan container Docker yang disediakan Laradock, cukup `cd` ke folder Laradock, entah yang di luar projek atau di dalam projek. Kemudian jalankan perintah:
```
sudo docker-compose up -d nginx mysql redis elasticsearch
```

Di Fedora, penulis perlu hak akses root untuk Docker, jadi perlu `sudo`. `nginx mysql redis elasticsearch` bisa diganti sesuai kebutuhan kita. Modul-modul yang tersedia bisa dilihat dalam folder Laradock.

Setelah beberapa saat, dan melewati proses unduh *image* yang dibutuhkan, container berjalan di *background* dan bisa dicek proses yang berjalan dengan perintah:
```
sudo docker-compose ps
```

Di sana ada keterangan container apa saja yang berjalan, dan jika ada error, akan tampak juga. Pada gambar berikut, proses error ditampilkan dengan State: *Exit*.

![docker-compose-ps](/img/2017/10/docker-compose-ps.png)

## Akses ke container

Di dalam container yang disediakan Laradock, yaitu `workspace`, sudah disediakan *tools* yang diperlukan untuk *development* Laravel. Antara lain Composer, Git, NodeJS, dll.
Kita bisa menjalankan *tools* itu dengan mengakses container tersebut via SSH.
Perintah untuk masuk ke container adalah:
```
sudo docker-compose exec workspace bash
```

Maka otomatis kita dibawakan ke direktori projek Laravel tadi. Di situ kita bisa misalnya menjalankan perintah `composer install` dsb.

## Penutup

Demikian pengenalan tentang Laradock ini. Namanya saja pengenalan, maklum kalau masih banyak misteri yang mungkin masih menjadi pertanyaan.
Tetapi penulis berharap semoga tulisan ini setidaknya memberikan sebuah manfaat bagi pembaca. Barangkali akan ada sekuel untuk artikel kali ini, jika memang ada *demand*.
