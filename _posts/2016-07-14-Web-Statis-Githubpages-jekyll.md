---
layout: post
title: Membuat Web Statis dengan GitHub Pages & Jekyll
author: aan
category: pemrograman
tags: [Blogging, jekyll, web statis]
---

Mungkin sebagian dari kita sudah sering mendengar istilah static site atau static web page yang sedang menjadi trend sekarang ini dalam dunia web. Banyaknya keuntungan yang bisa didapatkan dan juga kemudahan yang diberikan membuat web statis menjadi pilihan utama untuk menggantikan web dinamis dibandingkan dengan menggunakan CMS seperti WordPress, Blogspot dan lain-lain.

Apalagi sekarang sudah adanya dukungan hosting gratis untuk web statis oleh GitHub. Yaitu, GitHub Pages yang memberikan layanan hosting gratis untuk semua pengguna GitHub hanya dengan membuat repositori pada akunnya. GitHub hanya mendukung client-side-scripting hosting saja. Jadi, tidak diperuntukkan untuk server-side-scripting seperti PHP, Ruby dan lain-lain. Pada awalnya semua pengguna akan mendapatkan subdomain berupa username.github.io / organization.github.io . Sebagai contoh, saya sudah mendaftarkan subdomain saya sendiri yang bisa dicek di [https://aancw.github.io/](https://aancw.github.io/).

GitHub Pages  semakin lengkap dengan adanya generator untuk membuat web statis. Salah satunya adalah Jekyll, static site generator yang banyak digunakan oleh kalangan pengguna GitHub. Salah satu contoh yang menggunakan Jekyll dan GitHub Pages  yaitu situs teman saya yang beralamat di [http://rizaumami.github.io](http://rizaumami.github.io).

Saya sendiri belum menggunakan web statis dan masih menggunakan WordPress sebagai media utama dalam menulis blog. Tapi tak apa nanti migrasi ke web statis( Nanti kalau paket hosting nya sudah habis 😛 ).  Sebelum itu, kita diwajibkan untuk mempunyai akun GitHub terlebih dahulu karena akun itu lah yang akan kita gunakan sebagai subdomain dari web statis kita nanti. Silahkan ikuti panduan untuk membuat GitHub Pages  yang ada pada alamat [https://pages.github.com/](https://pages.github.com/)

Oke kita anggap bahwa sudah berhasil untuk membuat GitHub Pages  dengan subdomain yang ditentukan. Misal disini saya akan menggunakan subdomain [https://aancw.github.io/](https://aancw.github.io/) karena nama pengguna akun GitHub saya adalah [https://aancw.github.io/](https://aancw.github.io/) . Subdomain atau repositori tersebut yang akan nantinya kita gunakan untuk membuat web statis. Tapi, GitHub Pages  juga bisa untuk domain bebas kok hihi. Contohnya [https://pegelinux.id/](https://pegelinux.id/) yang menggunakan GitHub Pages  beserta domain bebas dan Jekyll.

Kita akan memasuki tahap instalasi untuk Jekyll bila proses pembuatan GitHub Pages  sudah selesai. Namun, sebelum itu kita harus memenuhi syarat terlebih dahulu seperti berikut:

- Ruby( Ruby v1.9.3 atau lebih untuk Jekyll 2 dan Ruby v2 atau lebih untuk Jekyll 3)
- RubyGems
- NodeJS
- Python 2.7

## Pemasangan Jekyll

Untuk melakukan instalasi, kita akan menggunakan RubyGems karena kemudahan yang diberikan. Kita cukup melakukan perintah sebagai berikut:

`gem install jekyll`

Semua dependensi Jekyll akan terpasang secara otomatis dengan menggunakan perintah tersebut. Jadi ga perlu khawatir hihi

```
Fetching: safe_yaml-1.0.4.gem (100%)
Successfully installed safe_yaml-1.0.4
Fetching: rouge-1.11.0.gem (100%)
Successfully installed rouge-1.11.0
Fetching: mercenary-0.3.6.gem (100%)
Successfully installed mercenary-0.3.6
Fetching: liquid-3.0.6.gem (100%)
Successfully installed liquid-3.0.6
Fetching: kramdown-1.11.1.gem (100%)
Successfully installed kramdown-1.11.1
Fetching: ffi-1.9.10.gem (100%)
Building native extensions.  This could take a while...
Successfully installed ffi-1.9.10
Fetching: rb-inotify-0.9.7.gem (100%)
Successfully installed rb-inotify-0.9.7
Fetching: rb-fsevent-0.9.7.gem (100%)
Successfully installed rb-fsevent-0.9.7
Fetching: listen-3.0.8.gem (100%)
Successfully installed listen-3.0.8
Fetching: jekyll-watch-1.4.0.gem (100%)
Successfully installed jekyll-watch-1.4.0
Fetching: sass-3.4.22.gem (100%)
Successfully installed sass-3.4.22
Fetching: jekyll-sass-converter-1.4.0.gem (100%)
Successfully installed jekyll-sass-converter-1.4.0
Fetching: colorator-0.1.gem (100%)
Successfully installed colorator-0.1
Fetching: jekyll-3.1.6.gem (100%)
Successfully installed jekyll-3.1.6
Parsing documentation for safe_yaml-1.0.4
Installing ri documentation for safe_yaml-1.0.4
Parsing documentation for rouge-1.11.0
Installing ri documentation for rouge-1.11.0
Parsing documentation for mercenary-0.3.6
Installing ri documentation for mercenary-0.3.6
Parsing documentation for liquid-3.0.6
Installing ri documentation for liquid-3.0.6
Parsing documentation for kramdown-1.11.1
Installing ri documentation for kramdown-1.11.1
Parsing documentation for ffi-1.9.10
Installing ri documentation for ffi-1.9.10
Parsing documentation for rb-inotify-0.9.7
Installing ri documentation for rb-inotify-0.9.7
Parsing documentation for rb-fsevent-0.9.7
Installing ri documentation for rb-fsevent-0.9.7
Parsing documentation for listen-3.0.8
Installing ri documentation for listen-3.0.8
Parsing documentation for jekyll-watch-1.4.0
Installing ri documentation for jekyll-watch-1.4.0
Parsing documentation for sass-3.4.22
Installing ri documentation for sass-3.4.22
Parsing documentation for jekyll-sass-converter-1.4.0
Installing ri documentation for jekyll-sass-converter-1.4.0
Parsing documentation for colorator-0.1
Installing ri documentation for colorator-0.1
Parsing documentation for jekyll-3.1.6
Installing ri documentation for jekyll-3.1.6
Done installing documentation for safe_yaml, rouge, mercenary, liquid, kramdown, ffi, rb-inotify, rb-fsevent, listen, jekyll-watch, sass, jekyll-sass-converter, colorator, jekyll after 20 seconds
14 gems installed
```

## Konfigurasi Jekyll

Setelah Jekyll terpasang, so what’s next? Langkah selanjutnya adalah membuat project, asumsikan namanya adalah startproject.

`jekyll new startproject`

Maka Jekyll akan membuat beberapa berkas yang dibutuhkan

`about.md  _config.yml  css  feed.xml  _includes  index.html  _layouts  _posts  _sass  _site`

Oke kita akan coba terlebih dahulu hasil yang dibuat oleh Jekyll dengan menggunakan perintah berikut untuk menjalankan web server

`jekyl serve`

```
Configuration file: /home/Downloads/startproject/_config.yml
            Source: /home/Downloads/startproject
       Destination: /home/Downloads/startproject/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
                    done in 0.224 seconds.
 Auto-regeneration: enabled for '/home/Downloads/startproject'
Configuration file: /home/Downloads/startproject/_config.yml
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
```

Jekyll sudah menjalankan service dan melakukan binding terhadap port 4000. Bila kita coba akses di browser maka akan menampilkan seperti berikut:

![Alt @sinaudev](../../img/tampilan-awal-jekyll.png "Tampilan Awal Jekyll")

Adapun beberapa berkas penting yang wajib diperhatikan saat melakukan konfigurasi sebagai berikut:

- Berkas `_config.yml` berguna sebagai tempat konfigurasi utama dari Jekyll seperti nama blog, judul blog dan lain sebagainya. Bila kita lihat akan seperti berikut:

```
# Welcome to Jekyll!
#
# This config file is meant for settings that affect your whole blog, values
# which you are expected to set up once and rarely need to edit after that.
# For technical reasons, this file is *NOT* reloaded automatically when you use
# 'jekyll serve'. If you change this file, please restart the server process.

# Site settings
title: Your awesome title
email: your-email@domain.com
description: > # this means to ignore newlines until "baseurl:"
  Write an awesome description for your new site here. You can edit this
  line in _config.yml. It will appear in your document head meta (for
  Google search results) and in your feed.xml site description.
baseurl: "" # the subpath of your site, e.g. /blog
url: "http://yourdomain.com" # the base hostname & protocol for your site
twitter_username: jekyllrb
github_username:  jekyll

# Build settings
markdown: kramdown
```

- Berkas yang berada pada `_includes` akan selalu dipanggil oleh halaman utama dan lain-lain. Karena berisi footer, header, dan lain-lain. Silahkan dicoba untuk ganti sesuai dengan kebutuhan masing-masing
- Berkas yang berada pada `_layouts` berisi pengaturan tata letak blog
- erkas yang berada pada `_posts` berisikan berkas ber-format markdown yang nantinya akan dikonversi kedalam html. Misal kita akan membuat postingan baru. Maka, terlebih dahulu membuat berkas dengan format **YEAR-MONTH-DAY-title.markdown** ( contoh : 2016-06-13-membuat-web-statis-dengan-jekyll.markdown ) dengan isi sebagai berikut:

```
---
layout: post
title:  "Membuat Web Statis dengan Jekyll"
date:   2016-06-13 14:58:35
categories:
---

Hallo... Ini adalah contoh web statis menggunakan GitHub dan Jekyll.
```

Jalankan kembali jekyll serve dan juga jekyll build (opsional bila konten tidak berubah) bila service sebelumnya sudah dimatikan. Halaman jekyll pun akan berubah seperti yang kita lakukan perubahan sebelumnya.

## Tema untuk Jekyll

Untuk tema bisa merujuk ke alamat http://jekyllthemes.org/ agar web statis nya bisa lebih cantik dan bagus dilihat hihi

## Unggah ke GitHub Pages

Setelah kita selesai melakukan perubahan terhadap bawaan dari jekyll. Tahap terakhir adalah melakukan unggah data ke GitHub Pages  yang sudah kita buat sebelumnya. Tapi sebelumnya kita harus melakukan inisialisasi git repositori.

`git init`

Kita lakukan perubahan terlebih dahulu terhadap berkas `_config.yml` untuk menyesuaikan url menjadi sesuai yang ditentukan. Misal menjadi seperti berikut:

```
# Site settings
title: Nothing to do here
email: your-email@domain.com
description: >
 Hanya contoh untuk tutorial membuat Web Statis dengan Jekyll.
baseurl: ""
url: "http://aancw.github.io" # the base hostname & protocol for your site
```

Push jekyll kedalam repositori yang sudah kita buat

```
git remote add origin https://github.com/username/username.github.io
git checkout -b master
git add .
git commit -m "Belajar Jekyll"
git push -u origin master
```

Akhirnya web statis kita sudah sukses mengudara dengan subdomain github.io . Akhir kata, semoga artikel ini bermanfaat.

Referensi:

- [https://www.smashingmagazine.com/2015/11/modern-static-website-generators-next-big-thing/](https://www.smashingmagazine.com/2015/11/modern-static-website-generators-next-big-thing/)
- [https://help.github.com/articles/using-jekyll-as-a-static-site-generator-with-github-pages/](https://help.github.com/articles/using-jekyll-as-a-static-site-generator-with-github-pages/)
- [https://pages.github.com/](https://pages.github.com/)
- [http://www.kunocreative.com/blog/bid/88040/Static-Dynamic-and-Interactive-Content-Pros-and-Cons](http://www.kunocreative.com/blog/bid/88040/Static-Dynamic-and-Interactive-Content-Pros-and-Cons)
- [https://www.taniarascia.com/make-a-static-website-with-jekyll/](https://www.taniarascia.com/make-a-static-website-with-jekyll/)
- [http://jekyllrb.com/](http://jekyllrb.com/)
