---
title: "fix fakedisk f3"
layout: post 
---
Banyak beredar flashdisk palsu yg menjajikan kapasitas besar, namun faktanya kapasitas tersebut tidak sesuai dengan apa yg ada di dalam flaskdisk tersebut. flaskdisk palsu juga cukup membayahayakan penggunanya dikarenakan kapasitasnya tidak sesuai dengan aslinya, maka file akan menjadi corrupt dan tidak bisa dibuka sama sekali.

Untuk mengatasi flaskdisk palsu yang terlanjur terbeli, ada salah satu tool di sistem linux bernama f3 alias fake flash fraud. Tool tersebut digunakan mengembalikan ukuran asli disk dengan cara mempartisi block yg bisa digunakan. Berikut cara menggunakan f3:

list disk yg akan digunakan:
```
$ lsblk
```
dalam kasus ini disk yg akan diperbaiki adalah /dev/sdb, ketik perintah:
```
$ sudo f3probe -n -t /dev/sdb
```
![fakedisk-1]({{ site.baseurl }}/images/fakedisk-1.png) 

lihat outputdari perintah sebelumnya, Setelah itu lanjut ketik perintah dengan menyesuaikan instruksi diatas:
```
$ sudo f3fix --last-sec=32852511 /dev/sdb
```
![fakedisk-2]({{ site.baseurl }}/images/fakedisk-2.png) 

Jika selesai disk akan otomatis terpartisi oleh f3, dan akan muncul partisi /dev/sdb1. untuk kasus ini partisi akan diformat menjadi exfat karena support multiplatform dan melengkapi keterbatasan fat32 yang maksimal per file nya hanya 4GB.
```
$ sudo mkfs.exfat /dev/sdb1 -n label_disk
```



saus:

[fight-flash-fraud.readthedocs.io](https://fight-flash-fraud.readthedocs.io/en/latest/index.html)
