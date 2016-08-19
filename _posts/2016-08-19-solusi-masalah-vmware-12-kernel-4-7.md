---
layout: post
title: Solusi Masalah VMWare Workstation Pro 12.1 di Kernel 4.7
author: aan
category: linux
tags: [ArchLinux, VMWare]
---
Beberapa hari yang lalu saya melakukan pembaruan terhadap sistem operasi saya. Saya menggunakan `Arch Linux` sebagai sistem operasi yang digunakan sehari-hari untuk kebutuhan _development_ dan kebutuhan lainnya. Kebetulan pada pembaruan tersebut melakukan pembaruan terhadap `Linux Kernel` menjadi versi 4.7. Yang dimana memang sudah pada tahap stabil untuk dilakukan perilisan terhadap `Linux Kernel` tersebut pada `Arch Linux`.

Hal yang sangat wajar bila terjadi adanya masalah ketika melakukan pembaruan terhadap _kernel_ yang mungkin dikarenakan perbedaan terhadap perangkat yang kita gunakan baik itu perangkat lunak ataupun perangkat keras yang belum kompatibel. Nah, pada kali ini saya mengalami hal tersebut( sebenarnya sudah sering *duh*). Selama saya menggunakan `Arch Linux` memang sering mengalami masalah dengan `VMWare` ataupun `VirtualBox` yang berkaitan dengan `linux-headers` ataupun yang lainnya.

Setelah saya melakukan pembaruan terhadap _kernel_ dan kemudian _restart_ laptop untuk melakukan pengaplikasian terhadap sistem. Kemudian saya menjalankan aplikasi `VMWare Workstation` untuk menghidupkan Windows 10 agar bisa melakukan _development_ dan uji coba terhadap aplikasi yang saya buat pada sistem operasi Windows. Eh, malah muncul gambar yang kurang lebih seperti berikut( gak sempet screenshot :p ) :

![VMWare Kernel Module Updater](http://i.imgur.com/mpFDo.png)

Setelah saya klik Install untuk melakukan kompilasi terhadap _modules_ yang belum terpasang dan mungkin belum terkompilasi. Saat proses kompilasi berlangsung, muncul sebuah _popup_ menyatakan bahwa ada masalah yang terjadi saat proses kompilasi dengan menyertakan alamat berkas yang bisa kita lihat. Seperti ini penggalan berkas log yang menyebutkan kesalahan pada kompilasi tersebut:

```
In file included from /tmp/modconfig-fVhjg6/vmnet-only/net.h:38:0,
                 from /tmp/modconfig-fVhjg6/vmnet-only/vnetInt.h:26,
                 from /tmp/modconfig-fVhjg6/vmnet-only/netif.c:42:
/tmp/modconfig-fVhjg6/vmnet-only/vm_device_version.h:56:0: note: this is the location of the previous definition
#define PCI_VENDOR_ID_VMWARE                    0x15AD
  CC [M]  /tmp/modconfig-fVhjg6/vmnet-only/vnetUserListener.o
/tmp/modconfig-fVhjg6/vmnet-only/netif.c: In function 'VNetNetifStartXmit':
/tmp/modconfig-fVhjg6/vmnet-only/netif.c:468:7: error: 'struct net_device' has no member named 'trans_start'; did you mean 'mem_start'?
    dev->trans_start = jiffies;
       ^~
make[2]: *** [scripts/Makefile.build:290: /tmp/modconfig-fVhjg6/vmnet-only/netif.o] Error 1
make[2]: *** Waiting for unfinished jobs....
In file included from /tmp/modconfig-fVhjg6/vmnet-only/net.h:38:0,
                 from /tmp/modconfig-fVhjg6/vmnet-only/vnetInt.h:26,
                 from /tmp/modconfig-fVhjg6/vmnet-only/bridge.c:52:
/tmp/modconfig-fVhjg6/vmnet-only/vm_device_version.h:56:0: warning: "PCI_VENDOR_ID_VMWARE" redefined
#define PCI_VENDOR_ID_VMWARE                    0x15AD
In file included from include/linux/pci.h:35:0,
                 from /tmp/modconfig-fVhjg6/vmnet-only/compat_netdevice.h:27,
                 from /tmp/modconfig-fVhjg6/vmnet-only/bridge.c:51:
include/linux/pci_ids.h:2253:0: note: this is the location of the previous definition
#define PCI_VENDOR_ID_VMWARE  0x15ad
make[1]: *** [Makefile:1457: _module_/tmp/modconfig-fVhjg6/vmnet-only] Error 2
make[1]: Leaving directory '/usr/lib/modules/4.7.0-1-ARCH/build'
make: *** [Makefile:120: vmnet.ko] Error 2
make: Leaving directory '/tmp/modconfig-fVhjg6/vmnet-only'
Unable to install all modules.  See log for details.
```

Pada berkas tersebut menyebutkan bahwa adanya kesalahan yang terjadi saat proses kompilasi modul `vmnet-only`. Kesalahan terjadi pada blok berikut:

```
/tmp/modconfig-fVhjg6/vmnet-only/netif.c:468:7: error: 'struct net_device' has no member named 'trans_start'; did you mean 'mem_start'?
    dev->trans_start = jiffies;
       ^~
```

Saya coba mencari informasi tersebut dan mendapatkan nya pada forum Arch Linux. Ah ternyata ada juga yang mengalami hal yang sama(sehati :p). Pada _thread_ tersebut pengguna yang memakai _nickname_ glavin memberikan solusi sebagai berikut:

```
# cd /usr/lib/vmware/modules/source
# tar xf vmnet.tar
# mv vmnet.tar vmnet.old.tar
# sed -i -e 's/dev->trans_start = jiffies/netif_trans_update(dev)/g' vmnet-only/userif.c
# tar cf vmnet.tar vmnet-only
# rm -r vmnet-only

# vmware-modconfig --console --install-all

*Solusi sebelum direvisi
```

Saya mencobanya, dan hasilnya? Sama saja. Masih tetap bermasalah. Saya pikir dia _typo_ saat menuliskan nya dan kemudian melakukan revisi terhadap solusi tersebut. Tapi, saya lebih menyarankan menggunakan solusi berikut:

```
# cd /usr/lib/vmware/modules/source
# tar -xvf vmnet.tar
# cd vmnet-only
# sed -i -e 's/dev->trans_start = jiffies/netif_trans_update(dev)/g' netif.c
# make
# cd ..
# sudo cp vmnet.o /lib/modules/`uname -r`/kernel/drivers/misc/vmnet.ko.gz

Restart and reload vmware modules
# sudo depmod -a
# sudo systemctl restart vmware
```

Booom. VMWare berjalan dengan normal kembali dan sayapun bisa melanjutkan _development_ aplikasi yang saya buat sebelumnya  (deadline soalnya :p) . Terimakasih sudah membaca artikel ini dan semoga artikel ini bermanfaat.

Referensi:

* https://bbs.archlinux.org/viewtopic.php?pid=1646949
* http://rglinuxtech.com/?p=1748
* https://wiki.archlinux.org/index.php/VMware
