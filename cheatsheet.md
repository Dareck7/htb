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

<br>

### Folder Structure

<img src="img/folder_structure.png">

<br>

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

<br>

### Nmap

#### Host Discovery
```console
sudo nmap IPRANGE -sn -n -oA nmap | grep for | cut -d" " -f5 > hosts.lst
```
#### Port Scanning
```console
# Nmap Scan (Quick)
$ nmap -sC -sV 10.10.10.10

# Nmap Scan (Full)
$ nmap -sC -sV -p- 10.10.10.10

# OS Detection
$ nmap -Pn -O 10.10.10.10

# Nmap Scan (UDP Quick)
$ nmap -sU -sV 10.10.10.10

# Agressive Scan (enable OS detection, version detection, script scanning, and traceroute)
$ nmap -p- -vv -A -T4 scanme.nmap.org

# Vulnerabillity Scan (/usr/share/nmap/scripts)
nmap -p<port> --script=vuln 10.10.10.10

# eJPT Scan
$ nmap -p- -T4 -Pn -n -vv -iL ips.txt # doing a blanket scan first
$ nmap -p<ports> -A -Pn -vv 10.10.10.10 # then an aggressive scan after based on what you find
```
