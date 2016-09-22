---
layout: post
date: 2016-09-22 19:12:42 +0700
title: Mencoba Emacs
author: aries
category: linux
tags: [editor,emacs]
---

Gara-gara sebuah dialog dalam serial 'Silicon Valley' saya mulai tertarik untuk mencoba emacs. Emacs sederhananya adalah sebuah text editor yang umumnya berada di _Unix-based systems_. Sebagaimana _Unix text editor_ lainnya, emacs memiliki kombinasi atau _shortcut_ khusus untuk beberapa perintah dasar. Tanpa perlu basa-basi lagi langsung saja ke topik utama tentang emacs.

# Instalasi

Karena saya menggunakan Debian maka untuk pemasangan emacs cukup menggunakan perintah berikut

```
sudo apt-get install emacs
```

emacs mempunyai dua versi saat kita memasangnya, yaitu versi GUI dan Terminal. Untuk membuka masing-masing versi bisa menggunakan perintah berikut :

_Emacs GUI_

```
emacs
```
![gui](http://blog.arsmp.com/assets/media/emac-gui.png)
_Emacs Terminal_

```
emacs -nw
```
![terminal](http://blog.arsmp.com/assets/media/emacs-terminal.png)

Saat pertama kali mengaktifkan emacs akan ada pilihan **TUTORIAL** dan saran saya ikuti tutorial tersebut. Dengan mengikuti tutorial yang disediakan oleh emacs sediktnya akan tahu _shortcut-shortcut_ dasar yang digunakan. _Shortcut_ di emacs sendiri biasanya dimulai dengan `CTRL` yang biasa disingkat `C` dan `META` yang disingkat `M`, untuk `META` sendiri biasanya ini tombol`alt` di keyboard. Contoh :

```
C + <key>
```
_Shortcut_ di atas maksudnya `CTRL + <key>`

```
M + <key>
```

_Shortcut_ di atas maksudnya `ALT + <key>`.
Selanjutnya tinggal mencari sendiri fungsi _shortcut_ lainnya di tutorial yang disediakan saat mengaktifkan emacs.

# Paket Tambahan

Mulai dari `emacs 24` telah disediakan paket kontrol yang dapat membantu untuk menambahkan beberapa fitur tambahan di emacs, paket kontrol tersebut disebut **ELPA** (_Emacs Lisp Package Archive_). Dan ada juga **MELPA** _repository_ khusus yang menampung paket-paket untuk emacs. Untuk menggunakannya tambahkan script berikut di berkas konfigurasi emacs. Pada kasus saya di Debian, berkas konfigurasi berada di `/home/username/.emacs`.

{% highlight lisp %}
(when (>= emacs-major-version 24)
  (require 'package)
      (add-to-list
	     'package-archives
			       '("melpa" . "http://melpa.org/packages/")
     t)
	 (package-initialize))
{% endhighlight %}
	 
Setelah menambahkan _script_ di atas pada berkas konfigurasi silahkan _restart_ emacs lalu tekan `M+x` dan ketikan :
 
```
 package-list-packages
```

Sehingga akan muncul daftar paket yang dapat dipasang. 
![paket](http://blog.arsmp.com/assets/media/paket-emacs.png)
 
Untuk memasang paket kembali tekan `M+x` dan ketikan :

```
package-install <ENTER> nama-paket
```

Ada paket yang bisa langsung terpasang ada pula yang membutuhkan _restart_ terlebih dahulu.
 
# Tampilan dan Tema
 
Emacs sendiri mempunyai tema bawaan yang dapat digunakan agar tidak terlalu standar tampilannya. Untuk memilih tema bisa gunakan perintah berikut. Tekan kembali `M+x` dan ketikan :

```
customize-theme <ENTER>
```
Setelah menekan ENTER kita akan diarahakan ke dalam pilihan tema yang tersedia.
  
![paket](http://blog.arsmp.com/assets/media/emacs-theme.png)
   
Untuk memasang tema baru caranya sama dengan memasang paket.
   
# Paket yang Saya Gunakan
   
Untuk tema saya menggunakan `suscolor` yang tampilannya menjadi sedikit mirip `sublime`. Sedangkan paket yang lain adalah :

* Neotree untuk menampilkan file manager.
* php-mode dan php-mode+ untuk tampilan berkas .php.
* markdown-mode untuk mode tampilan berkas .md.
* ac-php untuk _auto complete & suggestion_ fungsi PHP.
   
hasilnya seperti berikut 
   
![hasil](http://blog.arsmp.com/assets/media/emacs.gif)
   
# Kesimpulan
   
Emacs ini salah satu editor yang cukup mudah untuk dipelajari karena sifatnya yang bisa langsung ketik tidak perlu _insertion mode_ seperti. Namun, tetap saja perlu merubah sedikit kebiasaan dari editor sebelumnya. Tapi saya rasa itu hanya masalah waktu dan kebiasaan saja, sama seperti pertama kali migrasi dari Windows ke Linux agak kaku tapi setelahnya malah keenakan. Jadi, apa mau mencoba emacs ?

Referensi :

* http://searchenterpriselinux.techtarget.com/definition/Emacs