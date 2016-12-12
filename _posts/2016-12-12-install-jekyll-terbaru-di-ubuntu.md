---
layout: post
title: Install Jekyll Terbaru di Ubuntu 16.04
tags: [jekyll, staticgen]
comments: true
author: ali
category: pemrograman
summary: Jekyll dibangun dengan bahasa pemrograman `Ruby`, seperti yang kita ketahui `Ruby` memiliki _packet manager_ tersediri yang diberinama `gem`. Bagi pengguna Ubuntu, kita dapat memasang Jekyll melalui _official repository_ yakni dengan `apt-get`. Sayangnya, di Ubuntu, memasang Jekyll menggunakan `apt-get` kita tidak akan mendapatkan versi terbarunya, berbeda dengan Jekyll yang dipasang dari `gem` ia akan mendapatkan versi terbaru sesuai dengan apa yang dirilis oleh situs resmi Jekyll.<br/><br/>Tulisan ini hadir memberikan tentang cara bagaimana kita memasang Jekyll di Ubuntu dengan _packet manager_ `Ruby` yakni `gem`, bukan melalui _offcial repository_ Ubuntu melalui `apt-get`.<br/><br/>
---

Jekyll adalah _static site generator_. Dari kata `static site` dan `generator` sudah terbayang maknanya dibenak pikiran kita yakni; suatu alat yang mana berfungsi untuk menciptakan suatu laman situs yang bersifat statis. **Statis** di sini bukan berarti kontennya tidak dapat diubah, melainkan seluruh isi dari situs baik itu tema, konfigurasi maupun isi konten, berupa berkas (_everything is file_).

Jekyll dibangun dengan bahasa pemrograman `Ruby`, seperti yang kita ketahui `Ruby` memiliki _packet manager_ tersediri yang diberinama `gem`. Bagi pengguna Ubuntu, kita dapat memasang Jekyll melalui _official repository_ yakni dengan `apt-get`. Sayangnya, di Ubuntu, memasang Jekyll menggunakan `apt-get` kita tidak akan mendapatkan versi terbarunya, berbeda dengan Jekyll yang dipasang dari `gem` ia akan mendapatkan versi terbaru sesuai dengan apa yang dirilis oleh situs resmi Jekyll.

Agar tulisan ini lebih rapi dan mudah dibaca, penulis berikan tautan dengan daftar isinya:

# Daftar Isi

* daftar isi
{:toc}

# Batasan Masalah

Sesuai judul tulisan ini, penulis membatasi pada pengguna Ubuntu versi 16.04 LTS. Penulis juga mempraktekannya pada Ubuntu 16.04. Bagi Anda pengguna Ubuntu versi sebelumnya atau sesudahnya, mungkin masih bisa dipraktekan. Akan tetapi penulis tidak menjamin keberhasilannya, karena yang penulis praktekan hanya pada Ubuntu 16.04 LTS.

# Memasang Dependensi

Sebelum memulai pemasangan Jekyll, pertama-tama kita sinkronisasi _database_ `apt-get` kita di _localhost_ dengan _server_.

```bash
$ sudo apt-get update
```

Kemudian kita pasang beberapa paket dependensinya:

```bash
$ sudo apt-get install build-essential bison openssl libreadline6 libreadline6-dev curl git-core zlib1g zlib1g-dev libssl-dev libyaml-dev libxml2-dev autoconf libc6-dev ncurses-dev automake libtool
```

# Memasang Ruby

Setelah selasai kita pasang paket dependensinya, lanjut sekarang kita pasang `Ruby`. Di sini kita pasang `Ruby` versi _developer_ (`ruby-dev`).

```bash
$ sudo apt install ruby ruby-dev
```

# Mamasang Jekyll

_Nah_, setelah selesai semua kita memasang paket dependensi dan `Ruby`, baru kemudian kita bisa memanfaatkan `gem` untuk memasang Jekyll.

```bash
$ sudo gem install jekyll bundler
```

# Membuat Blog

Semua proses pemasangan paket sudah kita lalui, langkah selanjutnya kita tes dengan membuat blog. Untuk membuat blog, Anda cukup lakukan perintah berikut:

```bash
$ jekyll new nama_blog_Anda
```

Contohnya:

```bash
$ jekyll new blog
```

Lihat gambar di bawah ini:
![](/img/2016-12-12-jekyll-ubuntu.png)

# Menjalankan Jekyll

Untuk menjalankan Jekyll Anda tidak perlu memasang web server cukup gunakan _web server_ bawaan dari Jekyll yakni dengan cara berikut:

```bash
$ cd blog
$ jekyll serve
```

![](/img/2016-12-12-jekyll-ubuntu2.png)

Jika terjadi galat (_error_), gunakan perintah berikut:

```bash
$ cd blog
$ bundle exec jekyll serve
```

Jika berhasil Anda bisa langsung membukanya melalaui peramban (_browser_), dengan memasukan tautan:

```
http://127.0.0.1:4000/
```

![](/img/2016-12-12-jekyll-ubuntu3.png)

Secara asali (_default_) Jekyll berjalan pada `port 4000`, jika Anda ingin menggunakan `port 80` dan menggunakan _domain_ Anda, gunakan perintah di bawah ini:

```bash
$ sudo su
# jekyll serve -H sinaudev.com -P 80
```

atau

```bash
$ sudo su
# bundle exec jekyll serve -H sinaudev.com -P 80
```

`Port 80` hanya dapat digunakan pada `root` oleh karenanya kita masuk ke dalam sesi `root` terlebih dahulu.

Semoga bermanfaat 😅
