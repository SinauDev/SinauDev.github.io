---
layout: post
date: 2016-09-22 05:12:42 +0700
title: Install Arch Linux Bootstrap dari Live CD Ubuntu (Disertai Video)
author: ali
category: linux
tags: [gnu-linux,archlinux,chroot,bootstrap]
---

Arch Linux merupakan salah satu distro yang tergolong cukup sulit dalam hal memasangnya. Di samping kita berurusan dengan _full_ CLI, Arch Linux mewajibkan pula koneksi internet. Untuk tipe koneksi internet menggunakan jaringan kabel dengan `DHCP` ini mungkin menjadi solusi mudah, lain hal jika Anda menggunakan koneksi WiFi seperti halnya `wifi.id` yang mengharuskan kita _login_ dari _browser_ dahulu baru bisa kita menikmati akses internet; atau jika terjadi masalah pada WiFi _hardware_ kita yang mana dalam awal proses instalasi tidak terdeteksi sama sekali. Tentu dengan semua itu dalam metode CLI cukup menyulitkan bagi sebagaian orang, apalagi bagi mereka yang ingin migrasi dan baru pertama kali di Arch Linux.

Tulisan kali ini menjawab tentang hal tersebut, kita memasang Arch Linux dengan metode `bootstrap` atau `chroot`. Yang mana metode tersebut membutuhkan GNU/Linux lainnya untuk menjalankannya (_install form existing GNU/Linux_). Maksudnya Anda tidak akan dapat menjalankan metode `bootstrap` tanpa adanya GNU/Linux sebelunya. Minimal ada `Live` nya.

# Daftar Isi

* daftar isi
{:toc}

# Tahap Persiapan

Sebelum memasuki tahap instalasi ada beberapa kriteria yang perlu ada di Anda

1. Memiliki Live CD GNU/Linux.
Saya mempraktekan `bootstrap` Arch Linux dengan _Live_ CD Lubuntu 14.04 64bit. Sebetulnya dengan _Live_ CD apapun bisa yang terpenting ber-DE. Saya memilih Lubuntu karena ringan dan pada prakteknya saya gunakan `Virtual Box`.
2. Sudah Terkoneksi dengan Interet.
Ketika Anda menggunakan Live CD tersebut pastikan ada sudah terkoneksi dengan internet.
3. Mengunduh Arch Linux Bootstrap.
Ini dapat Anda persiapkan lebih dahulu atau nanti setelah proses instalasi saja di Live CD.

# Tahap Pengaturan Partisi

Pertama-tama kita buat dua buah partisi yakni `root` dan `swap`. Asumsinya partisi Anda benar-benar kosong tidak ada isinya sama sekali. Jika sudah ada GNU/Linux sebelumnya dapat menggunakan `swap` lama atau mungkin `/home` yang lama atau dari GNU/Linux yang Anda gunakan saat ini, bisa Anda satukan juga dengan `/home` Archlinux, dengan syarat **jangan** menggunakan `username` yang sama.

Kita gunakan `Gparted` saja agar mudah dalam pembagian partisi.

```
root = 18 GB
swap = 2 GB
```

Jika HDD Anda benar-benar kosong atau masih baru, gunakan skema partisi, `MBR` saja agar mudah. Kalau memang mau menggunakan `GPT` tidak masalah, tapi dalam praktiknya di sini saya menggunakan `MBR`. Anda tetap masih bisa menggunakan cara ini, hanya ada sedikit perbedaan yakni, Anda wajib buat partisi `/boot` secara terpisah buat miniminal `512 MB`.

# Tahap Chroot

Jika sudah selesai semua partisi Anda lakukan, sekarang kita memasuki metode `chroot`. kita unduh `Arch-bootstrap`-nya di:

```bash
$ wget -c http://suro.ubaya.ac.id/archlinux/iso/2016.09.03/archlinux-bootstrap-2016.09.03-x86_64.tar.gz
```

Kita `mount` partisi terlebih dahulu:

```
# sudo su
# mount /dev/sda1 /mnt
```

**Perhatian!** Pastikan Anda memilih partisi yang tepat. Anda dapat menggunakan `lsbk` untuk mengecek partisi mana yang harus di `mount` ke `/mnt`.

Lalu ekstrak `Arch Bootstrap`-nya, yakni:

```
# tar xvzf archlinux-bootstrap-2016.09.03-x86_64.tar.gz -C /mnt
# cd /mnt/root.x86_64
# mv * ..
# cd ..
# rmdir root.x86_64
```

Kemudian lakukan `mount bind`.

```
# cd /mnt
# mount -t proc proc proc/
# mount --rbind /sys sys/
# mount --rbind /dev dev/
# mount --rbind /run run/
```

Salin DNS dan juga buat mirror, ambil yang Indonesia saja.

```
# cp /etc/resolv.conf /mnt/etc/resolv.conf
# nano /mnt/etc/pacman.d/mirrorlist
```

Agar lebih mudah tekan tombol `CTRL+W` dan ketik `Indonesia`, lalu _uncomment mirror_ tersebut.

Kemudian baru kita lakukan `chroot`:

```
# chroot /mnt /bin/bash
# export PS1="(chroot) $PS1"
```

# Tahap Instalasi

Jika Anda sudah sukses masuk ke dalam Arch Linux dengan chroot, langsung selanjutnya kita pasang Arch Linux dengan metode yang dikatakan sebagai `bootstrap`. Sebelum proses pemasangan paket, kita diwajibkan untuk membuat `key` nya terlebih dahulu.

```
# pacman-key --init
Gerakan cursor mouse Anda secara random di sisi kanan atau kiri Terminal

# pacman-key --populate archlinux
```

Lalu baru kemudian kita sinkronisasikan _database_ repositori.

## Pemasangan Base

```
# pacman -Syy
```

Setelah itu kita pasang `base` nya:

```
# pacman -S base base-devel
```

Kemudian kita pasang `Xorg` dan `Grub` beserata beberapa utiliti lainnya:

```
# pacman -S xorg-server ttf-dejavu
# pacman -S grub-bios ntfs-3g bash-completion
```

## Pemasangan Driver VGA

Pasang _driver_ VGA, pastikan Anda mengetahuinya. Untuk mengecek dapat menggunakan cara:

```
# lspci | grep VGA
```

### Pengguna VGA Standard

```
# pacman -S xf86-video-vesa mesa mesa-demos
```

### Pengguna Intel

```
# pacman -S xf86-video-intel
```

### Pengguna Nvidia

```	
# pacman -S xf86-video-nouveau
```

### Pengguna ATI

```
# pacman -S xf86-video-ati
```

## Pemasangan DE

Lalu kita pasang DE nya. Saya merekomendasikan gunakan `Gnome`, jika Anda tidak ingin dipusingkan dengan banyak pengaturan. Akan tetapi paling tidak Anda memiliki minimum `RAM 2 GB` untuk menjalankan `Gnome`.

```
# pacman -S gnome gedit firefox file-roller
```

## Pengaturan Sistem dan User

Langsung kita memasuki tahap pengaturan zona waktu dan hostname.

```
# nano /etc/locale.conf
```

Lalu isikan seperti berikut:

```
LC_COLLATE=C
LANG=en_US.UTF-8
LC_TIME=id_ID.UTF-8
```

Kemudian:

```
# nano /etc/locale.gen
```

_Uncomment_ pada `en_US.UTF-8 UTF-8` dan `id_ID.UTF-8 UTF-8`.
Dan lakukan:

```
# locale-gen
```

Buat hostname dan zona waktu

```
# echo "Archlinux" > /etc/hostname
# ln -s /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
# hwclock --systohc --utc
```

Agar _group_ `wheel` dapat menggunakan `sudo`.

```
# nano /etc/sudoers

cari dan uncomment:
%wheel ALL=(ALL) ALL
```

Buat user dan _set password_ `root`.

```
# useradd -m -g users -G wheel,power,storage -s /bin/bash ali
# passwd ali
# passwd root
```

Buat init ram disk:

```
# mkinitcpio -p linux
```

Kemudian buat `fstab` agar kita dapat `booting`.

```
# genfstab -p / > /etc/fstab
# nano /etc/fstab
Beri tanda comment beberapa gvfs-fuse dan zram
```

Lalukan pemasangan `grub` dan pengaturannya.

```
# grub-install /dev/sda
# grub-mkconfig -o /boot/grub/grub.cfg
```

Kemudian kita aktifkan `NetworkManager` dan `GDM`:

```
# systemctl enable NetworkManager
# systemctl enable gdm
```

Keluar tahap instalasi dan _restart_.

```	
# exit
# reboot
```

# Video Tutorial

Jika Anda masih kesulitan dalam mempraktikan tulisan ini, saya menyediakan video tutorial yang telah saya buat di Virtual Box, caranya sama persis dengan isi dalam tulisan di sini. Lihat video berikut:

<iframe width="700" height="500" src="//www.youtube.com/embed/MLk579mNCjc" frameborder="0" allowfullscreen="allowfullscreen"></iframe>

Semoga bermanfaat 😁
