---
title: "Iperf2 cheatsheet"
layout: post
---

Iperf2 adalah salah satu tool yang digunakan untuk bandwitdh testing, iperf2 memiliki 2 mode yaitu server dan client. berikut cheatsheet untuk iperf2:

Basic usage

```
#server
>iperf -s
#client
>iperf -c ip_server
```
Dengan spesifik port
```
#server
>iperf -s -p 6400
#client
>iperf -c ip_server -p 6400

```
Dengan protokol udp
```
#server
>iperf -s -u -p 6400
#client
>iperf -c ip_server -u -p 6400
```
dua arah (client)
```
#dua arah sekaligus
>iperf -c ip_server -d
#dua arah satu persatu
>iperf -c ip_server -r
```
Saus:
[jamescoyle.net](https://www.jamescoyle.net/cheat-sheets/581-iperf-cheat-sheet)

