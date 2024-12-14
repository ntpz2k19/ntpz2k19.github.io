---
layout: post
title: "Reset Cisco device"
---

Cisco memiliki berbagai jenis perangkat dan untuk melakukan reset pada perangkat tersebut diperlukan proses tertentu. Pada artikel ini akan dibahas cara melakukan hard reset pada perangkat Cisco switch Catalyst 2960 dan Cisco router 1921.

## Cisco 2960 reset 
Berikut langkah-langkah untuk melakukan reset pada cisco 2960:

Pertama hidupkan perangkat, dan tekan tombol mode di ujung perangkat, tombol tersebut akan menghalangi proses booting perangkat dan memasukan ke dalam model flash. 

Setelah itu, ketik perintah `flash_init`.

Lalu hapus konfigurasi file, yaitu : `vlan.dat` dan `config.text`.
```
switch: delete flash:vlan.dat
switch: delete flash:config.text
```
Setelah file terhapus langkah terakhir ketik perintah: `reset`.

## Cisco 1921 reset
Berikut langkah-langkah untuk melakukan reset pada Cisco 1921:

Pertama hidupkan router, di saat yang bersamaan lakukan send break signal di kabel console, untuk software minicom bisa dengan mengetik `Ctrl a + f`.

Kedua, dengan mengirim sinyal break maka proses booting terhalang dan memasuki mode rommon. di dalam mode rommon lalu ketik perintah berikut:
```
rommon > confreg 0x2124
rommon > reset
```
Setelah itu router akan melakukan reboot, setelah reboot lakukan beberapa konfigurasi:
```
Router> enable
Router# configure terminal
Router(config)# config-register 0x2102
Router(config)# exit
Router# write memory
```
Router sukses di reset.


saus:

[css.instech.washington.edu](http://css.insttech.washington.edu/~lab/Support/HowtoUse/cisco_reset.html)

