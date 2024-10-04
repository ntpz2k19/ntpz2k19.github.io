---
layout: post
title: "Cisco Topology"
---

Berikut topologi singkat dalam perangkat cisco untuk implementasi user management, vlan, ssh, dan switching.
![cisco-vlan-1]({{ site.baseurl }}/images/cisco-vlan1.png)

# Router side

## User management
Untuk user management di router maupun switch memiliki perintah yang sama.
```
R1# configure terminal
R1(config)# hostname R1 <-- edit hostname sesuai selera
R1(config)# username hilmi privilege 7 password 0 njirmomen
R1(config)# enable password 0 woilahcik
```

## IP configuration
Struktur IP dalam router:
**Interface** | **IP** | **type**
--- | ---| ---
FastEthernet 1/1|192.168.10.60/24|Out-Interface
FastEthernet 0/0|"-"|In-Interface
FastEthernet 0/0.10|10.10.10.1/24|Vlan10
FastEthernet 0/0.100|172.16.10.1/24|Vlan100(DHCP)
FastEthernet 0/0.200|160.10.1.1/24|Vlan200

Berikut cara mengimplementasikan:
```
<--Masuk mode config-->
R1#configure terminal
<--Konfigurasi interface uplink-->
R1(config)#interface fastethernet 1/1
R1(config-if)#no shutdown
R1(config-if)#ip addr 192.168.10.60 255.255.255.0
R1(config-if)#desc UPLINK
R1(config-if)#exit

<--Konfigurasi interface jalur lokal-->
R1(config)#interface FastEthernet 0/0
R1(config-if)#no shutdown
R1(config-if)#desc MAIN
R1(config-if)#exit

<--Konfigurasi vlan interface 10-->
R1(config)#int fa0/0.10
R1(config-subif)#no shutdown
R1(config-subif)#encapsulation dot1q 10
R1(config-subif)#desc MGMT
R1(config-subif)#ip addr 10.10.10.1 255.255.255.0

<--Konfigurasi vlan interface 100-->
R1(config)#int fa0/0.100
R1(config-subif)#no shutdown
R1(config-subif)#encapsulation dot1q 100
R1(config-subif)#desc WIFI
R1(config-subif)#ip addr 172.16.10.1 255.255.255.0
R1(config-subif)#exit

<--Konfigurasi vlan interface 200-->
R1(config)#int fa0/0.200
R1(config-subif)#no shutdown
R1(config-subif)#encapsulation dot1q 200
R1(config-subif)#desc CLIENT
R1(config-subif)#ip addr 160.10.1.1 255.255.255.0
R1(config-subif)#exit

<--Tambahkan routing "kemanapun"-->
R1(config)#ip route 0.0.0.0 192.168.10.1
```
Untuk melihat konfigurasi IP bisa menggunakan perintah `show ip int br` atau `show run | section ip`, dan untuk melihat routing table bisa dengan perintah `show ip route`.

## DNS server

Sinkronisasikan dengan DNS server agar bisa menerjemahkan domain name:
```
R# configure terminal
R(config)# ip name-server 8.8.8.8  
R(config)# ip name-server 8.8.4.4
R(config)# ip name-server 1.1.1.1 
<-- apply dns -->
R(config)# ip domain-lookup 
```
## DHCP pool & server
Berikut perintah untuk membuat dhcp pool:
```
R# conf t
R(config)# ip dhcp pool WIFI
R(config)# network 172.16.10.0 255.255.255.0
R(config)# default-router 172.16.10.1
R(config)# dns-server 1.1.1.1 9.9.9.9
```
## Konfigurasi NAT
Untuk konfigurasi NAT tentukan interface out dan in. interface uplink akan menjadi out interface dan interface lainya menjadi inside.
```
R1#configure terminal
R1(config)#int fa1/1
R1(config-if)#ip nat outside
R1(config-if)#exit
R1(config)#int fa0/0
R1(config-if)#ip nat inside
R1(config-if)#exit
R1(config)#int fa0/0.10
R1(config-if)#ip nat inside
R1(config-if)#exit
R1(config)#int fa0/0.100
R1(config-if)#ip nat inside
R1(config-if)#exit
R1(config)#int fa0/0.200
R1(config-if)#ip nat inside
R1(config-if)#exit
```
Lalu buat access list
```
R1#configure terminal
R1(config)#ip access-list standard NAT <-- Nama NAT bisa diubah sesuai selera
R1(config-std-nacl)#permit 10.10.10.0 0.0.0.255
R1(config-std-nacl)#permit 160.10.1.0 0.0.0.255
R1(config-std-nacl)#permit 172.16.10.0 0.0.0.255
R1(config-std-nacl)#exit
```
Setelah membuat access-list selanjutnya konfigurasikan NAT.
```
R(config)# ip nat inside source list NAT int fa1/1 overload
```
## Konfigurasi SSH
SSH memungkinkan user untuk me remote jarak jauh secara ter-enkripsi, karena itu banyak network engineer lebih memilih SSH daripada telnet.

Pertama ubah versi ssh dan generate rsa untuk ssh:
```
R1(config)#crypto key generate rsa general-keys label hilmi
[2048] <--input bytesize 2048
R1(config)#ip ssh version 2
```
Lalu konfigurasi virtual terminal:
```
R1(config)#line vty 0 4
R1(config-line)#login local
R1(config-line)#transport input all
```
Untuk di sisi client pastikan cipher dan algoritma-nya sesuai, agar lebih mudah edit file di `~/.ssh/config`.
```bash
host hilmi 192.168.10.60 #ubah hilmi sesuai selera
  Hostname 192.168.10.60
  User hilmi
  HostkeyAlgorithms ssh-rsa #ssh-dss,
  KexAlgorithms +diffie-hellman-group14-sha1,diffie-hellman-group1-sha1
  Ciphers aes128-cbc,3des-cbc,aes192-cbc,aes256-cbc,aes128-ctr,aes192-ctr,aes256-ctr
```
Jadi setiap saat untuk melakukan remote cukup ketik perintah `ssh hilmi`.
# Switch1 side
Di sisi router tidak banyak yang di konfigurasi, cukup user, ssh, vlan, dan trunk.
## User management
Konfigurasi user seperti halnya router:
```
switch1# configure terminal
switch1(config)# hostname switch1 <-- edit hostname sesuai selera
switch1(config)# username hilmi privilege 7 password 0 switch1
switch1(config)# enable password 0 woilahcik
```

# VLAN management
Berikut untuk mengkonfigurasi vlan:
```
switch1(config)#vlan 10
switch1(config-vlan)#name MGMT
switch1(config-vlan)#exit
switch1(config)#vlan 100
switch1(config-vlan)#name WIFI
switch1(config-vlan)#exit
switch1(config)#vlan 200
switch1(config-vlan)#name CLIENT
switch1(config-vlan)#exit
```
Beri IP pada vlan 10 agar switch dapat di remote dengan ssh:
```
switch1(config)#ip int vlan 10
switch1(config-if)#no shutdown
switch1(config-if)#desc MGMT
switch1(config-if)#ip addr 10.10.10.2 255.255.255.0
switch1(config-if)#exit
```
## Trunking interface
Interface dikonfigurasi mode trunk untuk mengangkut semua vlan dari sumber ke sumber, seperti halnya batang pohon (trunk=batang).
```
switch1(config)#int gi2/3
switch1(config-if)#no shutdown
switch1(config-if)#desc TO-R1
switch1(config-if)#switchport trunk encapsulation dot1q
switch1(config-if)#switchport trunk allowed vlan 10-200
switch1(config-if)#exit
switch1(config)#int gi2/2
switch1(config-if)#no shutdown
switch1(config-if)#desc TO-SW2
switch1(config-if)#switchport trunk encapsulation dot1q
switch1(config-if)#switchport trunk allowed vlan 10-200
switch1(config-if)#exit
```
## VLAN access interface
Setelah mengkonfigurasi trunk interface bisa di tentukan mengakses vlan yang di inginkan.
```
switch1#conf t
switch1(config)#int gi0/1
switch1(config-if)#switchport mode access
switch1(config-if)#switchport access vlan 200
switch1(config-if)#exit
switch1(config)#int gi0/2
switch1(config-if)#switchport mode access
switch1(config-if)#switchport access vlan 100
switch1(config-if)#exit
```
## Konfigurasi SSH
SSH memungkinkan user untuk me remote jarak jauh secara ter-enkripsi.
```
R1(config)#crypto key generate rsa general-keys label hilmi
[2048] <--input bytesize 2048
R1(config)#ip ssh version 2
```
Lalu konfigurasi virtual terminal:
```
R1(config)#line vty 0 4
R1(config-line)#login local
R1(config-line)#transport input all
```
Saus:
## I MADE IT UP



