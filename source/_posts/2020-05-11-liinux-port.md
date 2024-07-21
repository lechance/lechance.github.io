---
title: Linux Ports
date: 2020-05-11
categories: Linux
tags:
- linux
- port
---

##### Ports

Query opened ports
- netstat 
```
netstat -tunlp
```
- ss
```
/**
t-tcp 
u-udp 
n-numeric(don't resolve service name) 
l-listening(listening sockets) 
p-processes(show process)
*/
ss -tunlp
```
- lsof
```
/**
n-inhibits the conversion of network numbers to hostname for network files
P-inhibits the conversion of port numbers to port names for network files
i-selects the listing of files any of whose Internet address matches the address specified in ++i++
An Internet address is specifed in the form(Items in square brackets are optional)
[46][protocol][@hostname|hostaddr][:service|port]
s-display file size[or state]
form:   -s([)protocol:port)
*/

lsof -nP -iTCP -sTCP:LISTEN
```
- nc
```
nc -zv (127.0.0.1) 80[-90]
```