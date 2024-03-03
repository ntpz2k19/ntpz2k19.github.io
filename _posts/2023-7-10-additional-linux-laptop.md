---
layout: post
title: Konfigurasi laptop linux
---

Saya menambahkan artikel ini saat saya menjalankan linux di laptop. karena ada beberapa fitur di laptop yang mungkin perlu sedikit di konfigurasi dengan cara sederhana.

## Touchpad

Touchpad secara default di linux tidak bisa melakukan tap touching seperti halnhya di  windows, Namun ada cara cukup simple yang bisa dilakukan, yaitu menjalankan  perintah:

```bash
synclient TapButton1=1 &
```

Agar perintah ini bisa dijalankan tiap kali booting maka bisa ditambahkan di startup, untuk saya cukup saya simpan di .xinitrc, untuk  pegguna openbox bisa di tambahkan perintah di ~/.config/openbox/autostart .

## Brightness

Mengkonfigurasi brightness di window manager openbox sejauh ini saya lakukan secara manual dengan perintah sebagai root user.
```bash
echo 2500 > /sys/class/backlight/intel_backlight/brightness
```
>Direktori intel_backlight bisa berbeda tergantung vendor,dalam kasus ini vendor yang dipakai adalah Intel.

Angka 2500 bisa dirubah sesuai dengan kecerahan yang diperlukan, untuk orang yang tidak suka terang seperti saya maka saya atur dengan angka yang rendah.

Untuk lebih mudahnya bisa dibuat shell scripting, di letakan di direktori /root/setbacklight .
```bash
#!/bin/bash
read -p "Masukan angka untuk tingkatan pencahayaan:(1800-5000) " light
if ((light >= 1800 && light <= 5000)); then
        echo $light > /sys/class/backlight/intel_backlight/brightness
        echo "Pencahayaan telah di set"
else
        echo "angka yang anda masukan tidak valid"
fi

```
Beri akses executable pada file.

```
sudo chmod +x /root/setbacklight
```


## Keyboard Beep

keyboard beep cukup mengganggu, untuk mendisable bisa dengan mendisable kernel module:
```bash
# cek module pcspkr
sudo lsmod | grep pcspkr
# disable pcspkr
sudo rmmod pcspkr
# disable permanen pcspkr
echo "blacklist pcspkr" > /etc/modprobe.d/nobeep.conf
```

saus: 

[superuser.com - keyboard beep](https://superuser.com/questions/1507961/how-to-disable-the-beep-on-lenovo-p1-gen-2-with-manjaro-arch)

[archlinux.org - backlight](https://wiki.archlinux.org/title/backlight)

[blog.desdelinux.net - toucpad](https://blog.desdelinux.net/en/tip-activar-click-con-el-touchpad-en-openbox/)
