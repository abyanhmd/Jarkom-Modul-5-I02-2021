# Jarkom-Modul-5-I02-2021

# Jarkom-Modul-3-I02-2021
## Group IUP 2
- Gede Yoga Arisudana (05111942000009)
- Abyan Ahmad (05111942000013)
- Zulfiqar Rahman Aji (05111942000019)


### Topology
![Topology](/Image/Topology.png)

### Subnet
We used VLSM Subnetting for this case.

![VLSM Subnet](/Image/VLSM_Subnet.png)

![VLSM Tree](/Image/VLSM_Tree.png)

![VLSM IP](/Image/VLSM_IP.jpg)

**FOOSHA**
```sh
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
hwaddress ether 9a:c7:17:77:74:00

auto eth1
iface eth1 inet static
 address 192.209.7.145
 netmask 255.255.255.252

auto eth2
iface eth2 inet static
 address 192.209.7.149
 netmask 255.255.255.252
```

**WATER7**

```sh
auto eth0
iface eth0 inet static
 address 192.209.7.146
 netmask 255.255.255.252
 gateway 192.209.7.145

auto eth1
iface eth1 inet static
 address 192.209.7.129
 netmask 255.255.255.248

auto eth2
iface eth2 inet static
 address 192.209.7.1
 netmask 255.255.255.128

auto eth3
iface eth3 inet static
 address 192.209.0.1
 netmask 255.255.252.0
```

**GUANHAO**

```sh
auto eth0
iface eth0 inet static
 address 192.209.7.150
 netmask 255.255.255.252
 gateway 192.209.7.149

auto eth1
iface eth1 inet static
 address 192.209.4.1
 netmask 255.255.254.0

auto eth2
iface eth2 inet static
 address 192.209.6.1
 netmask 255.255.255.0

auto eth3
iface eth3 inet static
 address 192.209.7.137
 netmask 255.255.255.248
```

**Doriki**

```sh
auto eth0
iface eth0 inet static
 address 192.209.7.130
 netmask 255.255.255.248
 gateway 192.209.7.129
```

**Jipangu**

```sh
auto eth0
iface eth0 inet static
        address 192.209.7.131
        netmask 255.255.255.248
        gateway 192.209.7.129
```

**Jorge**

```sh
auto eth0
iface eth0 inet static
 address 192.209.7.138
 netmask 255.255.255.248
 gateway 192.209.7.137
```

**Maingate**

```sh
auto eth0
iface eth0 inet static
 address 192.209.7.139
 netmask 255.255.255.248
 gateway 192.209.7.137
```

**Blueno - Cipher - Elena - Fukurou**

```sh
auto eth0
iface eth0 inet dhcp
```

<br>

### Routing
**Water7**
shell
route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.209.7.145

**Gunahao**
```shell
route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.209.7.149
```

**Foosha**
```shell
route add -net 192.209.7.128 netmask 255.255.255.248 gw 192.209.7.146
route add -net 192.209.7.0 netmask 255.255.255.128 gw 192.209.7.146
route add -net 192.209.0.0 netmask 255.255.252.0 gw 192.209.7.146
route add -net 192.209.4.0 netmask 255.255.254.0 gw 192.209.7.150
route add -net 192.209.6.0 netmask 255.255.255.0 gw 192.209.7.150
route add -net 192.209.7.136 netmask 255.255.255.248 gw 192.209.7.150
```

<br>

### DHCP

**Jipangu - DHCP Server**
```shell
# /etc/dhcp/dhcpd.conf

subnet 192.209.7.128 netmask 255.255.255.248 {
 option routers 192.209.7.129;
}
subnet 192.209.7.0 netmask 255.255.255.128 {
    range 192.209.7.2 192.209.7.126;
    option routers 192.209.7.1;
    option broadcast-address 192.209.7.127;
    option domain-name-servers 192.209.7.130;
    default-lease-time 600;
    max-lease-time 7200;
}
subnet 192.209.0.0 netmask 255.255.252.0 {
    range 192.209.0.2 192.209.3.254;
    option routers 192.209.0.1;
    option broadcast-address 192.209.3.255;
    option domain-name-servers 192.209.7.130;
    default-lease-time 600;
    max-lease-time 7200;
}
subnet 192.209.4.0 netmask 255.255.254.0 {
    range 192.209.4.2 192.209.5.254;
    option routers 192.209.4.1;
    option broadcast-address 192.209.5.255;
    option domain-name-servers 192.209.7.130;
    default-lease-time 600;
    max-lease-time 7200;
}
subnet 192.209.6.0 netmask 255.255.255.0 {
    range 192.209.6.2 192.209.6.254;
    option routers 192.209.6.1;
    option broadcast-address 192.209.6.255;
    option domain-name-servers 192.209.7.130;
    default-lease-time 600;
    max-lease-time 7200;
}
```

**Water7 - Foosha - Guanhao - DHCP Relay**

The configuration is only adding Jipangu's IP and each nodes' eth interface in */etc/default/isc-dhcp-relay*.

<br>

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
