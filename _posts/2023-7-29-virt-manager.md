---
layout: post
title: Setup Virt Manager
---

Virt Manager adalah salah satu frontend untuk mengatur virtualisasi, Virt Manager bisa menjalankan beberapa virtualisasi seperti KVM yang paling utama, namun Virt Manager juga bisa menjalankan virtualisasi Xen dan LXC(linux container).

Qemu+KVM+Virt-Manager bisa menjadi alternative Virtualbox karena jauh lebih efektif dan efisien, terutama dari segi performa jauh lebih cepat.

Dalam kasus ini saya akan melakukan setup simple kombinasi qemu-kvm-virt manager.

Pastikan device yang dipakai support virtualisasi, jika shell memberikan output berarti device sudah support untuk virtualisasi.

`$ LC_ALL=C lscpu | grep Virtualization`

> Dalam kasus tertentu perlu untuk mengkonfigurasi BIOS untuk meng-enable virtualisasi

Lalu lakukan instalasi qemu

```bash
# untuk distro archlinux
sudo pacman -S qemu-full bridge-utils virt-manager dnsmasq iptables-nft
# nama package menyesuaikan distro masing-masing
```
Setelah instalasi, buat virtual adapter virbr0 untuk nat, ini akan jauh mempermudah terutama jikalau kita menggunakan wifi dimana kita tau akan sulit untuk autentikasi.

Pertama buat konfigurasi untuk virtual adapter virbr0 di /tmp/default.xml

```xml
<network>
  <name>default</name>
  <bridge name="virbr0"/>
  <forward mode="nat"/>
  <ip address="192.168.10.1" netmask="255.255.255.0">
    <dhcp>
      <range start="192.168.10.2" end="192.168.10.254"/>
    </dhcp>
  </ip>
</network>
```
Konfigurasi akan disimpan di /etc/libvirtd/qemu/networks/default.xml

Lalu jalankan sekaligus enable service libvirtd:

`sudo systemctl enable --now libvirtd `

Setelah itu jalankan perintah berikut:
```bash
virsh net-define /tmp/default.xml
sudo virsh net-start default
sudo virsh net-autostart default
```
Kita bisa cek adapter dengan perintah `ip a` atau `ifconfig`.

Setup sudah selesai, kita cukup buka virt-manager.

Berikut cara untuk menghapus virtual adapter virbr0 jikalau ingin mengganti atau menghapus:
Pertama kita matikan sekaligus mendisable service libvirtd

`sudo systemctl disable --now libvirtd`

Selanjutnya kita cek adapter mana yang akan kita hapus

`sudo virsh net-list`

Lalu hapus adapter, dalam kasus ini nama adapternya adalah default.

```bash
sudo virsh net-destroy default
sudo virsh net-undefine default
```
Virtual adapter telah terhapus.

saus:

[virt-manager.org](https://virt-manager.org/)

[archlinux.org - qemu](https://wiki.archlinux.org/title/QEMU)

[bbs.archlinux.org](https://bbs.archlinux.org/viewtopic.php?id=242808)

[archlinux.org - KVM](https://wiki.archlinux.org/title/KVM)

[thegeekdiary.com](https://www.thegeekdiary.com/how-to-remove-virbr0-and-lxcbr0-interfaces-on-centos-rhel-5-and-rhel-7/)

