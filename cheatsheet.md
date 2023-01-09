# Pentest Cheatsheet

<img src="img/htb2.png">

- [Penetration Testing Process](#penetration-testing-process)
- [Folder Structure](#folder-structure)
- [VPN Troubleshooting](#vpn-troubleshooting)
- [Reconnaissance](#reconnaissance)
  - [Nmap](#nmap)
    - [Host Discovery](#host-discovery)
    - [General Enumeration](#general-enumeration)
    - [Scanning UDP](#scanning-udp)
    - [nmapAutomator](#automated-nmap-scanning-(my-preference-is-nmapautomator,-never-missed-a-port))
- [Misc](#misc)

<br>

## Penetration Testing Process

<img src="img/penetration_testing_process.png">

<br>

## Folder Structure

<img src="img/folder_structure.png">

<br>

## VPN Troubleshooting
```diff
# Still Connected to VPN 
$ sudo openvpn ./htb.ovpn  

...SNIP...  

+ Initialization Sequence Completed  

# Getting VPN Address  
ip -4 a show tun0 

# Checking Routing Table 
netstat -rn

# Deleting a virtual interface 
sudo ip link delete tun1

# Pinging Gateway 
ping -c4 $ip
```

<br>

## Reconnaissance

### Nmap

#### Host Discovery
> $ nmap IPRANGE -sn -n | grep for | cut -d" " -f5 > hosts.lst

#### General Enumeration
```console
# Doing a blanket scan first
nmap -Pn -n -vvv -p- -T4 -oN nmap/allports $ip

# Then an aggressive scan after based on what you find
nmap -Pn -n -vvv -p<ports> -A -oN nmap/targeted $ip
```
#### UDP Scanning
```console
nmap -Pn -n -vvv -sU -oN nmap/udp $ip
```
#### Automated nmap scanning (my preference is nmapAutomator, never missed a port)
```console
# It is recommended to scan ONE IP at a time
# Do NOT overload the network
# All scans, consecutively: Quick, Targeted, UDP, All ports, Vuln scan, CVE scan, Gobuster, Nikto
nmapAutomator $ip All
```

### Misc
