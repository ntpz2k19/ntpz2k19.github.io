---
title: fork bomb
layout: post 
---

Fork bomb atau bisa disebut bom garpu adalah salah satu jenis serangan terhadap sistem yang menduplikat proses dan menyebabkan sistem mengalami down dikarenakan jumlah proses yang terlalu banyak.

Berikut penerapan fork bomb di sistem *nix dan windows:
```bash
#Bash shell
:(){:|:&};:
#atau agar lebih mudah dibaca:
bomb(){bomb|bomb&};bomb
#dalam sistem windows
%0|%0
```

Karena fork bomb dibuat untuk menduplikat dan memperbanyak proses, cara penanganan paling mudah untuk sistem linux adalah dengan membatasi proses tiap user dan group, edit file /etc/security/limit.conf, berikut konfigurasi paling mudah:

```bash
#membatasi user ntapz sejumlah 3000 proses
ntapz   hard    nproc   3000
#membatasi group wheel sejumlah 2000 proses
@wheel  hard    nproc   2000
```
saus:

[supportpro.com](https://www.supportpro.com/blog/what-is-a-fork-bomb-and-how-can-it-be-prevented/)

[teknopedia.teknokrat.ac.id](https://teknopedia.teknokrat.ac.id/wiki/Fork_bomb)
