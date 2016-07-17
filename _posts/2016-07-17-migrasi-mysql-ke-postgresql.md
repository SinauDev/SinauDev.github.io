---
layout: post
title: Migrasi MySQL ke PostgreSQL
author: rezha
category: pemrograman
tags: [PostgreSQL, Ubuntu]
---
Pada tahun 2013, saya memulai sebuah [proyek untuk menyimpan log percakapan IRC di kanal #ubuntu-indonesia](https://ubuntu-id.rezhajulio.web.id) karena cukup banyaknya kelas online yang diadakan disana. Seiring berjalannya waktu, saya menggunakan PostgreSQL untuk semua proyek-proyek saya dan menyisakan satu proyek ini yang menggunakan MySQL. Menjalankan MySQL dan PostgreSQL pada satu mesin bisa dibilang cukup membuang sumber daya dimana hanya satu proyek saja yang menggunakan MySQL. Oleh karena itu saya melakukan migrasi proyek ini untuk menggunakan PostgreSQL. Sudah 450.000 baris log percakapan yang tersimpan di proyek ini.

Pertama-tama kita perlu melakukan ekspor data dari MySQL dengan perintah berikut:

```
mysqldump --compatible=postgresql --default-character-set=utf8 -r namadatabase.mysql -u root namadatabase
```

Kita melakukan ekspor menggunakan `mysqldump` yang sebisa mungkin sepadan dengan sintaks PostgreSQL. Hasil dari ekspor ini disimpan sesuai nama database yang kamu berikan. Namun hasil dari ekspor ini terkadang masih memberikan error ketika dimasukkan ke PostgreSQL. Saya menggunakan sebuah _script_ Python untuk melakukan konversi.

```
wget https://raw.githubusercontent.com/lanyrd/mysql-postgresql-converter/master/db_converter.py
python db_converter.py namadatabase.mysql namadatabase.psql
```

Setelah itu, hasil konversi SQL bisa diimpor ke POstgreSQL menggunakan perintah
```
psql -f namadatabase.psql
```