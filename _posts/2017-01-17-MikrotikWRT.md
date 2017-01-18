---
layout: post
title: MikrotikWRT, buka-bukaan dengan Mikrotik
tags: [mikrotik, openwrt, lede]
comments: true
author: ahmadi
category: jaringan
---

<img border="0" src="/img/rbWRT.png" style="float:left; margin:10px"/>

Menggganti sistem operasi bawaan Mikrotik (`RouterOS`) dengan `LEDE`/`OpenWRT` mungkin layak pilih buat kamu yang lebih suka buka-bukaan (baca : `OpenSource`) atau buat para *Power User* yang mau bangun sendiri paket perangkat lunak yang digunakan atau justru membangun sendiri sistem operasinya, karena LEDE/OpenWRT lebih fleksibel dibanding RouterOS. Walau RouterOS versi 4 sudah mendukung fitur `MetaROUTER`. Metarouter merupakan fitur yang memungkinkan Mikrotik RouterBoard untuk menjalankan OpenWRT secara virtual berdampingan dengan RouterOS.

Saya sendiri punya alasan lain yaitu terlalu banyak fitur bawaan RouterOS yang tidak terpakai buat pengguna rumahan seperti saya, pun sebenarnya saya juga orang yang suka **buka-bukaan**. 

😊

**Tahap 1 : Periksa Dukungan**

Hal pertama yang kita lakukan yaitu memeriksa dukungan LEDE untuk perangkat di [ToH](https://lede-project.org/toh/start). Perangkat yang saya gunakan (`rb941-2nd`) ternyata belum memiliki dukungan dari LEDE. Untungnya berkah "buka-bukaan" ada yang membuat [patch](http://patchwork.ozlabs.org/patch/711136/raw/)  yang dapat saya gunakan untuk menambahkan dukungan buat perangkat rb941-2nd.

😌

**Tahap 2 : Membangun Firmware**

Jika ternyata perangkat kamu didukung oleh LEDE, kamu bisa langsung mengunduh firmware yang tersedia dan mengabaikan tahap ini. Di tahap ini saya ~~terpaksa~~ meng-*compile* sendiri firmware untuk mikrotik rb941-2nd. Jika kamu memiliki rb941-2nd yang sama dengan saya dan malas *compile* sendiri, kamu bisa unduh [di sini](https://github.com/ahmadihamid/rb941-2nd-LEDE/releases). Buat yang mau belajar *compile* silakan lanjut membaca.

**Memasang Dependensi**

Pasang semua dependensi yang dibutuhkan untuk membangun firmware, kebutuhan depedensi dapat dilihat [di sini](https://wiki.openwrt.org/doc/howto/buildroot.exigence).

**Mengunduh Kode Sumber**

````shell
git clone https://git.lede-project.org/source.git
````

**Menerapkan patch**

Konfigurasi quilt :

```shell
cat > ~/.quiltrc <<EOF
QUILT_DIFF_ARGS="--no-timestamps --no-index -p ab --color=auto"
QUILT_REFRESH_ARGS="--no-timestamps --no-index -p ab"
QUILT_PATCH_OPTS="--unified"
QUILT_DIFF_OPTS="-p"
EDITOR="nano"
EOF
```

Terapkan patch :

```shell
cd source
mkdir patches
cp "/lokasi/LEDE-DEV-ar71xx-add-support-for-RB-941-2nD.patch" patches
quilt push -a
```

**Meng-compile Kode Sumber**

```shell
cd source
./scripts/feeds update -a
./scripts/feeds install -a
make menuconfig #membuat file .config spesifik untuk router.
```

<img border="0" src="/img/rbWRT-menuconfig.png" width="65%" style="float:left; margin:7px"/>

selesai membuat file `.config` sesuai kebutuhan router (perhatikan dokumentasi/ToH untuk hal ini), kita akan melakukan *compile* dengan perintah :

```shell
make -j1 V=s
```

proses *compile* untuk pertama kali akan memakan waktu yang cukup lama bergantung dari spesifikasi komputer yang digunakan, dan membutuhkan koneksi internet untuk mengunduh beberapa kode sumber dari paket yang akan dipasang.

**Tahap 3 : Ramdisk Boot**

Kita enggak bisa langsung melakukan flashing firmware LEDE ke Mikrotik Routerboard melalui RouterOS, flashing akan dilakukan melalui LEDE yang diboot ke RAM melalui `DHCP/TFTP` server. Karena saya enggak punya DHCP/TFTP server maka saya menggunakan script berikut di laptop dengan OS `ArchLinux` :

```shell
#!/bin/bash
USER=annajm #sesuaikan username kamu, pun dengan nama interface 
ifconfig enp1s0 192.168.1.10 up
dnsmasq -i enp1s0 --dhcp-range=192.168.1.100,192.168.1.200 \
--dhcp-boot=lede-ar71xx-mikrotik-vmlinux-initramfs.elf \
--enable-tftp --tftp-root=/home/$USER/ -d -u $USER -p0 -K --log-dhcp --bootp-dynamic
```

Ubah variabel dns di file `/etc/NetworkManager/NetworkManager.conf` menjadi :

```shell
dns=dnsmasq
```

Jalankan script loader/tftp tadi dengan hak akses `root/sudo`

```shell
~ ❯ sudo ./loader.sh
```

Restart NetworkManager

```shell
~ ❯ sudo systemctl restart NetworkManager
```
...

Sekian panjang kita bercerita tapi belum menjamah samasekali ke Perangkat Mikrotik Routerboard. Yuk kita jamahi sekrang!

😙

 Lakukan konfigurasi berikut pada RouterBoard agar RouterBoard Mikrotik mencoba melakukan booting dari DHCP/TFTP server saat pertama dihidupkan (hanya sekali),
 
- System → Routerboard → Settings → Boot device: Try ethernet once then NAND
- System → Routerboard → Settings → Boot protocol: DHCP
- System → Routerboard → Settings → Ceklis pilihan "Force Backup Booter" 

Cabut power Mikrotik, hubungkan kabel lan ke *port1* (`wan`/`internet`), dan hidupkan lagi RouterBoard, berikut adalah pesan yang keluar dari script tftp server yang kita jalankan :

```shell
~ ❯ sudo ./loader.sh                                                          ⏎
dnsmasq: started, version 2.76 DNS disabled
dnsmasq: compile time options: IPv6 GNU-getopt DBus i18n IDN DHCP DHCPv6 no-Lua TFTP conntrack ipset auth DNSSEC loop-detect inotify
dnsmasq-dhcp: DHCP, IP range 192.168.1.100 -- 192.168.1.200, lease time 1h
dnsmasq-tftp: TFTP root is /home/annajm/ 
dnsmasq-dhcp: 3917112521 available DHCP range: 192.168.1.100 -- 192.168.1.200
dnsmasq-dhcp: 3917112521 vendor class: Mips_boot
dnsmasq-dhcp: 3917112521 DHCPDISCOVER(enp1s0) e4:8d:8c:69:a8:a4 
dnsmasq-dhcp: 3917112521 tags: enp1s0
dnsmasq-dhcp: 3917112521 DHCPOFFER(enp1s0) 192.168.1.131 e4:8d:8c:69:a8:a4 
dnsmasq-dhcp: 3917112521 requested options: 1:netmask, 3:router
dnsmasq-dhcp: 3917112521 bootfile name: lede-ar71xx-mikrotik-vmlinux-initramfs.elf
dnsmasq-dhcp: 3917112521 next server: 192.168.1.10
dnsmasq-dhcp: 3917112521 sent size:  1 option: 53 message-type  2
dnsmasq-dhcp: 3917112521 sent size:  4 option: 54 server-identifier  192.168.1.10
dnsmasq-dhcp: 3917112521 sent size:  4 option: 51 lease-time  1h
dnsmasq-dhcp: 3917112521 sent size:  4 option: 58 T1  30m
dnsmasq-dhcp: 3917112521 sent size:  4 option: 59 T2  52m30s
dnsmasq-dhcp: 3917112521 sent size:  4 option:  1 netmask  255.255.255.0
dnsmasq-dhcp: 3917112521 sent size:  4 option: 28 broadcast  192.168.1.255
dnsmasq-dhcp: 3917112521 sent size:  4 option:  3 router  192.168.1.10
dnsmasq-dhcp: 1828775469 available DHCP range: 192.168.1.100 -- 192.168.1.200
dnsmasq-dhcp: 1828775469 vendor class: Mips_boot
dnsmasq-dhcp: 1828775469 DHCPREQUEST(enp1s0) 192.168.1.131 e4:8d:8c:69:a8:a4 
dnsmasq-dhcp: 1828775469 tags: enp1s0
dnsmasq-dhcp: 1828775469 DHCPACK(enp1s0) 192.168.1.131 e4:8d:8c:69:a8:a4 
dnsmasq-dhcp: 1828775469 requested options: 1:netmask, 3:router
dnsmasq-dhcp: 1828775469 bootfile name: lede-ar71xx-mikrotik-vmlinux-initramfs.elf
dnsmasq-dhcp: 1828775469 next server: 192.168.1.10
dnsmasq-dhcp: 1828775469 sent size:  1 option: 53 message-type  5
dnsmasq-dhcp: 1828775469 sent size:  4 option: 54 server-identifier  192.168.1.10
dnsmasq-dhcp: 1828775469 sent size:  4 option: 51 lease-time  1h
dnsmasq-dhcp: 1828775469 sent size:  4 option: 58 T1  30m
dnsmasq-dhcp: 1828775469 sent size:  4 option: 59 T2  52m30s
dnsmasq-dhcp: 1828775469 sent size:  4 option:  1 netmask  255.255.255.0
dnsmasq-dhcp: 1828775469 sent size:  4 option: 28 broadcast  192.168.1.255
dnsmasq-dhcp: 1828775469 sent size:  4 option:  3 router  192.168.1.10
dnsmasq-tftp: sent /home/annajm/lede-ar71xx-mikrotik-vmlinux-initramfs.elf to 192.168.1.131
dnsmasq-dhcp: 1828775469 available DHCP range: 192.168.1.100 -- 192.168.1.200
dnsmasq-dhcp: 1828775469 vendor class: Mips_boot
dnsmasq-dhcp: 1321268380 available DHCP range: 192.168.1.100 -- 192.168.1.200
dnsmasq-dhcp: 1321268380 vendor class: udhcp 1.25.1
dnsmasq-dhcp: 1321268380 DHCPDISCOVER(enp1s0) e4:8d:8c:69:a8:a4 
dnsmasq-dhcp: 1321268380 tags: enp1s0
dnsmasq-dhcp: 1321268380 DHCPOFFER(enp1s0) 192.168.1.131 e4:8d:8c:69:a8:a4 
dnsmasq-dhcp: 1321268380 requested options: 1:netmask, 3:router, 6:dns-server, 12:hostname, 
dnsmasq-dhcp: 1321268380 requested options: 15:domain-name, 28:broadcast, 42:ntp-server, 
dnsmasq-dhcp: 1321268380 requested options: 121:classless-static-route
dnsmasq-dhcp: 1321268380 bootfile name: lede-ar71xx-mikrotik-vmlinux-initramfs.elf
dnsmasq-dhcp: 1321268380 next server: 192.168.1.10
dnsmasq-dhcp: 1321268380 sent size:  1 option: 53 message-type  2
dnsmasq-dhcp: 1321268380 sent size:  4 option: 54 server-identifier  192.168.1.10
dnsmasq-dhcp: 1321268380 sent size:  4 option: 51 lease-time  1h
dnsmasq-dhcp: 1321268380 sent size:  4 option: 58 T1  30m
dnsmasq-dhcp: 1321268380 sent size:  4 option: 59 T2  52m30s
dnsmasq-dhcp: 1321268380 sent size:  4 option:  1 netmask  255.255.255.0
dnsmasq-dhcp: 1321268380 sent size:  4 option: 28 broadcast  192.168.1.255
dnsmasq-dhcp: 1321268380 sent size:  4 option:  3 router  192.168.1.10
dnsmasq-dhcp: 1321268380 available DHCP range: 192.168.1.100 -- 192.168.1.200
dnsmasq-dhcp: 1321268380 vendor class: udhcp 1.25.1
dnsmasq-dhcp: 1321268380 DHCPREQUEST(enp1s0) 192.168.1.131 e4:8d:8c:69:a8:a4 
dnsmasq-dhcp: 1321268380 tags: enp1s0
dnsmasq-dhcp: 1321268380 DHCPACK(enp1s0) 192.168.1.131 e4:8d:8c:69:a8:a4 
dnsmasq-dhcp: 1321268380 requested options: 1:netmask, 3:router, 6:dns-server, 12:hostname, 
dnsmasq-dhcp: 1321268380 requested options: 15:domain-name, 28:broadcast, 42:ntp-server, 
dnsmasq-dhcp: 1321268380 requested options: 121:classless-static-route
dnsmasq-dhcp: 1321268380 bootfile name: lede-ar71xx-mikrotik-vmlinux-initramfs.elf
dnsmasq-dhcp: 1321268380 next server: 192.168.1.10
dnsmasq-dhcp: 1321268380 sent size:  1 option: 53 message-type  5
dnsmasq-dhcp: 1321268380 sent size:  4 option: 54 server-identifier  192.168.1.10
dnsmasq-dhcp: 1321268380 sent size:  4 option: 51 lease-time  1h
dnsmasq-dhcp: 1321268380 sent size:  4 option: 58 T1  30m
dnsmasq-dhcp: 1321268380 sent size:  4 option: 59 T2  52m30s
dnsmasq-dhcp: 1321268380 sent size:  4 option:  1 netmask  255.255.255.0
dnsmasq-dhcp: 1321268380 sent size:  4 option: 28 broadcast  192.168.1.255
dnsmasq-dhcp: 1321268380 sent size:  4 option:  3 router  192.168.1.10
```

dari output di atas kita bisa lihat RouterBoard mendapatkan IP 192.168.1.131 dan file ramdisk berhasil dikirimkan.

```shell
...
dnsmasq-dhcp: 3917112521 DHCPOFFER(enp1s0) 192.168.1.131 e4:8d:8c:69:a8:a4 
...
dnsmasq-tftp: sent /home/annajm/lede-ar71xx-mikrotik-vmlinux-initramfs.elf to 192.168.1.131
...
```

Pada tahapan ini LEDE telah di-boot pada RAM RouterBoard. Kita bisa mengakses LEDE melalui LuCi atau ssh untuk melakukan *flashing* menggunakan file squashfs dan menanamkan firmware secara permanen ke RouterBoard.

Cabut kabel lan dari *port1* dan hubungkan ke *port* `lan` 2/3/4/dst, karena administrasi router tidak dapat dilakukan melalui port `wan`.

Akses LuCi melalui IP 192.168.1.1

<img border="0" src="/img/rbWRT-luci.png" style="float:left; margin:10px"/>

**Flash Firmware.**

**Selesai.**

😪

Selamat buka-bukaan.
 
😀

***Referensi :***

[https://lede-project.org/docs/guide-developer/the-source-code](https://lede-project.org/docs/guide-developer/the-source-code) 

[https://wiki.openwrt.org/doc/howto/buildroot.exigence](https://wiki.openwrt.org/doc/howto/buildroot.exigence) 

[https://wiki.openwrt.org/toh/mikrotik/common](https://wiki.openwrt.org/toh/mikrotik/common) 

[https://wiki.openwrt.org/toh/mikrotik/rb941_2nd](https://wiki.openwrt.org/toh/mikrotik/rb941_2nd) 
