---
layout: post
title: Battery linux
---

Linux secara default belum melakukan optimalisasi baterai pada laptop seperti halnya di Windows, namun kita bisa sedikit konfigurasi agar bisa lebih hemat daya.


> Dalam kasus ini hardware yang digunakan adalah processor Intel, hardware yang berbeda seperti AMD atau arsitektur yang berbeda seperti ARM mungkin memiliki cara yang berbeda. Melakukan tweak pada sistem juga beresiko berbagai malfungsi seperti freezing, harap untuk selalu berhati-hati. 

Berikut cara untuk melakukan optimalisasi baterai pada linux:

## Kernel parameter

Tambahkan kernel parameter pada /etc/default/grub di bagian "GRUB_CMDLINE_LINUX_DEFAULT=" :

`"i915.i915_enable_rc6=1 i915.i915_enable_fbc=1 i915.lvds_downclock=1"`

## Tingkat kecerahan

Kurangi kecerahan dengan melakukan setting backlight, bisa juga secara manual edit kecerahan sebagai root:

echo 100 > /sys/class/backlight/intel_backlight/brighness

## Suspend/Hibernate

Suspend adalah menyimpan proses ke dalam ram untuk menghemat baterai, begitu halnya hibernate namun hibernate akan menyimpanya di disk, hibernate akan lebih menghemat daya, namun menambah read/write pada disk.

untuk pengguna systemd: 

```bash
# untuk suspend
systemctl suspend
# untuk hibernate
systemctl hibernate
```
## Powertop

Powertop adalah salah satu binary yang digunakan untuk monitor dan optimisasi penggunaan daya di linux, berikut cara instalasi powertop:

```bash
sudo pacman -S powertop
```
Untuk menjalankan powertop cukup jalankan
```
sudo powertop
```
Agar powertop otomatis mengkonfigurasi:
```
sudo powertop --auto-tune
```
 
## Mematikan service yang tidak dipakai

Pastikan apakah service ini benar-benar dipakai atau tidak.

cek service yang berjalan

```bash
systemctl --type=service
# atau untuk keseluruhan
systemctl
systemctl list-units
systemctl list-unit-files
```

Untuk mematikan service 

```bash
# mematikan service
sudo systemctl stop nama_service.service
# men-disable service (agar service tidak berjalan setiap boot)
sudo systemctl disable nama_service.service
# mematikan sekaligus disable
sudo systemctl disable --now nama_service.service
```

## Mematikan kernel module

Matikan beberapa kernel module yang tidak digunakan.

Melihat list module:

`lsmod`

menghapus module sementara:

```bash
rmmod nama_module
# atau
modporbe -r nama_module
```

Sisable module, edit file /etc/modprobe.d/disabled.conf

`disable nama_module`

lihat artikel konfigurasi laptop untuk contoh disable module.

## Laptop mode tools

Arch linux menyediakan package yang bernama laptop-mode-tools, tool ini digunakan untuk melakukan optimisasi daya.

Laptop-mode-tools ada di repository aur, untuk instalasinya bisa menggunakan aur helper atau manual mendownload pkgbuild, atau bisa langsung mendownload dari [git repo](https://github.com/rickysarraf/laptop-mode-tools/)

## Undervolt

Undervolt adalah proses mengurangi voltase cpu bertujuan untuk mengurangi pengunaan energi dan panas tanpa mengurangi performa, motherboard saat ini kebanyakan mampu melakukan undervolt melalui bios(dilansir arch wiki bahwa melakukan undervolt intel dari bios sudah tidak bisa dilakukan karena alasan vulnerability).

Pertama install intel-undervolt

`sudo pacman -S intel-undervolt`

Untuk melakukan monitoring intel-undervolt:

```
sudo intel-undervolt measure
```

Edit konfigurasi file di /etc/intel-undervolt.conf

```
# undervolt sekitar -100 mW 
undervolt 0 'CPU' -100
undervolt 1 'GPU' -100
undervolt 2 'CPU Cache' -100
undervolt 3 'System Agent' 0
undervolt 4 'Analog I/O' 0
# mengurangi maksimal temperatur pc, jika maksimal 100 maka akan dikurangi menjadi 60
tjoffset -40
```

Lalu ketik perintah dibawah berikut untuk meng-apply perubahan konfigurasi:
```
sudo intel-undervolt apply
```

Sauce:


[wiki.archlinux.org - Power Management](https://wiki.archlinux.org/title/Power_management)
[wiki.archlinux.org - Undervolting CPU](https://wiki.archlinux.org/title/Undervolting_CPU)
[github.com](https://github.com/kitsunyan/intel-undervolt)
[phoronix.com](https://www.phoronix.com/review/intel_i915_power) 
[baeldung.com](https://www.baeldung.com/linux/optimize-battery-life) 
