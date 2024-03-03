---
layout: post
title: "Auto startx"
---
Xorg-xinit dipakai untuk menjalankan xorg server secara manual, untuk menjalankanya yaitu dengan perintah startx.

Saya sangat suka dengan ini karena cukup simpel dan ramah resource, terutama pengguna window manager, disini saya akan menjadikan perintah startx otomatis jalan setelah login.

Untuk pengguna shell bash tambahkan config ini di ~/.bash_profile, untuk pengguna shell zsh bsa di ~/.zprofile .

```bash
if [[ -z $DISPLAY ]] && [[ $(tty) = /dev/tty1 ]]; then
        startx
fi
```
saus: [https://bbs.archlinux.org/viewtopic.php?id=144119](https://bbs.archlinux.org/viewtopic.php?id=144119)
