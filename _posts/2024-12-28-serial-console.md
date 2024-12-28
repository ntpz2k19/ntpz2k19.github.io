---
layout: post
title: "Serial console Minicom"
---

## Serial console
Serial console adalah perangkat yang biasa digunakan untuk melakukan console pada perangkat secara langsung tanpa melalui koneksi jaringan. Serial console biasa berbentuk USB dengan ujung RJ-45, ada juga dengan ujung Db9. Tool yang biasa digunakan untuk berkomunikasi menggunakan serial console adalah minicom.

Untuk menggunakan Minicom, perlu diketahui device file di dalam direktori `/dev`, untuk mengetahui cukup ketik perintah `sudo dmesg| grep tty`.

![serial-1]({{ site.baseurl }}/images/serial-1.png)

Setelah mengetahui perangkat serial yang akan digunakan, perlu disiapkan setup agar setiap membuka minicom sudah otomatis dengan konfigurasi serial yang diperlukan, ketik perintah `sudo minicom -s`.

Setelah mengesekusi minicom, lakukan setup konfigurasi minicom. 

![serial-2]({{ site.baseurl }}/images/serial-2.png)

Saat memasuki setup, terdapat beberapa pilihan, ketik `c` untuk set 9600 bitrate, lalu ketik `q` untuk set stop bit di 8N1, setelah itu ketik enter untuk konfirmasi konfigurasi.

![serial-3]({{ site.baseurl }}/images/serial-3.png)

Setelah itu save file di menu utama.

![serial-4]({{ site.baseurl }}/images/serial-4.png)

Konfigurasi tersebut akan tersimpan di dalam file `/etc/minirc.cisco`.

![serial-5]({{ site.baseurl }}/images/serial-5.png)

## Keybinds & Cheatsheet
Untuk menggunakan Minicom, ada beberapa keybind dan cheatsheet yang perlu diketahui. Berikut keybind yang perlu di ketahui:

**Keybind**|**Fungsi**
Ctrl a + z|Menampilkan menu keybind.
Ctrl a + q|Keluar dari Minicom.
Ctrl a + f|Mengirim sinyal break pada perangkat.

`sudo minicom -b 9600 -D /dev/ttyUSB0 -C Documents/router-log.txt`

**Parameter**|**Penjelasan**
-b|Baudrate
-D|device
-C|Output log

saus:

[stackoverflow.com](https://stackoverflow.com/questions/2530096/how-to-find-all-serial-devices-ttys-ttyusb-on-linux-without-opening-them)

