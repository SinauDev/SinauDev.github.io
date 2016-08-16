---
layout: post
title: Memilih versi PHP di Debian
author: aries
category: linux
tags: [php,Debian,apache]
---
Meskipun sekarang versi PHP sudah sampai versi 7 tapi rasanya belum bisa sepenuhnya untuk meninggalkan PHP5. Contohnya belum lama ini, saat saya mencoba memasang beberapa module di drupal ternyata belum bisa berjalan dengan semestinya.

Kebetulan php _default_ yang terpasang di laptop saya ini php7 ( saya juga heran karena yang saya ingat itu memasang php5, mungkin terpasang saat mengubah Debian Jessie ke SID ) maka terpaksa saya harus menurunkan kembali ke versi php5. Dan akhirnya kini di laptop terpasang dua jenis php5 dan php7.

## Mengubah Versi ke PHP5

Karena bawaanya di sini php7 maka saya matikan terlebih dahulu _module_ php7 untuk apache

`sudo a2dismod php7.0`

Setelah itu aktifkan _module_ apache untuk php5

`sudo a2enmod php5`

Terakhir _restart_ apache _service_

`sudo /etc/init.d/apache2 restart`

## Mengembalikan ke PHP7

Untuk mengembalikan ke versi php7 caranya sama dengan yang di atas hanya diubah mana yang dimatikan dan mana yang dihidupkan.

Matikan dahulu _module_ php5

`sudo a2dismod php5`

Aktifkan kembali _module_ untuk php7

`sudo a2enmod php7.0`

_Restart_ kembali apache

`sudo /etc/init.d/apache2 restart`

Maka php7 sudah kembali sebagai php _default_ OS Anda.

## Catatan Tambahan

Jika saat _restart_  apache gagal, pastikan _module_ php untuk apache sudah terpasang. Untuk kasus yang saya alami saat mengubah dari php7 ke php5 saya cukup memasang _module_ yang dibutuhkannya dengan cara :

`sudo apt-get install libapache2-mod-php5`

Referensi :

* http://askubuntu.com/questions/761713/how-can-i-downgrade-from-php-7-to-php-5-6-on-ubuntu-16-04
* http://askubuntu.com/questions/536128/apache2-start-error-when-using-php5-module-ubuntu-server