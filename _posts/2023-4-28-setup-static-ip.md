---
layout: post
title: Cara setup static IP di linux
---

Static IP adalah IP yang tetap, tidak berubah-ubah tidak seperti dhcp, biasanya sering diterapkan di router ataupun server agar ip nya tetap diingat, berbeda dengan client biasa seperti mobile atau pc yang tidak begitu memerlukan static ip.

Berikut beberapa cara untuk melakukan setup static IP di linux:

## systemd-networkd

Untuk sistem linux yang memang mayoritas menggunakan systemd bisa menggunakan systemd-networkd untuk melakukan static ip.

Pertama buat service file /etc/systemd/network/enp0s3.network (dalam kasus ini adapter yang dipakai adalah enp3s0, nama adapter mungkin akan berbeda)

Edit file sebagai berikut:
```bash
[Match]
Name=enp0s3

[Network]
Address=192.168.1.102/24
Gateway=192.168.1.1
DNS=8.8.8.8
DNS=8.8.4.4
```

> sebelumnya coba cek apakah ada proses yang melibatkan ip address, karena itu akan mengganggu konfigurasi jaringan, bisa dicek dengan perintah sudo systemctl list-unit-file , ada kemungkinan beberapa proses seperti dhcpcd, networkmanager, nectl, netplan ikut berjalan.

Setelah itu tinggal jalankan dan enable service yang tadi dibuat dengan perintah `sudo systemctl enable --now systemd-networkd`

## Setup static di debian

Untuk debian bisa dengan mengedit file di /etc/network/interfaces

```bash
# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
#allow-hotplug enp0s3
#iface enp0s3 inet dhcp
auto enp0s3
iface enp0s3 inet static
  address 192.168.4.150
  netmask 255.255.255.0
  gateway 192.168.4.1
  dns-nameservers 192.168.4.1 8.8.8.8 1.1.1.1

auto enp0s8
iface enp0s8 inet static
  address 10.20.30.1
  netmask 255.255.255.0
  network 10.20.30.0
  broadcast 10.20.30.255
```

## Netplan

Adapun dengan netplan cukup dengan mengedit file yang berformat .yaml di direktori /etc/netplan/

```bash
# This is the network config written by 'subiquity'
network:
  ethernets:
    enp0s3:
      dhcp4: no
      addresses: [192.168.4.214/24]
      routes:
        - to: default
          via: 192.168.4.1
      nameservers:
        search: [petik.or.id]
        addresses: [192.168.4.1, 8.8.8.8]
    enp0s8:
      dhcp4: no
      addresses: [192.168.114.1/24]
  version: 2
```

## Iproute2

Pertama cek ip dengan perintah `ip a` untuk memastikan adapter mana yang mau diedit, lalu kita flush agar ip dalam satu adapter tidak lebih dari 1 `sudo ip a flush enp3s0`

Setelah itu tambahkan ip dengan perintah `sudo ip a add 192.168.1.20/24 dev enp3s0` dalam kasusnya kita pakai adapter enp0s3 dan netmask 24 (255.255.255.0).

Lalu kita aktifkan adapter dengan perintah `sudo ip link set enp0s3 up`

> untuk ip route bersifat temporer, setelah reboot kofigurasi akan ter-reset, jika ingin secara permanen dengan perintah ip maka bisa dengan custom daemon script  menyesuaikan init system agar perintah dijalankan setelah booting.




Saus:

[blog.dbrgn.ch](https://blog.dbrgn.ch/2014/1/14/setting_static_ip_with_iproute2/)
[ostechnix.com](https://ostechnix.com/configure-static-dynamic-ip-address-arch-linux/)
[cyberciti.biz](https://www.cyberciti.biz/faq/add-configure-set-up-static-ip-address-on-debianlinux/)
[the-empire.systems](https://the-empire.systems/arch-linux-router/)
[linuxopsys.com](https://linuxopsys.com/topics/permanently-add-static-route-in-linux)
