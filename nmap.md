# Nmap
<br>

- [Introduction to Nmap](#introduction-to-nmap)
    - [Use Cases](#use-cases)
    - [Nmap Architecture](#nmap-architecture)
    - [Syntax](#syntax)
    - [Scan Techniques](#scan-techniques)
- [Host Discovery](#host-discovery)
    - [Scan Network Range](#scan-network-range)

<br>
<br>
<br>

<img src="img/nmap.png">

<br>

**Enumeration** is collecting as much information as possible.<br>
The more information we have, the easier it will be for us to find `vectors of attack`. 

<br>
<br>

## Introduction to Nmap

### Use Cases

- Audit the security aspects of networks
- Simulate penetration tests
- Check firewall and IDS settings and configurations
- Types of possible connections
- Network mapping
- Response analysis
- Identify open ports
- Vulnerability assessment as well.
<br>

### Nmap Architecture

- Host discovery
- Port scanning
- Service enumeration and detection
- OS detection
- Scriptable interaction with the target service (Nmap Scripting Engine)
<br>

### Syntax

```
sudo nmap <scan types> <options> <target>
```
<br>

### Scan Techniques

```
Dareck7@htb[/htb]$ nmap

<SNIP>
SCAN TECHNIQUES:
  -sS/sT/sA/sW/sM: TCP SYN/Connect()/ACK/Window/Maimon scans
  -sU: UDP Scan
  -sN/sF/sX: TCP Null, FIN, and Xmas scans
  --scanflags <flags>: Customize TCP scan flags
  -sI <zombie host[:probeport]>: Idle scan
  -sY/sZ: SCTP INIT/COOKIE-ECHO scans
  -sO: IP protocol scan
  -b <FTP relay host>: FTP bounce scan
<SNIP>
```

#### TCP-SYN scan

The TCP-SYN scan (`-sS`) is one of the default settings unless we have defined otherwise. The TCP-SYN scan sends one packet with the `SYN flag` and, therefore, never completes the `three-way handshake`, which results in not establishing a full TCP connection to the scanned port.

- If our target sends an `SYN-ACK` flagged packet back to the scanned port, Nmap detects that the port is `open`.
- If the packet receives an `RST` flag, it is an indicator that the port is `closed`.
- If Nmap does not receive a packet back, it will display it as `filtered`. Depending on the firewall configuration, certain packets may be dropped or ignored by the firewall.

<br>
<br>
<br>

## Host Discovery

The most effective host discovery method is to use `ICMP echo requests`, which we will look into.

### Scan Network Range
```
sudo nmap 10.129.2.0/24 -sn -oA tnet | grep for | cut -d" " -f5

10.129.2.4
10.129.2.10
10.129.2.11
10.129.2.18
10.129.2.19
10.129.2.20
10.129.2.28
```
This scanning method works only if the firewalls of the hosts allow it.

Tester le -oA tnet
Ajout détails options -sn, -oA, network range
Retirer la sortie ?
