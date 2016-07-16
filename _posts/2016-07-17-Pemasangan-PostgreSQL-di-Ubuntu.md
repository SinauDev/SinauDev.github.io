---
layout: post
title: Pemasangan PostgreSQL di Ubuntu
author: rezha
category: linux
tags: [PostgreSQL, Ubuntu]
---
### Daftar Isi
* [Pengenalan PostgreSQL](#PengenalanPostgreSQL-0)
* [Instalasi PostgreSQL](#InstalasiPostgreSQL-1)
* [Upgrade PostgreSQL](#UpgradePostgreSQL-2)


## <a name='PengenalanPostgreSQL-0'></a>Pengenalan PostgreSQL

Apakah kamu seorang _programmer_ atau _developer_ ? Tentu kamu sudah tidak asing lagi dengan _SQL_. _SQL_ atau _Structured Query Language_ adalah sebuah bahasa yang digunakan untuk mengakses data dalam basis data relasional. Bahasa ini secara de facto merupakan bahasa standar yang digunakan dalam manajemen basis data relasional. Saat ini hampir semua server basis data yang ada mendukung bahasa ini untuk melakukan manajemen datanya.

PostgreSQL adalah sebuah sistem basis data _SQL_. Piranti lunak ini merupakan salah satu basis data yang paling banyak digunakan saat ini, selain MySQL dan Oracle. PostgreSQL adalah sistem database yang kuat untuk urusan relasi, serta open source. Memiliki lebih dari 15 tahun pengembangan aktif dan sudah terbukti segala rancangan arsitekturnya telah mendapat reputasi tentang “kuat”, “handal”, “integritas data”, dan “akurasi data”.

## <a name='InstalasiPostgreSQL-1'></a>Instalasi PostgreSQL
Ubuntu sudah memiliki PostgreSQL dalam _repository_-nya, namun pada Ubuntu 14.04 yang saya gunakan, versi PostgreSQL masih pada versi 9.3. Sedangkan PostgreSQL sendiri sudah mengeluarkan versi 9.5. Kamu bisa menggunakan perintah berikut untuk mendapatkan versi terbaru PostgreSQL. Ubah nama `trusty` sesuai dengan versi Ubuntu yang kamu gunakan.

```
sudo echo 'deb http://apt.postgresql.org/pub/repos/apt/ trusty-pgdg main' > /etc/apt/sources.list.d/pgdg.list

wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | \
  sudo apt-key add -

sudo apt-get update

sudo apt-get install postgresql-9.5 pgadmin3
```

## <a name='UpgradePostgreSQL-2'></a>Upgrade PostgreSQL
Lalu bagaimana jika saya sudah terlanjur menginstall PostgreSQL dari repo Ubuntu? Kamu akan memiliki 2 _cluster_ PostgreSQL dalam 2 versi yang berbeda, tapi tidak perlu panik, saya akan membantu kamu untuk melakukan peningkatan _cluster_. Pertama-tama lakukan pengecekan _cluster_ yang ada.

```
pg_lsclusters
```

Jika kamu memiliki lebih dari 1 _cluster_, maka jalankan perintah berikut.

```
sudo pg_dropcluster 9.5 main --stop
sudo pg_upgradecluster 9.3 main
sudo pg_dropcluster 9.3 main
```

Perintah tersebut berfungsi untuk menghapus _cluster_ PostgrSQL 9.5 yang otomatis dibuat ketika melakukan pemasangan, lalu kita melakukan peningkatan versi cluster PostgreSQL 9.3 ke 9.5, dan yang terakhir kita menghapus _cluster_ PostgreSQL 9.3 yang sudah tidak digunakan lagi. Untuk memastikannya, silahkan jalankan `pg_lsclusters` lagi.

Semoga kamu bisa melakukan instalasi dan upgrade dengan lancar. Jika kamu sebelumnya pernah menggunakan MySQL (Saya juga pernah) dan ingin melakukan migrasi ke PostgreSQL, mungkin akan saya bahas di post berikutnya. _**Ciao!**_ 


_Disadur dari [blog pribadi](https://rezhajulio.id/installing-or-upgrading-postgresql-9-4-on-ubuntu-14-04/) dengan perubahan_