---
layout: post
title: Cara reset password
---

Ada beberapa cara reset password dengan linux, bahkan untuk reset password di sistem operasi windows pun bisa dilakukan di linux,sebelumnya pastikan sudah membuat bootable linux, entah menggunakan tool untuk mem-burn cd seperti rufus,etcher,dd,dst. berikut beberapa langkah-langkah yang akan saya demonstrasikan.

## Reset root Linux

Lakukan booting ke usb yang telah di isi linux, untuk caranya bisa dengan masuk ke boot menu yang di sesuaikan dengan motherboard masing-masing. Untuk kasus ini saya menggunakan Archiso.


![chroot-1]({{ site.baseurl }}/images/chroot-1.png)

Lihat partisi yang ada di hardisk dengan perintah fdisk atau lsblk, kira-kira mana yang partisi root karena kita akan melakukan chroot.

![chroot-2]({{ site.baseurl }}/images/chroot-2.png)

Kalau untuk desktop biasanya root adalah partisi paling besar pertama atau kedua setelah home, maka kita coba mount partisi paling besar yang kira2 bisa di-chroot, mari kita coba mount partisinya di /mnt.

![chroot-3]({{ site.baseurl }}/images/chroot-3.png)

> nb: untuk melakukan chroot di arch bisa dengan perintah arch-chroot, sama halnya dengan artix dengan perintah artix-chroot, namun untuk distribusi lain cukup chroot saja.

Lalu langsung chroot saja

```bash
# chroot /mnt
```

Setelah chroot langsung saja ganti password root nya, jika ada user bisa juga ganti password user nya dengan perintah
```bash
# passwd root
# passwd nama_user
```
Nanti akan diminta password baru, isi dengan password baru dan juga tulis ulang untuk konfirmasi nya.

Setelah selesai maka unmount lagi partisi nya dan tinggal matikan dengan perintah poweroff.
```bash
# umount /mnt
# poweroff
```

## Reset Windows Administrator

Untuk melakukan reset pada windows password administrator, dalam linux bisa menggunakan tool yang bernama chntpw. untuk instalasi bisa gunakan package manager tiap-tiap distro.
```bash
## untuk Arch dan turunanya
# pacman -S chntpw

## untuk Debian dan turunanya
# apt install chntpw

## untuk Redhat dan turunanya
# dnf install chntpw
```

Sebelumnya mari kita lakukan mounting disk terlebih dahulu.

![chroot-4]({{ site.baseurl }}/images/chroot-4.png)

Karena disini partisi C:\ dalam windows berada dalam /dev/sda4 , maka kita lakukan mounting langsung, namun perlu diketahui partisi C:\ tidak selalu di /dev/sda4.

```bash
# mount /dev/sda4 /mnt
```
> Namun jika ternyata outputnya menolak untuk di mount dan hanya memberi akses read-only, itu berarti OS windows nya belum benar-benar ter-shutdown, untuk melakukanya hidupkan kembali sistem windows nya, lalu tekan tahan shift sambil menekan tombol shutdown. dan juga jika masih read-only, bisa dengan perintah `$ sudo mount -t ntfs-3g -o remove_hiberfile /dev/sdX /mnt`

Setelah di-mounting, langsung masuk ke direktori /mnt/Windows/System32/config
```bash
cd /mnt/Windows/System32/config
```
Didalam direktori config terdapat file yang bernama SAM(Security Account Manager), itu adalah file konfigurasi database user di windows, untuk me reset password nya bisa lakukan dengan tool chntpw. 

Cek user yang ada di sistem windows:

![chroot-5]({{ site.baseurl }}/images/chroot-5.png)

Untuk melakukan reset :

Pilih nomor 1 untuk meng-edit.
![chroot-6]({{ site.baseurl }}/images/chroot-6.png)

Setelah itu isi nomor user yang akan diganti


![chroot-7]({{ site.baseurl }}/images/chroot-7.png)

Lalu isi 1 untuk mengkosongkan password dari user yang dipilih.

![chroot-8]({{ site.baseurl }}/images/chroot-8.png)

Setelah itu langsung saja pencet q lalu q lagi,lalu y untuk konfirmasi perubahan.

![chroot-9]({{ site.baseurl }}/images/chroot-9.png)

Sekian, Begitulah cara untuk melakukan reset password pada OS windows dan linux ;D

saus:

[opensource.com](https://opensource.com/article/18/3/how-reset-windows-password-linux)

[archlinux.org](https://wiki.archlinux.org/title/chroot)

[archlinux.org - ntfs mounting](https://bbs.archlinux.org/viewtopic.php?id=226958)



