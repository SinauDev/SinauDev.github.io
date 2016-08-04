---
layout:     post
title:      Mengarahkan GitHub Pages ke Domain Pribadi
date:       2016-08-04
category: tutorial
author: ali
tags: [github,jekyll,tips-trik]
---

Mengarahkan GitHub _pages_ Ke _domain_ Pribadi. Pada tanggal 7 Maret 2016 s/d 21 Maret 2016 di [Rumahweb](http://gratis.rumahweb.co.id/) sedang mengadakan promo gratis satu tahun _domain_ `.id`, Nah dalam kesempatan itu saya coba mengambil _domain_ dengan nama [pegelinux.id](http://pegelinux.id) sebagai pengganti dari _domain_ [pegelinux.github.io](http://pegelinux.github.io).

Adapun caranya adalah sebagai berikut:

## Untuk _Domain_ Utama

1. Buat berkas dengan nama `CNAME` wajib dengan huruf kapital semua.
2. Isikan berkas itu dengan domain Anda, misalnya `pegelinux.id`.<br/>
[![alt CNAME](https://ali.my.id/images/post/pegelinux-id-domain-1.png "CNAME")](/images/post/pegelinux-id-domain-1.png){: .thumbnail}
3. Kemudian masuk ke `domain panel` ubah `A Record` dari _domain_ Anda tersebut menjadi:

   | Name | IP             |
   |------|----------------|
   | @    | 192.30.252.153 |
   | @    | 192.30.252.154 |
   {: .table .table-striped} 

    Ini berurusan dengan tempat Anda membeli _domain_. Dalam contoh di sini saya menggunakan pengaturan _domain_ bawaan [Digital Ocean](https://m.do.co/c/734a86fa5365).<br/>
[![alt A Record](https://ali.my.id/images/post/pegelinux-id-domain-2.png "A record")](/images/post/pegelinux-id-domain-2.png){: .thumbnail}
**Perhatian:** Jika Anda tidak memiliki domain panel, bisa menggunakan fasilitas `Cloud Flare` (artikel menyusul 😄)
4. Jika sudah selesai bisa langsung Anda `git add CNAME`, `git commit -m "menambahkan CNAME"` dan `git push -u origin master`.
5. Jika sudah selesai cek di _setting_ repositori Anda. Sudah seperti gambar berikut:<br/>
[![alt A Record](https://ali.my.id/images/post/pegelinux-id-domain-3.png "pegelinux.id")](/images/post/pegelinux-id-domain-3.png){: .thumbnail}

## Untuk _Subdomain_
Adapun untuk _subdomain_ caranya lebih mudah yakni Anda hanya perlu mengarahan `CNAME` di `domain panel` Anda dengan ke domain `github.io` Anda.Misalnya

|Subdomain         | CNAME              |
|blog.pegelinux.id | pegelinux.github.io|
{: .table .table-striped} 

<br/>
Langkah terakhir yakni tes apakah GitHub _pages_ Anda sudah mengarah ke domain Anda tersebut, apa belum. Semoga bermanfaat.😊

## Sumber

<https://ali.my.id/tutorial/github-domain>