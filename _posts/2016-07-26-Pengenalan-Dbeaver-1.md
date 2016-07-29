---
layout: post
title: Pengenalan DBeaver Part 1
author: aries
category: linux
tags: [sql client]
---

Mungkin salah satu plikasi wajib bagi para programmer adalah _sql client_, aplikasi yang membantu para programmer dalam menangani database yang digunakan. Cukup banyak jenis dari aplikasi _sql client_ ini dari yang mulai _web based_ sampai dengan _desktop_, dari yang berbayar sampai dengan yang gratis. Pada tulisan pertama saya ini saya mencoba untuk membahas salah satu _sql client desktop_ yang bernama Dbeaver.

## Kenapa DBeaver ?

Kenapa saya memilih DBeaver untuk dibahas ? Sederhana, karena beberapa hari yang lalu saya baru  mencoba DBeaver dari sebelumnya saya menggunakan mysql workbench. Beberapa poin kenapa saya memutuskan mencoba DBeaver diantaranya :

1. Mendukung banyak jenis database dari mulai MySQL, Postgre, MS SQL Server, dan bahkan mendukung juga database nosql seperti mongodb. Khusus yang nosql kita harus pasang yang edisi enterprise. Meskipun dilabeli _enterprise_ statusnya masih _free to use_ tapi tidak _open-source_.

2. DBeaver juga punya banyak versi yang dapat dijalankan di berbagai sistem operasi seperti Windows, Linux, Mac OS X, dan Solaris.

3. Tampilan yang cukup _user friendly_ ( subjektif ).

## Fitur

Untuk pembahasan fitur ini saya akan bagi ke dalam beberapa kelompok seperti berikut :

1. Fitur Global : Fitur yang dibahas meliputi koneksi, driver database, pengaturan user, Administer, dan sistem info.
2. Fitur di Database :  Fitur yang dibahas adalah diagram, dan tabel ( DDL dan DML ).
3. Fitur Lainnya : Fitur yang dibahas adalah `Views`, `Function & Procedure`, `Import & Exports`.

Untuk tulisan ini saya akan membahas poin nomor satu terlebih dahulu.

**1.Koneksi**

DBeaver ini mendukung untuk melakukan sambungan ke beberapa server database sekaligus. Di sini saya mencoba menyambungkan ke dua “server” yaitu localhost saya sendiri dan satu lagi database di homestead ( vagrant ).
Untuk membuat koneksi ada tiga cara, yang pertama melalui menu bar file→new→Dbeaver→Database connection atau, klik kanan pada kolom `Database Navigator` lalu pilih `Create New Connection`. Dan cara terakhir adalah langsung memilih _icon_ `New Connection`.

Setelah memilih menu tersebut akan muncul dialog pilihan driver yang akan digunakan, karena saya hanya ada MySQL maka saya pilih MySQL. Setelah itu akan muncul dialog konfigurasi untuk mengisikan `server host`, `port`, `database default`, `username`, dan `password`. Dialog selanjutnya ada pilihan untuk mengaktifkan `SSH Tunnel`, `Proxy`, dan `SSL`, tapi untuk tahap ini bisa dilewati. Terakhir beri nama koneksi yang dibuat dan tetukan jenis koneksi. Saat ini tersedia tiga jenis koneksi : `Development`, `Testing`, `Production`.

Setelah berhasil maka pada kolom `Database Navigator` akan muncul daftar koneksi yang tadi dibuat, dan masing-masing koneksi memiliki sub menu `Database`, `Users`, `Administer`, dan `System Info`.

**2. Driver Database**

Seperti yang disinggung di atas setidaknya terdapat 33 _driver_ yang disediakan oleh aplikasi ini. Dan jika kita menggunakan yang versi _enterprise_ akan didapatkan tambahan _driver_ untuk database nosql seperti MongoDB dan Cassandra.

**3. Pengaturan User**

Dengan aplikasi ini kita dapat dengan mudah menambahkan atau mengubah `user`. Pertama pilih menu `“Users”`  maka pada kolom utama akan muncul daftar `user` yang terdaftar. 

Untuk mengubah `user`, klik dua kali pada `user` yang dipilih maka akan muncul tampilan edit `user` yang berisi `user name`, `host`, `password`, dan `user privileges`. Sedangkan untuk menambah `user` baru cukup klik kanan pada kolom utama dan pilih `Create New User`.

**4. Administer**

Fitur ini berisi informasi _session_ pada database yang digunakan.

**5. System Info**

Sesuai dengan namanya menu ini lebih berisi informasi-informasi umum dari database yang digunakan seperti `session status`, `global status`, `charset`, `user privileges`, dan lain sebagainya.

##
Tulisan pertama rasanya cukup sampai di sini, pada tulisan selanjutnya saya akan coba bahas fitur nomor dua. Pada fitur tersebutlah kita akan banyak bermain dengan DDL ( `Data Definition language`) dan DML ( `Data Manipulation Language` ).

Terakhir mohon maaf jika terdapat informasi yang salah dan silahkan beritahu saya agar saya dapat langsung mengoreksinya.

Terima Kasih.

Referensi :

* http://dbeaver.jkiss.org/docs/features/
