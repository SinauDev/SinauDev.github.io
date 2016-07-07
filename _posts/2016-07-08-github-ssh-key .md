---
layout: post
title: Lebih Mudah GitHub dengan SSH Key
---

Bahasan pertama di SinauDev yakni mengenai pemanfaatan SSH Key untuk GitHub. Apa sih maanfaatnya? Mengapa harus menggunakan SSH key? Salah satu manfaat yang terasa ketika menggunakan SSH Key yakni kita tidak perlu lagi memasukan _username_ dan _password_ pada saat _clone_, _push_, ataupun _pull_. Dengan demikian akan mempersingkat kerja kita.

## Batasan

Tutorial ini diperuntukan bagi Anda yang sudah memiliki akun GitHub dan pengguna GNU/Linux.

## Persiapan

1. Pastikan Anda telah memasang `git` dan `openssh` pada GNU/Linux distro yang Anda pergunakan saat ini.
2. Pastikan Anda memiliki akses internet untuk tahap _testing_ seperti proses _cloning_.


## Praktek

* Buat SSH Key. 

```
ssh-keygen -t rsa
```
* Masukan _passphrase_ . Sebetulnya ini opsional, tapi penulis menyarankan menggunakan ini agar lebih aman.

```
ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ali/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/ali/.ssh/id_rsa.
Your public key has been saved in /home/ali/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:WutFMnNKtzxqReRIpJVK4If5RtJRer+uFLF0ZjRz4AU ali@ali-pc
The key's randomart image is:
+---[RSA 2048]----+
|    ...o+.Eoo    |
|   . +.=oo.=     |
|    =.*o++=      |
|     =.o.Bo      |
|      o S.=      |
|     . + %.o     |
|      . +.*      |
|       o.+ .     |
|       .+..      |
+----[SHA256]-----+
```
* Ambil Pubkey SSH Anda dengan cara, mengetik perintah `cat` pada `terminal`. Yakni:

```
cat ~/.ssh/id_rsa.pub
```
* Masuk ke GitHub, klik pada foto profile Anda dan pilih `settings` , lalu pilih `SSH and GPG keys`
* Klik `New SSH Key` dan _paste_ `pubkey` yang telah Anda _copy_ sebelumnya. Contohnya:

```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDlnqGwxZwSg1XvyWaUEM117Dfep2g7+O/n4Dg2Acb4vvqD2F33qMXQbVcdLlxTWJaDbbJMNxDp02dBMoHxlOPVyzqLc9Q43SQPxGAYzxG4WhEiDZbEqV2KlC3zxc5Noy3OyIeMqXEL5pTRTEApblsM6rirdhYSvRDD/MOLl51Sfx+RxRgHlBh12UfvSIoEmnJy10a6hsb8iSoEZuTui4ueRxZf3YMkgkFCTbG26zuTHEvsFPjsiuBjjBfkvLwJfrUFoO3mbExWH+Zcoup4uZtpVGE7EuuvsFcMXZepnIQgJmXFJlyO3giEKsuXOpgg9QpoVIb7GbgXuhMz95FKA/Ib ali@ali-pc
```
* Selesai, coba Anda tes _clone_ salah satu proyek di GitHub.



## Video Tutorial

Untuk mempermudah penulis membuatkan sebuah video singkat, tidak dipungkiri lagi belajar dengan cara melihat orang lain melakukan lebih mudah dibandingkan dengan cara membaca.

<iframe width="640" height="360" src="https://www.youtube.com/embed/xrVl0P7VRf4" frameborder="0" allowfullscreen></iframe>
