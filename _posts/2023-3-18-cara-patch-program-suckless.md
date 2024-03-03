---
layout: post
title: Patching Suckless
---

Melakukan patch pada software suckless sebenarnya tidaklah begitu sulit, tapi bisa jadi benar-benar sulit, terutama bagi saya yang sangat awam di dunia linux apalagi program yang ditulis oleh suckless menggunakan bahasa C.

pertama mari kita Download st dengan wget :
![download-st]({{ site.baseurl }}/images/patch1.png)

lalu extract file yang telah di download :

![extract-st]({{ sitebase.url }}/images/patch2.png) 

Untuk melakukan patch pada suckless sebenarnya cukup simple, tinggal lakukan

```bash
$ patch -p1 < nama-patch.diff
```
Semisal kita akan melakukan patch padaa st saat telah di download dan di extract, untuk file patch st bisa di dapat di [patch](https://st.suckless.org/patches/) , kita akan melakukan beberapa patch yaitu scrollback dengan beberapa fitur agar bisa scroll, lalu untuk color scheme saya ambil favorit saya yaitu dracula karena cukup nyaman dimata.

![dl-patch]({{ site.baseurl }}/images/patch3.png)

Disini saya melakukan patch st-scrollback

![patching1]({{ site.baseurl }}/images/patch4.png)

Setelah patch bisa ditambah patch lain, seperti saya tambahkan scrollback mouse altscreen, sekaligus saya tambahkan dracula patch.

Setelah selesai patching, tinggal di make untuk menginstall st

```bash
$ sudo make clean install
```

Jika gagal patch, akan diberi informasi di file dengan akhiran .rej , disitu kita bisa patch manual dengan cara membuka kedua file nya, yaitu semisal config.h dan config.h.rej, lalu lakukan patch manual sesuai arahan config.h.rej, jika + tambahkan baris sesuai posisi file nya, jika – maka hapus baris tersebut.

untuk mengembalikan patch cukup dengan perintah

```bash
$ patch -R < nama-patch.diff
```

saus:
[suckless](https://suckless.org/hacking/)
[Yt source](https://youtu.be/h02KpjxFc7k)





