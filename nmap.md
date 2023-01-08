# Nmap

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
- [Host and Port Scanning](#host-and-port-scanning)
    - [Discovering Open TCP Ports](#discovering-open-tcp-ports)
    - [Filtered Ports](#filtered-ports)
    - [Discovering Open UDP Ports](#discovering-open-udp-ports)
- [Saving the Results](#saving-the-results)
    - [Different Formats](#different-formats)


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

### Nmap Architecture

- Host discovery
- Port scanning
- Service enumeration and detection
- OS detection
- Scriptable interaction with the target service (Nmap Scripting Engine)

### Syntax

```console
sudo nmap <scan types> <options> <target>
```

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

## Host Discovery

The most effective host discovery method is to use `ICMP echo requests`, which we will look into.

### Scan Network Range
```console
sudo nmap 10.129.2.0/24 -sn -n -oA tnet | grep for | cut -d" " -f5 > hosts.lst
```
- `10.129.2.0/24` 	Target network range.
- `-sn` 	Disables port scanning.
- `-n`      Disables DNS resolution.
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
sudo nmap -sn -n -oA tnet -iL hosts.lst | grep for | cut -d" " -f5 > ips.txt
```
- `-iL` 	Performs defined scans against targets in provided 'hosts.lst' list.

In this example, we see that only 3 of 7 hosts are active. Remember, this may mean that the other hosts ignore the default ICMP echo requests because of their firewall configurations. Since Nmap does not receive a response, it marks those hosts as `inactive`.

<br>

### Scan Multiple IPs

It can also happen that we only need to scan a small part of a network:

```console
sudo nmap -sn -n -oA tnet 10.129.2.18 10.129.2.19 10.129.2.20 | grep for | cut -d" " -f5 > ips.txt
```

If these IP addresses are next to each other, we can also define the range in the respective octet:

```console
sudo nmap -sn -n -oA tnet 10.129.2.18-20 | grep for | cut -d" " -f5 > ips.txt
```

<br>

### Scan Single IP

Before we scan a single host for open ports and its services, we first have to determine if it is alive or not.

```console
sudo nmap 10.129.2.18 -sn -n -oA host -PE --reason
```
- `-PE` 	Performs the ping scan by using 'ICMP Echo requests' against the target.
- `--reason ` Displays the reason for specific result.

<br>

We see here that Nmap does indeed detect whether the host is alive or not through the `ARP request` and `ARP reply` alone. To disable ARP requests and scan our target with the desired `ICMP echo requests`, we can disable ARP pings by setting the "`--disable-arp-ping`" option. Then we can scan our target again and look at the packets sent and received (`--packet-trace`).

```console
Dareck7@htb[/htb]$ sudo nmap 10.129.2.18 -sn -oA host -PE --packet-trace --disable-arp-ping 

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-15 00:12 CEST
SENT (0.0107s) ICMP [10.10.14.2 > 10.129.2.18 Echo request (type=8/code=0) id=13607 seq=0] IP [ttl=255 id=23541 iplen=28 ]
RCVD (0.0152s) ICMP [10.129.2.18 > 10.10.14.2 Echo reply (type=0/code=0) id=13607 seq=0] IP [ttl=128 id=40622 iplen=28 ]
Nmap scan report for 10.129.2.18
Host is up (0.086s latency).
MAC Address: DE:AD:00:00:BE:EF
Nmap done: 1 IP address (1 host up) scanned in 0.11 seconds
```

<br>
<br>
<br>

## Host and Port Scanning

| State      |  Description    |
|--------------|----------------|
| `open`   | This indicates that the connection to the scanned port has been established. These connections can be **TCP connections**, **UDP datagrams** as well as **SCTP associations**. |
| `closed`      | When the port is shown as closed, the TCP protocol indicates that the packet we received back contains an `RST` flag. This scanning method can also be used to determine if our target is alive or not.  |
| `filtered` | Nmap cannot correctly identify whether the scanned port is open or closed because either no response is returned from the target for the port or we get an error code from the target. |
| `unfiltered` |  	This state of a port only occurs during the **TCP-ACK** scan and means that the port is accessible, but it cannot be determined whether it is open or closed. |
| `open-filtered` | If we do not get a response for a specific port, `Nmap` will set it to that state. This indicates that a firewall or packet filter may protect the port. |
| `closed-filtered` | This state only occurs in the **IP ID idle** scans and indicates that it was impossible to determine if the scanned port is closed or filtered by a firewall. |


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

<br>
<br>
<br>

## Saving the Results

### Different Formats

- Normal output (`-oN`) with the `.nmap` file extension
- Grepable output (`-oG`) with the `.gnmap` file extension
- XML output (`-oX`) with the `.xml` file extension

We can also specify the option (`-oA`) to save the results in all formats.
```console
sudo nmap 10.129.2.28 -p- -oA target
```

Next, we look at the different formats Nmap has created for us:
```console
Dareck7@htb[/htb]$ ls

target.gnmap target.html  target.nmap
```
