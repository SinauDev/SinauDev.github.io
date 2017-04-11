---
layout: post
title: Menambah Dukungan MathJax dalam Blog Jekyll 
tags: [Web, Jekyll]
category: pemrograman
comments: true
author: ahmadi
ext-js: "https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-MML-AM_CHTML"
summary: "MathJax merupakan skrip JavaScript yang berfungsi untuk menampilkan tulisan berupa LaTeX, MathML, AsciiMath pada peramban. Sederhananya buat menulis rumus Matematika dalam halaman web."
--- 

> Menulis membantu diri sendiri. Tetap ada walau mati.

Menulis blog dalam lingkungan web statis seperti ini memang `memaksa` banyak belajar. Kadang sepertinya malah buang-buang waktu alias `kotra-produktif`. 
Tapi menulis memang bukan buat membuat orang belajar apalagi pamer kepintaran, menulis malah menjadi sarana belajar tersendiri buat saya yang malas belajar ini. Ya walau niat awal saya menggunakan `Jekyll` untuk `nge-blog` biar terlihat *geek*. Tau *geek* kan? Yang seperti digambar ini lho :

![](/img/ps4-bebegik.jpg) 

😋

`...`

Beberapa hari yang lalu saat menulis [artikel](https://ahmadihamid.com/Perjalanan-Statis-4/) yang memiliki rumus Matematika di dalamnnya saya `terpaksa` belajar menambahkan dukungan `MathJax` pada Jekyll karena ternyata dukungan untuk menulis rumus/persamaan Matematika tidak diberikan secara bawaan. 

MathJax merupakan skrip JavaScript yang berfungsi untuk menampilkan tulisan berupa `LaTeX`, `MathML`, `AsciiMath` pada peramban. Sederhananya: buat menulis rumus Matematika dalam halaman web.

Untuk mendapatkan fitur tersebut kita hanya perlu menambahakan parameter `js` atau `ext-js` pada [front matter](https://jekyllrb.com/docs/frontmatter/) yang mengarah pada berkas JavaScript MathJax, seperti contoh di bawah ini: 

```shell
---
layout: post
title: tes mathjax
tags: [Kimia Analisis]
comments: true
author: ahmadi
summary: ""
ext-js: "https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-MML-AM_CHTML"
--- 
```

Saya memilih menggunakan `ext-js` untuk menjaga kesederhanaan isi repository, toh tidak setiap postingan mengandung LaTeX.

Skrip MathJax akan me-*render* setiap karakter yang berada dalam pembatas `$$ ... $$`. Berikut adalah contoh penulisannya :  

`$$\sum_{i=0}^n i^2 = \frac{(n^2+n)(2n+1)}{6}$$`

Dan dibawah ini adalah hasilnya :

$$\sum_{i=0}^n i^2 = \frac{(n^2+n)(2n+1)}{6}$$


Indah kan?
Selamat mencoba!

😘

**Referensi** :

<https://ahmadihamid.com/Perjalanan-Statis-4/>

<https://t.me/halamanbelakang>

<http://docs.mathjax.org/en/latest/start.html>
