---
layout: post
title: kombo maut st + tabbed
---

Kali ini saya melirik salah satu tool dari suckless yang bernama tabbed, tabbed memungkinkan kita untuk melakukan tabbing jikalau ada suatu aplikasi tidak memiliki fitur tabbing, tabbed sendiri cukup keren menurut saya karena dalam filosofi KISS(Keep It Simple,Stupid), lebih baik membuat suatu aplikasi baru ketimbang menambah fitur agar mempermudah development.

Tabbed sendiri dibuat agar browser buatan suckless yang bernama surf bisa melakukan tabbing, namun tabbed tidak terkhusus pada surf, namun bisa juga aplikasi lain, untuk kasus ini saya menggunakan tabbed untuk terminal favorit saya yaitu st.

Pertama kita clone repository, disini saya menggunakan tabbed hasil modifikasi distrotube:

`git clone https://gitlab.com/dwt1/tabbed-distrotube.git`

Lalu kita coba make untuk memastikan apakah file bisa di compile atau tidak.

`make`

Jika compiling sukses kita langsung saja lakukan instalasi.

`sudo make install`

Instalasi tabbed selesai, sekarang kita tambahkan keybind baru, dalam kasus ini saya menggunakan openbox dimana konfigurasi usernya berada di .config/openbox/rc.xml.

```xml
    <keybind key="C-A-t">
      <action name="Execute">
        <command>tabbed -r 2 st -w ''</command>
      </action>
    </keybind>

```

Dalam konfigurasi ini kita menambahkan keybind tabbed+st dengan kunci Ctrl+Alt+t.

Berikut keybind tabbed:

`Ctrl+Alt+Enter` | Membuka Terminal baru 
`Ctrl+Alt+l` | Pindah ke kanan tab   
`Ctrl+Alt+h` | Pindah ke kiri tab    
`Ctrl+Alt+q` | Menghapus Tab         

saus:

[gitlab.com - distrotube](https://gitlab.com/dwt1/tabbed-distrotube)

[suckless.org - tabbed](https://tools.suckless.org/tabbed)

[youtube.com - distrotube](https://youtu.be/-Xr4ouz8zQA)

[pre.tir.tw](https://pre.tir.tw/008/blog/output/st-tabbed.html)
