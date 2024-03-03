---
layout: post
title: "Openssh cisco"
---
Kali ini saya berkesempatan mengikuti training cisco, namun saat saya mencoba mengakses console cisco dengan openssh di linux, saya mengalami beberapa permasalahan, terlebih detailnya adalah karena algoritma yang kurang spesifik.

Untuk mengatasi masalah algoritma, cukup kita edit di file konfigurasi ~/.ssh/config :
```
HostkeyAlgorithms ssh-dss,ssh-rsa
KexAlgorithms +diffie-hellman-group14-sha1,diffie-hellman-group1-sha1
Ciphers aes128-cbc,3des-cbc,aes192-cbc,aes256-cbc,aes128-ctr,aes192-ctr,aes256-ctr
```

saus:

[youtube.com](https://www.youtube.com/watch?v=hAipb0Wv3d8)
