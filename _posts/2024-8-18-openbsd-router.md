---
title: basic openbsd router
layout: post
---

OpenBSD adalah salah satu OS turunan BSD yg difokuskan di security, dan cukup bisa diandalkan sebagai router sekaligus firewall.

Untuk melakukan konfigurasi router pada OpenBSD ada beberapa cara, seperti pada router umumnya, terdapat konfigurasi IP, DHCP, DNS,sedikit SSH dan firewall.


![openbsd-1]({{ site.baseurl }}/images/basic-openbsd.png)

Berikut konfigurasi yang akan digunakan: 

**Interface** | **IP** | **type**
--- | ---| ---
em0 | 192.168.10.77(atau DHCP) | out-interface 
em1 | 192.168.20.1 | in-interface(NAT)


## Kofigurasi IP
Sebelum melakukan konfigurasi IP, diperlukan terlebih dahulu untuk meng-enable ip forwarding agar packet bisa diteruskan, ketik perintah `sysctl net.inet.ip.forwarding=1`, perintah tersebut akan mengedit pada file `/etc/sysctl.conf`.


Untuk melakukan konfigurasi IP pada interface dilakukan dengan meng-edit file /etc/hostname.namainterface .

berikut konfigurasi IP untuk interface em0: /etc/hostname.em0 
```
inet 192.168.10.77
# atau jika menggunakan dhcp
# inet autoconf
```
Lakukan konfigurasi juga pada interface em1: /etc/hostname.em1 
```
inet 192.168.20.1 255.255.255.0 192.168.20.255
#     ^IP           ^Subnet       ^Broadcast
```

lalu edit default gateway pada file /etc/mygate :
```
192.168.10.1
```

## Konfigurasi DNS 
Untuk konfigurasi DNS resolver cukup dengan edit file /etc/resolv.conf :
```
nameserver 192.168.10.1 
nameserver 9.9.9.9
```
## Konfigurasi DHCP 
Konfigurasi DHCP di openbsd cukup di handle di dalam file /etc/dhcpd.conf :
```
subnet 192.168.20.0 netmask 255.255.255.0 {
        option routers 192.168.20.1;
        option domain-name-servers 192.168.10.1, 9.9.9.9;
        range 192.168.20.4 192.168.20.254;
}
```
Lalu tentukan em1 sebagai sumber DHCP nya dengan perintah `rcctl set dhcpd flags em1`, perintah ini akan menambahkan dhcp flag di dalam file `/etc/rc.conf.local` . 

Selanjutnya alankan dan enable service DHCP dengan perintah `rcctl enable dhcpd && rcctl start dhcpd`.

Untuk melakukan debugging dhcpd OpenBSD gunakan perintah `/usr/sbin/dhcpd -d -f`.

## SSH
Untuk konfigurasi ssh untuk saat ini tidak perlu terlalu dalam, hanya mengganti port saja pada file `/etc/ssh/sshd_config`.
```
#cari sesi Port
Port 22000
```
Lalu jalankan ulang service sshd dengan perintah `rcctl restart sshd`.

## Firewall 
OpenBSD secara default menggunakan backend firewall terbaik yg bernama PF (Packet filter), Lakukan konfigurasi sederhana dengan edit pada file `/etc/pf.conf`.

```
#edit variabel statis
remote = "22000"
lan1 = "em1"

#blokir semua koneksi
block all

#packet yg di block akan di drop secara diam, lalu egress adalah out-interface alias em0
set block-policy drop
set loginterface egress
set skip on lo0

#konfigurasi nat
match in all scrub (no-df random-id max-mss 1440)
match out on egress inet from !(egress:network) to any nat-to (egress:0)

pass out quick inet
pass in on $lan1 inet

#buka port ssh, 
pass in proto tcp to port { $remote }
#pass out proto { tcp udp } to port { 22 53 80 123 443 }

#buka outgoing ping 
pass out inet proto icmp icmp-type { echoreq }
#pass in inet proto icmp icmp-type { echoreq }
```
Untuk meng-enable PF jalankan perintah `pfctl -f /etc/pf.conf`,lalu untuk men-disable `pfctl -d`.

### saus:

[openbsd.org](https://www.openbsd.org/faq/pf/example1.html)

[digitalocean.com](https://www.digitalocean.com/community/tutorials/how-to-configure-packet-filter-pf-on-freebsd-12-1)


