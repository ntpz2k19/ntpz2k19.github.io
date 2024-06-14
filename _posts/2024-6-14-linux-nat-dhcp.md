---
title: "Linux NAT DHCP"
layout: post
---

Linux adalah salah satu OS yang cukup modular, bisa digunakan sebagai desktop, server, router, bahkan embedded system. Berikut cara untuk menjadikan linux sebagai router dengan fitur NAT dan DHCP.

**Interface** | **IP** | **type**
--- | ---| ---
eth0| 10.10.10.9(static) atau DHCP | out-interface 
eth1| 20.20.20.1/24 | in-interface(NAT)

## Setup IP
Untuk setup IP interface bisa menyesuaikan, atau bisa dengan artikel [Berikut](https://ntpz2k19.github.io/2023/04/28/setup-static-ip.html).


## Setup NAT
sudo iptables -A FORWARD -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
sudo  iptables -A FORWARD -i eth1 -o eth0 -j ACCEPT
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE


## Setup DHCP server
untuk dhcp server di distro ubuntu/debian bisa gunakan isc-dhcp-server, atau di arch linux bisa dengan dhcpd. berikut konfigurasi file:
```
## setup dhcp server
##/etc/dhcpd4.conf
default-lease-time 
max-lease-time 7200;600;



option domain-name-servers 8.8.8.8, 8.8.4.4;
option subnet-mask 255.255.255.0;
option routers 10.10.10.1;
subnet 10.10.10.0 netmask 255.255.255.0 {
  range 10.10.10.2 10.10.10.20;
}
```
saus:

[servervault.com](https://serverfault.com/questions/1120791/iptables-how-best-to-dnat-snat-with-a-dynamic-dhcp-address)

[wiki.archlinux.org](https://wiki.archlinux.org/title/dhcpd#Listening_on_only_one_interface)
