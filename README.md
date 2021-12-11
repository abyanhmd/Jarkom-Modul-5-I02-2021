# Jarkom-Modul-5-I02-2021

## Problem 4
Access from Blueno and Cipher subnets to Doriki is only allowed from 07.00 - 15.00 from Monday to Thursday.

```shell
# iptables for Doriki

# Blueno
iptables -A INPUT -s 192.209.7.0/25 -p tcp -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
iptables -A INPUT -s 192.209.7.0/25 -j REJECT

# Cipher
iptables -A INPUT -s 192.209.0.0/25 -p tcp -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
iptables -A INPUT -s 192.209.0.0/25 -j REJECT
```

![4 Test](/Image/4_Test.png)

## Problem 5
Access from Elena and Fukuro subnets to Doriki is only allowed from 15.01 - 06.59 every day.

```shell
# iptables for Doriki

# Elena
iptables -A INPUT -s 192.209.4.0/23 -m time --timestart 15:01 --timestop 23:59 -j ACCEPT
iptables -A INPUT -s 192.209.4.0/23 -m time --timestart 00:00 --timestop 06:59 -j ACCEPT
iptables -A INPUT -s 192.209.4.0/23 -j REJECT

#Fukurou
iptables -A INPUT -s 192.209.6.0/24 -m time --timestart 15:01 --timestop 23:59 -j ACCEPT
iptables -A INPUT -s 192.209.6.0/24 -m time --timestart 00:00 --timestop 06:59 -j ACCEPT
iptables -A INPUT -s 192.209.6.0/24 -j REJECT
```

![5 Test](/Image/5_Test.png)

## Problem 6

Since we have 2 Web Servers, Luffy wants Guanhao to be set up so that every request from a client that accesses the DNS Server will be distributed alternately to Jorge and Maingate

```shell
iptables -A PREROUTING -t nat -p tcp -d 192.209.7.130 --dport 80 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 192.209.7.138
iptables -A PREROUTING -t nat -p tcp -d 192.209.7.130 --dport 80 -j DNAT --to-destination 192.209.7.139
iptables -A POSTROUTING -t nat -p tcp -d 192.209.7.138 --dport 80 -j SNAT --to-source 192.209.7.130
iptables -A POSTROUTING -t nat -p tcp -d 192.209.7.139 --dport 80 -j SNAT --to-source 192.209.7.130
```

- PREROUTING in here is to route from DNS Server's IP to Web Server's IP with specific properties to distribute the packet.

- POSTROUTING in here is to route from Web Server's IP to DNS Server's IP so the DNS Server's can retrieve the packet.

![6 Test](/Image/6_Test.png)
