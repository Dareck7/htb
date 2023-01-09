## Pentest Cheatsheet

### Penetration Testing Process

<img src="img/penetration_testing_process.md">

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
