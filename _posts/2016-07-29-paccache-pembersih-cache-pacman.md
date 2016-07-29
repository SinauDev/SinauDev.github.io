---
layout: post
title: Paccache, Pembersih Cache Pacman
author: aan
category: linux
tags: [Arch Linux, Pacman]
---

Hari ini saya melakukan rutinitas bersih-bersih hardisk dikarenakan semakin menipis nya ruang lingkup yang tersisa. Untuk hardisk berukuran 750GB, hanya tersisa 44GB. Seperti biasa saya melakukan peninjauan terhadap besaran berkas yang ada pada hardisk untuk mengetahui partisi atau folder mana yang mempunyai ukuran lebih besar. Di KDE sendiri sudah disediakan `software` bernama Filelight untuk melakukan peninjauan berkas yang ada pada hardisk.

![Hasil Pemindaian Filelight](/img/Hasil Filelight.png)

Setelah proses pemindaian yang dilakukan oleh Filelight selesai, saya melakukan peninjauan terhadap setiap grafik yang diberikan. Tapi rasanya ada yang menarik, pada direktori `/var/cache/pacman/pkg` mempunyai ukuran berkas sebesar 24.9 GiB dan dengan jumlah 6775 berkas. Kok bisa sebesar itu ya? Ketika saya lakukan pengecekan terhadap direktori tersebut ternyata berisi `package archive` yang sudah terpasang dari versi awal saat melakukan pemasangan aplikasi. Pantas saja kalau begitu, akhirnya saya coba baca-baca di wiki Arch Linux untuk permasalahan tersebut.

Terkadang `package archive` tersebut diperlukan ketika kita ingin melakukan pemasangan aplikasi kembali atau melakukan `downgrade` terhadap versi aplikasi yang terpasang dengan software yang bernama `downgrade` . Bila kita tidak membutuhkan lebih baik dibersihkan saja daripada hanya memenuhi hardisk. Pada wiki Arch Linux menyebutkan bahwa untuk melakukan pembersihan terhadap `package archive` bisa menggunakan perintah `pacman -Sc` yang dimana akan melakukan pembersihan keseluruhan dan hanya menyisakan berkas arsip dengan versi yang sedang terpasang dan perintah `pacman -Scc` untuk melakukan pembersihan berkas arsip tanpa terkecuali.

Karena keterbatasan tersebut, maka disarankan untuk menggunakan `paccache` yang sudah disediakan oleh `pacman` itu sendiri. `paccache` dapat melakukan pembersihan semua berkas arsip yang terpasang ataupun sudah tidak terpasang dan bisa diatur untuk jumlah versi yang akan dipertahankan dalam pembersihan, secara bawaan akan mempertahankan 3 versi pada berkas arsip yang ada dengan perintah berikut:

```
paccache -r
```

Nah itu berguna juga bila kita memasang `downgrade` untuk melakukan penurunan versi pada aplikasi. Tapi jumlah versi yang dipertahankan bisa diatur saat melakukan pembersihan dengan perintah:

```
paccache -rk 1
```

1 adalah jumlah versi yang dipertahankan. Setelah dilakukan, kurang lebih hasilnya akan seperti berikut:

```
[root@KotakRusak pkg]# paccache -rk 1

==> finished: 5193 packages removed (disk space saved: 20.57 GiB)
```

Untuk lebih lanjut bisa dengan membaca help:

```
paccache -h
```

Dengan begini, berkurang lah ukuran berkas yang ada pada hardisk dan itu sedikit meringankan ruang lingkup yang ada. Akhir kata, semoga artikel ini bermanfaat.

Referensi:

* https://wiki.archlinux.org/index.php/pacman#Cleaning_the_package_cache
* https://utils.kde.org/projects/filelight/
* https://wiki.archlinux.org/index.php/downgrading_packages
