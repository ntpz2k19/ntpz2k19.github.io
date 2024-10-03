---
title: "Freebsd Wireguard Multiple client"
layout: post
---

Wireguard adalah salah satu protokol vpn yang dirilis pada tahun 2015 dan termasuk protokol vpn paling baru. Wireguard juga men-support banyak sistem operasi mulai dari Windows, Linux, FreeBSD, OpenBSD, MacOS, Android, dan juga iOS. Untuk membuat server wireguard di dalam mesin FreeBSD diperlukan beberapa langkah berikut:


![wg1]({{ site.baseurl }}/images/wg1.png)

## Instalasi 

Lakukan upgrade dan upgrade package terlebih dahulu:
```
sudo pkg update
sudo pkg upgrade
```
Lalu install package wireguard-tools
```
sudo pkg install wireguard-tools
```
## Setup key server
Konfigurasi wireguard terletak di direktori `/usr/local/etc/wireguard', beri akses privilege terlebih dahulu.
```
sudo chmod -R 770 /usr/local/etc/wireguard
```
Lalu masuk ke direktorinya
```
cd /usr/local/etc/wireguard
```
Buat server private key
```
wg genkey | tee server_private.key
```
Nanti outputnya kira kira seperti ini: `6EJJwSZoabkORF5Sht8Z4sgaPBxnLi7LX3BMOcPXdFQ=`.

Generate public key berdasarkan private key.
```
wg pubkey < server_private.key > server_public.key
```
Isi public key server kira kira seperti ini `dBFlMraTi5pAejIbef9glOwZZQMk3XAJu1wjgALVJnc=`

Buat file konfigurasi interface wg0 di `/usr/local/etc/wireguard/wg0.conf`. Ganti address sesuai selera, dan ganti Private key sesuai dengan isi file `server_private.key`.
```
[Interface]
Address = 172.16.10.1/24
SaveConfig = true
ListenPort = 51820
PrivateKey = 6EJJwSZoabkORF5Sht8Z4sgaPBxnLi7LX3BMOcPXdFQ=
```

## Setup key client
Buat private key untuk client.
```
wg genkey | tee client_private.key
```
Seperti ini contoh isi key: `UOeEYaXxV5eGdTdyELsJUpK3i3OQJWOyMm6vzqW5D1M=`.

Lalu buat public key untuk client.
```
wg pubkey < client_private.key > client_public.key
```
Seperti ini contoh isi key:`rCh6folxNHzIQJpa1c1mVUqnMxnnTu8/xTpFfJ582gY=`.

## Client configuration file

Buat file konfigurasi client di `/usr/local/etc/wireguard/wg_client1.conf`, file inilah yg akan disalin ke client untuk koneksi ke VPN server.
```
[Interface]
PrivateKey = 6EJJwSZoabkORF5Sht8Z4sgaPBxnLi7LX3BMOcPXdFQ=
Address = 192.168.100.2/24

[Pvir]
PublicKey = uSYKhXf8rbJfZNtUNuRwzAOYmjBnre3O2T+Isv4f+k0=
AllowedIPs = 0.0.0.0/0
Endpoint = 103.10.17.25:51820
PersistentKvipalive = 30
```



Saus:

[vultr.com](https://docs.vultr.com/how-to-install-wireguard-vpn-on-freebsd-14-0)

[github.com](https://gist.github.com/etiennetremel/a90d898103b0d3e450bc53d428a47e91)
