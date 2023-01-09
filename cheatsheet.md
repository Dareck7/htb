## Pentest Cheatsheet

<img src="img/htb2.png">

- [Penetration Testing Process](#penetration-testing-process)
- [Folder Structure](#folder-structure)
- [VPN Troubleshooting](#vpn-troubleshooting)
- [Nmap](#nmap)
  - [Host Discovery](#host-discovery)

<br>

### Penetration Testing Process

<img src="img/penetration_testing_process.png">

### Folder Structure

<img src="img/folder_structure.png">

### VPN Troubleshooting
```diff
# Still Connected to VPN 
$ sudo openvpn ./htb.ovpn  

...SNIP...  

+ Initialization Sequence Completed  


# Getting VPN Address 
$ ip -4 a show tun0  

# Checking Routing Table 
$ netstat -rn

# Pinging Gateway 
$ ping -c4 $ip  

# Deleting a virtual interface 
$ sudo ip link delete tun1
```

### Nmap

#### Host Discovery
```console
sudo nmap IPRANGE -sn -n -oA nmap | grep for | cut -d" " -f5 > hosts.lst
```
