---
layout: post
title: Hardening ssh server
---

SSH atau Secure Shell adalah metode remote console yang bisa diakses jarak jauh, dan karena itu harus diperketat keamananya agar tidak mudah terserang.

Berikut cara untuk pengerasan ssh server:

Edit atau tambahkan line di /etc/ssh/sshd_config di komputer server

```bash
#ubah port autentikasi
Port 9876

#Ganti ke protokol ssh versi 2
Protocol 2

#Perketat autentikasi login
PermitRootLogin probibit-password

#Tolak izin login dengan password kosong
PermitEmptyPassword no

#tolak login dengan password,hanya dengan autentikasi key saja
PasswordAuthentication no

#Tolak izin terusan dengan protokol tcp dan x11, ini berarti ssh tidak bisa mengakses remote desktop
AllowTcpForwarding no
X11Forwarding no
```

Lalu buat authentikasi dengan public key di komputer client
```bash
$ ssh-keygen
```
Kunci authentikasi akan di simpan di ~/.ssh/ dengan file id_rsa sebagai private key dan id_rsa.pub sebagai public key.

Untuk mengirim kunci authentikasi ke server dilakukan dengan perintah:
```bash
ssh-copy-id -i /home/nama_user/.ssh/id_rsa.pub user@server.com
```
Nanti akan diminta authentikasi, setelah diberi authentikasi, tinggal disable password authentikasi agar hanya bisa diakses dengan auth key.

Lalu lakukan restart pada ssh service.
```bash
#untuk systemd
$ sudo systemctl restart sshd
#untuk openrc
$ sudo rc-service sshd start
#untuk runit di artix 
$ ln -s /etc/runit/sv/sshd /run/runit/service/.
```
Karena di ssh server telah di set spesifik port, maka cara menghubungkanya dengan perintah:
```bash
$ ssh -p 9876 user@server.com
```

Untuk test keamanan ssh lebih lanjut bisa dengan [sshaudit.com](https://sshaudit.com).

Saus:

[Youtube](https://www.youtube.com/watch?v=l1iu3iZq1aQ)

[makeuseof](https://www.makeuseof.com/ways-to-secure-ssh-connections-linux/)

[phoenixnap](https://phoenixnap.com/kb/ssh-with-key)

Bonus saus:

[stackoverflow](https://stackoverflow.com/questions/2419566/best-way-to-use-multiple-ssh-private-keys-on-one-client)
