# Common Pitfalls

<br>

- [VPN Troubleshooting](#vpn-troubleshooting)

    - [Still Connected to VPN](#still-connected-to-vpn)
    - [Getting VPN Address](#getting-vpn-address)
    - [Checking Routing Table](#checking-routing-table)
    - [Pinging Gateway](#pinging-gateway)
    - [How to delete a virtual interface](#how-to-delete-a-virtual-interface)

- [Changing SSH Key and Password](#changing-ssh-key-and-password)

<br>

## VPN Troubleshooting
<br>

### Still Connected to VPN

The easiest method of checking if we have successfully connected to the VPN network is by checking whether we have `Initialization Sequence Completed` at the end of our VPN connection messages:
```diff
Dareck7@htb[/htb]$ sudo openvpn ./htb.ovpn

...SNIP...

+ Initialization Sequence Completed
```
<br>

### Getting VPN Address

Another way of checking whether we are connected to the VPN network is by checking our VPN `tun0` address, which we can find with the following command:
```
Dareck7@htb[/htb]$ ip -4 a show tun0

6: tun0: <POINTOPOINT,MULTICAST,NOARP,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UNKNOWN group default qlen 500
    inet 10.10.10.1/23 scope global tun0
       valid_lft forever preferred_lft forever
 ```
 As long we get our IP back, then we should be connected to the VPN network.
 
 <br>
 
 ### Checking Routing Table

Another way to check for connectivity is to use the command sudo `netstat -rn` to view our routing table:
```diff
  Dareck7@htb[/htb]$ sudo netstat -rn

  [sudo] password for user: 

  Kernel IP routing table
  Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
  0.0.0.0         192.168.195.2   0.0.0.0         UG        0 0          0 eth0
+ 10.10.14.0      0.0.0.0         255.255.254.0   U         0 0          0 tun0
+ 10.129.0.0      10.10.14.1      255.255.0.0     UG        0 0          0 tun0
  192.168.1.0     0.0.0.0         255.255.255.0   U         0 0          0 eth0
```
<br>

### Pinging Gateway

From here, we can see that we are connected to the `10.10.14.0/23` network on the `tun0` adapter and have access to the `10.129.0.0/16` network and can ping the gateway `10.10.14.1` to confirm access.
```diff
  Dareck7@htb[/htb]$ ping -c4 10.10.14.1
  PING 10.10.14.1 (10.10.14.1) 56(84) bytes of data.
  64 bytes from 10.10.14.1: icmp_seq=1 ttl=64 time=111 ms
  64 bytes from 10.10.14.1: icmp_seq=2 ttl=64 time=111 ms
  64 bytes from 10.10.14.1: icmp_seq=3 ttl=64 time=111 ms
  64 bytes from 10.10.14.1: icmp_seq=4 ttl=64 time=111 ms
  
  --- 10.10.14.1 ping statistics ---
+ 4 packets transmitted, 4 received, 0% packet loss, time 3012ms
  rtt min/avg/max/mdev = 110.574/110.793/111.056/0.174 ms
```
Finally, we can either attack an assigned target host on the `10.129.0.0/16` network or begin enumeration for live hosts.

<br>

### How to delete a virtual interface

You can use `sudo ip link delete` to remove the interface:
```
Dareck7@htb[/htb]$ sudo ip link delete tun1
```

<br>
<br>

## Changing SSH Key and Password
<br>

By default, SSH keys are stored in the `.ssh` folder within our home folder (for example,` /home/htb-student/.ssh`).
In case we start facing some issues with connecting to SSH servers, we may want to renew or change our SSH key and password to make sure they are not causing any issues. We can do this with the `ssh-keygen command`, as follows (we can encrypt our SSH key with a password when prompted or keep it empty if we do not want to use a password):

```
Dareck7@htb[/htb]$ ssh-keygen
```
