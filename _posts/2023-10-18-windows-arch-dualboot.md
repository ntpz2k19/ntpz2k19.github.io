---
layout: post
title: "Windows-Arch dualboot"
---

Ada beberapa alasan mengapa saya masih menggunakan Windows meskipun Windows sudah bukan OS utama saya, dalam keadaan tertentu, kadang saya membutuhkan Windows untuk mengoperasikan beberapa aplikasi yang mungkin karena wine masih belum compatible seperti AutoCAD, lalu juga memang Ayah saya hanya bisa menggunakan Windows.

Pertama, install Windows dengan mode efi, ini akan memudahkan dikarenakan akan lebih sulit melakukan custom install di Windows ketimbang di linux. berikut beberapa sumber yang menyediakan cara untuk instalasi Windows dengan mode efi:

[linux-woeusb](https://superuser.com/questions/1017756/create-a-bootable-windows-10-usb-drive-uefi-from-linux)

[windows-rufus](https://www.windowscentral.com/how-create-windows-10-usb-bootable-media-uefi-support)

Setelah instalasi, Terdapat beberapa partisi, cari partisi efi, karena partisi tersebut digunakan termasuk juga untuk menyimpan bootloader.

![efi-1]({{ site.baseurl }}/images/efi.png)

Disini dijelaskan bahwa partisi efi ada di nomor 2, jika dibaca di sistem linux kira-kira seperti /dev/sda2.

Lalu buat archlinux bootable, bisa menggunakan balena-etcher atau dd.

setelah mengetahui partisi efi kita langsung lakukan instalasi linux. Jika ada free space cukup tinggal di format, namun jika perlu partitioning bisa gunakan beberapa disk tool seperti fdisk, cfdisk, atau yang graphical seperti gparted, dan dikarenakan kita sudah menginstall Windows dengan boot mode uefi, kita tidak perlu melakukan setup efi partition. Lalu kita cukup memerlukan 1 partisi untuk root, untuk tambahan partisi hal itu opsional.

Setelah partiitoning, lakukan format pada partisi, pastikan lokasi partisi sudah benar, karena hal ini sangat fatal jika keliru. Untuk kasus ini partisi yang akan kita format adalah /dev/sda6.
```bash
sudo mkfs.ext4 -L root /dev/sda6
```

Sebelum melakukan instalasi, lakukan koneksikan ke wifi archlinux menggunakan iwd, berikut cara melakukan koneksi:
```
systemctl start iwd
iwctl
[iwd] device list #daftar wireless card, misal wlp2s0
[iwd] station wlp2s0 scan #melakukan scanning
[iwd] station wlp2s0 get-networks #menghasilkan hasil scan
[iwd] station wlan0 connect nama_wifi
```

Setelah itu kita cukup lakukan instalasi Archlinux pada umumnya:
```bash
#mounting dari usb bootable:
mount /dev/sda6 /mnt
#buat direktori untuk partisi efi:
mkdir /mnt/boot/efi
#mount partisi efi:
mount /dev/sda2 /mnt/boot/efi
#instalasi root:
pacstrap -K  /mnt base base-devel linux linux-firmware
#generate fstab
genfstab -U /mnt >> /mnt/etc/fstab
#chroot ke partisi
arch-chroot /mnt
#set time zone, untuk regional sesuaikan dengan lokasi
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
timedatectl set-ntp true
#set lokalisasi:
pacman -S neovim #install text editor
nvim /etc/locale.gen
# uncomment bagian en_US.UTF-8 UTF-8
locale-gen
#set hostname 
echo "nama_hostname" > /etc/hostname
#setup root password
passwd
#setup bootloader
pacman -S grub efibootmgr os-prober
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=grub
grub-mkconfig -o /boot/grub/grub.cfg
```

setelah instalasi Archlinux, lakukan post instalation arch seperti setup desktop environment atau window manager. Setelah itu lakukan konfigurasi grub agar Windows terbaca di grub, edit di /etc/grub.d/40_custom :
```
#!/bin/sh
exec tail -n +3 $0
# This file provides an easy way to add custom menu entries.  Simply type the
# menu entries you want to add after this comment.  Be careful not to change
# the 'exec tail' line above.
menuentry 'Windows' {
   search --fs-uuid --set=root D244-AB68
   chainloader /EFI/Microsoft/Boot/bootmgfw.efi
```
Lalu update konfigurasi grub
```
grub-mkconfig -o /boot/grub/grub.cfg
```
> Disarankan jika akan booting kedalam Windows gunakan yang dari opsi 40_custom, bukan Windows boot manager karena ada kemungkinan konflik bootloader dan grub akan bermasalah.

Saus:

[archlinux.org - installation](https://wiki.artixlinux.org/Main/Installation)

[archlinux.org -bootloader](https://wiki.archlinux.org/title/GRUB)

