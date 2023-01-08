# HTB Academy

## Folder Structure

<img src="img/folder_structure.png">


## VPN Troubleshooting

### Still Connected to VPN
```diff
Dareck7@htb[/htb]$ sudo openvpn ./htb.ovpn

...SNIP...

+ Initialization Sequence Completed
```

### Getting VPN Address
```console
Dareck7@htb[/htb]$ ip -4 a show tun0

6: tun0: <POINTOPOINT,MULTICAST,NOARP,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UNKNOWN group default qlen 500
    inet 10.10.10.1/23 scope global tun0
       valid_lft forever preferred_lft forever
 ``` 
 ### Checking Routing Table
```console
Dareck7@htb[/htb]$ sudo netstat -rn

[sudo] password for user: 

Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
0.0.0.0         192.168.195.2   0.0.0.0         UG        0 0          0 eth0
10.10.14.0      0.0.0.0         255.255.254.0   U         0 0          0 tun0
10.129.0.0      10.10.14.1      255.255.0.0     UG        0 0          0 tun0
192.168.1.0     0.0.0.0         255.255.255.0   U         0 0          0 eth0
```
### Pinging Gateway
```console
Dareck7@htb[/htb]$ ping -c4 10.10.14.1
PING 10.10.14.1 (10.10.14.1) 56(84) bytes of data.
64 bytes from 10.10.14.1: icmp_seq=1 ttl=64 time=111 ms
64 bytes from 10.10.14.1: icmp_seq=2 ttl=64 time=111 ms
64 bytes from 10.10.14.1: icmp_seq=3 ttl=64 time=111 ms
64 bytes from 10.10.14.1: icmp_seq=4 ttl=64 time=111 ms
  
--- 10.10.14.1 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3012ms
rtt min/avg/max/mdev = 110.574/110.793/111.056/0.174 ms
```
### How to delete a virtual interface
```console
Dareck7@htb[/htb]$ sudo ip link delete tun1
```
