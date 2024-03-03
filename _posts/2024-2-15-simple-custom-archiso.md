---
title: Simple custom Archiso
layout: post
---

Archiso adalah salah satu utility dari Arch Linux yang memungkinkan untuk membuat custom iso sesuai yang diinginkan dimulai dari package yang ingin digunakan hingga konfigurasi seperti apa yang diinginkan. Alasan saya terpikir untuk menggunakan Archiso adalah agar memudahkan untuk recovery system seperti Backup file, reset password, dan keperluan lainya.

Berikut cara untuk melakukan custom Archiso:

```bash
#install pakage arch iso
$ sudo pacman -S archiso
# aur juga menyediakan archiso-git dan archiso-profiles
# untuk tambahan profile.
```
Selanjutnya membuat custom profile, dari arch sendiri disarankan menggunakan profile releng, lakukan copy profile kedalam direktori lokal.

```bash
$ sudo cp -r /usr/share/archiso/configs/releng/ archlive-custom
```
Berikut kira-kira isi direktori profile:
```
archlive-custom
├── airootfs
│   ├── etc
│   ├── root
│   └── usr
├── bootstrap_packages.x86_64
├── efiboot
│   └── loader
├── grub
│   ├── grub.cfg
│   └── loopback.cfg
├── packages.x86_64
├── pacman.conf
├── profiledef.sh
└── syslinux
    ├── archiso_head.cfg
    ├── archiso_pxe.cfg
    ├── archiso_pxe-linux.cfg
    ├── archiso_sys.cfg
    ├── archiso_sys-linux.cfg
    ├── archiso_tail.cfg
    ├── splash.png
    └── syslinux.cfg
```
Karena custom yang akan dilakukan sesederhana mungkin, yang diperhatikan adalah daftar package dan direktori root.

File daftar package ada di dalam file pacakges.x86_64,hapus daftar dhcpcd dan tambahkan beberapa package berikut di akhir file(atau bisa ditambah package lain yang diperlukan):
```
networkmanager
network-manager-applet
xorg
xorg-xinit
lxqt
breeze-icons
gparted
chntpw
smartmontools
```
Karena dari iso Arch linux sendiri sudah autologin root, tidak perlu lagi konfigurasi login tty kecuali diperlukan.

Untuk langsung masuk graphical, karena menggunakan xinit maka edit file di dalam direktori airootfs/root/ yaitu file .zlogin dan file .xinitrc .

```bash
# tambahkan di akhir file .zlogin
startx 
# tambahkan di file  .xinitrc
exec startlxqt
```
Untuk konfigurasi tambahan bisa diedit di airootfs/etc/ 

untuk pembuatan iso lakukan peintah berikut:
```bash
#buat direktori baru untuk hasil file.
mkdir result
#mulai proses pembuatan iso.
mkarchiso -v -w archiso-custom/ -o result/ archiso-custom/
```
Tunggu proses pembuatan iso, Proses pembuatan akan menggunakan banyak resource processor. untuk hasil bisa dilihat di direktori result/  

saus:

[wiki.archlinux.org](https://wiki.archlinux.org/title/archiso)
