# Nmap
<br>

- [Introduction to Nmap](#introduction-to-nmap)
    - [Use Cases](#use-cases)
    - [Nmap Architecture](#nmap-architecture)
    - [Syntax](#syntax)
    - [Scan Techniques](#scan-techniques)
- [Host Discovery](#host-discovery)
    - [Scan Network Range](#scan-network-range)
    - [Scan IP List](#scan-ip-list)
    - [Scan Multiple IPs](#scan-multiple-ips)
    - [Scan Single IP](#scan-single-ip)

<br>

**Enumeration** is collecting as much information as possible.<br>
The more information we have, the easier it will be for us to find `vectors of attack`. 

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

```console
sudo nmap <scan types> <options> <target>
```

<br>

### Scan Techniques

```console
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
```console
sudo nmap 10.129.2.0/24 -sn -oA tnet | grep for | cut -d" " -f5
```
- `10.129.2.0/24` 	Target network range.
- `-sn` 	Disables port scanning.
- `-oA tnet` 	Stores the results in all formats starting with the name 'tnet'.

This scanning method works only if the firewalls of the hosts allow it.

<br>

### Scan IP List

During an internal penetration test, it is not uncommon for us to be provided with an IP list with the hosts we need to test.

```console
Dareck7@htb[/htb]$ cat hosts.lst

10.129.2.4
10.129.2.10
10.129.2.11
10.129.2.18
10.129.2.19
10.129.2.20
10.129.2.28
```
If we use the same scanning technique on the predefined list, the command will look like this:
```console
sudo nmap -sn -oA tnet -iL hosts.lst | grep for | cut -d" " -f5
```
- `-sn` 	Disables port scanning.
- `-oA tnet` 	Stores the results in all formats starting with the name 'tnet'.
- `-iL` 	Performs defined scans against targets in provided 'hosts.lst' list.

In this example, we see that only 3 of 7 hosts are active. Remember, this may mean that the other hosts ignore the default ICMP echo requests because of their firewall configurations. Since Nmap does not receive a response, it marks those hosts as `inactive`.

<br>

### Scan Multiple IPs

It can also happen that we only need to scan a small part of a network:

```console
sudo nmap -sn -oA tnet 10.129.2.18 10.129.2.19 10.129.2.20 | grep for | cut -d" " -f5
```

If these IP addresses are next to each other, we can also define the range in the respective octet:

```console
sudo nmap -sn -oA tnet 10.129.2.18-20 | grep for | cut -d" " -f5
```

<br>

### Scan Single IP

Before we scan a single host for open ports and its services, we first have to determine if it is alive or not.

```console
sudo nmap 10.129.2.18 -sn -oA host -PE --packet-trace
```
- `10.129.2.18` 	Performs defined scans against the target.
- `-sn` 	Disables port scanning.
- `-oA host` 	Stores the results in all formats starting with the name 'host'
- `-PE` 	Performs the ping scan by using 'ICMP Echo requests' against the target.
- `--packet-trace` 	Shows all packets sent and received

If we disable port scan (`-sn`), Nmap automatically ping scan with `ICMP Echo Requests` (`-PE`). Once such a request is sent, we usually expect an `ICMP reply` if the pinging host is alive. The more interesting fact is that our previous scans did not do that because before Nmap could send an ICMP echo request, it would send an ARP ping resulting in an ARP reply. We can confirm this with the "--packet-trace" option. To ensure that ICMP echo requests are sent, we also define the option (-PE) for this.
