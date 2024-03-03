---
layout: post
title: Burn iso image
---

Cara burn iso image di linux terbilang sangat mudah, karena tool-nya sendiri sudah jadi bawaan di sebagian besar distro karena tool ini yang bernama "dd" sudah termasuk dalam bagian core utilities itu sendiri. 

sebelum melakukan burn pastikan storage device mana yang akan di-burn, bisa menggunakan perintah:
```bash
$ lsblk -f
```
setelah tahu disk apa yang akan di burn, semisal saya akan melakukan burn pada /dev/sdb1:
```bash
$ sudo dd if=/file/iso/yang/akan/diisi.iso of=/dev/sdb1 bs=1M status=progress
```
tunggu proses hingga selesai, setelah itu usb bisa langsung dicabut.

sebenarnya masih banyak penggunaan perintah dd seperti custom iso image, disk cloning dan semacamnya, terkait perintah dd bisa dibaca di arch wiki 
[dd - archlinux wiki](https://wiki.archlinux.org/title/Dd)
