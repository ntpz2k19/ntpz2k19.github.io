---
title: Basic Cisco router
layout: post
---

![cisco-1]({{ site.baseurl }}/images/basic-cisco.png)

Cisco merupakan salah satu perangkat networking yang cukup populer, maka untuk melakukan setup routernya diperlukan beberapa langkah, yaitu diantaranya; setup IP, Routing, DHCP, dan NAT.

Berikut konfigurasi yang akan di bentuk:

**Interface** | **IP** | **type**
--- | ---| ---
FastEthernet 0/0 | 192.168.100.2/24(atau DHCP) | out-interface 
FastEthernet 0/1 | 192.168.200.1/24 | in-interface(NAT)

##Setup IP

Untuk melakukan setup IP di router cisco ada 2 cara, yaitu static IP dan dhcp, berikut perintah untuk melakukan setup IP:
```
<--Masuk mode config-->
R# configure terminal 

<--Konfigurasi if-out manual-->
R(config)# int fa0/0 
R(config-if)# ip address 192.168.100.2 255.255.255.0
R(config-if)# no shutdown
R(config-if)# end
R(config)# ip route 0.0.0.0 0.0.0.0 192.168.100.1 --->ip gateway external.
R(config)# ip default gateway 192.168.100.1 
R(config)# ip default network 192.168.100.0

<--Atau konfigurasi if-out dhcp-->
R# configure terminal
R(config)# int fa0/0
R(config-if)# ip address dhcp  
R(config-if)# no shutdown

<--Konfigurasi if-in-->
R# configure terminal
R(config)# interface fa0/1 
R(config-if)# ip address 192.168.200.1 255.255.255.0
R(config-if)# no shutdown
R(config-if)# end 

```
Untuk melihat konfigurasi IP bisa menggunakan perintah `show ip int br` atau `show run | section ip`, dan untuk melihat routing table bisa dengan perintah `show ip route`.

##DNS server

Sinkronisasikan dengan DNS server agar bisa menerjemahkan domain name:
```
R# configure terminal
R(config)# ip name-server 8.8.8.8  
R(config)# ip name-server 8.8.4.4
R(config)# ip name-server 1.1.1.1 
<-- apply dns -->
R(config)# ip domain-lookup 
```

##DHCP pool & server
Berikut perintah untuk membuat dhcp pool:
```
R# conf t
R(config)# ip dhcp pool internal
R(config)# network 192.168.100.0 255.255.255.0
R(config)# default-router 192.168.100.1
R(config)# dns-server 8.8.8.8
```
##Konfigurasi NAT

NAT atau network address translation digunakan agar request dari internal bisa diteruskan oleh external. NAT sendiri diciptakan karena terbatasnya jumlah IPv4. Untuk konfigurasi NAT sebagai berikut:
```
R# configure terminal
<-- set interface fa0/0 sebagai out interface -->
R(config)# interface FastEthernet0/0
R(config-if)# ip nat outside
R(config-if)# exit
<-- set interface fa0/1 sebagai in interface -->
R(config)# int fa0/1
R(config-if)# ip nat inside 
R(config-if)# end
```
Setelah menentukan out interface dan in interface selanjutnya buat nat access list.
```
R# conf t
R(config)# ip access-list standard NAT <-- nama access list, bisa diganti sesuai selera
R(config-std-nacl)# permit 192.168.200.0 0.0.0.255 <---wildcard, lawan dari subnet, subnet yg dipakai 255.255.255.0
R(config-std-nacl)# exit
```
Setelah membuat access-list selanjutnya konfigurasikan NAT.
```
R(config)# ip nat inside source list NAT int fa0/0 overload
```
Setelah semua di konfigurasi bisa dilakukan test ping ip address dan domain name, jika ternyata ada konfigurasi yang keliru bisa dilakukan cek konfigurasi dengan perintah `show run`, atau dengan filter `show run | section ip`. jika dirasa semua sudah berjalan dengan baik maka konfigurasi bisa disimpan agar tidak ter-reset saat reboot dengan perintah `write`.



saus:


[youtube.com](https://www.youtube.com/watch?v=rymxrGyiUp8&pp=ygUJY2lzY28gbmF0)
