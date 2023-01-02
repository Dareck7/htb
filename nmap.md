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
- [Host and Port Scanning](#host-and-port-scanning)
    - [Discovering Open TCP Ports](#discovering-open-tcp-ports)
    - [Filtered Ports](#filtered-ports)
    - [Discovering Open UDP Ports](#discovering-open-udp-ports)

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
sudo nmap 10.129.2.18 -sn -oA host -PE --reason
```
- `10.129.2.18` 	Performs defined scans against the target.
- `-sn` 	Disables port scanning.
- `-oA host` 	Stores the results in all formats starting with the name 'host'
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

<br>
<br>

### Discovering Open TCP Ports

By default, `Nmap` scans the top 1000 TCP ports with the SYN scan (`-sS`). This SYN scan is set only to default when we run it as root because of the socket permissions required to create raw TCP packets. Otherwise, the TCP scan (`-sT`) is performed by default.<br>

#### Nmap - Trace the Packets

```console
Dareck7@htb[/htb]$ sudo nmap 10.129.2.28 -p 21 --packet-trace -Pn -n --disable-arp-ping

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-15 15:39 CEST
SENT (0.0429s) TCP 10.10.14.2:63090 > 10.129.2.28:21 S ttl=56 id=57322 iplen=44  seq=1699105818 win=1024 <mss 1460>
RCVD (0.0573s) TCP 10.129.2.28:21 > 10.10.14.2:63090 RA ttl=64 id=0 iplen=40  seq=0 win=0
Nmap scan report for 10.11.1.28
Host is up (0.014s latency).

PORT   STATE  SERVICE
21/tcp closed ftp
MAC Address: DE:AD:00:00:BE:EF (Intel Corporate)

Nmap done: 1 IP address (1 host up) scanned in 0.07 seconds
```

- `10.129.2.28` 	Scans the specified target.
- `-p 21` 	Scans only the specified port.
- `--packet-trace` 	Shows all packets sent and received.
- `-n` 	Disables DNS resolution.
- `--disable-arp-ping` 	Disables ARP ping.

<br>

#### Request
- `SENT (0.0429s)` Indicates the SENT operation of Nmap, which sends a packet to the target.
- `TCP` 	Shows the protocol that is being used to interact with the target port.
- `10.10.14.2:63090 >` 	Represents our IPv4 address and the source port, which will be used by Nmap to send the packets.
- `10.129.2.28:21` 	Shows the target IPv4 address and the target port.
- `S` 	SYN flag of the sent TCP packet.
- `ttl=56 id=57322 iplen=44 seq=1699105818 win=1024 mss 1460` 	Additional TCP Header parameters.

#### Response
- `RCVD (0.0573s)` 	Indicates a received packet from the target.
- `TCP` 	Shows the protocol that is being used.
- `10.129.2.28:21 >` 	Represents targets IPv4 address and the source port, which will be used to reply.
- `10.10.14.2:63090` 	Shows our IPv4 address and the port that will be replied to.
- `RA` 	RST and ACK flags of the sent TCP packet.
- `ttl=64 id=0 iplen=40 seq=0 win=0` 	Additional TCP Header parameters.

<br>
<br>

### Filtered Ports

When a packet gets dropped, `Nmap` receives no response from our target, and by default, the retry rate (`--max-retries`) is set to 1. This means Nmap will resend the request to the target port to determine if the previous packet was not accidentally mishandled.

```console
Dareck7@htb[/htb]$ sudo nmap 10.129.2.28 -p 139 --packet-trace -n --disable-arp-ping -Pn

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-15 15:45 CEST
SENT (0.0381s) TCP 10.10.14.2:60277 > 10.129.2.28:139 S ttl=47 id=14523 iplen=44  seq=4175236769 win=1024 <mss 1460>
SENT (1.0411s) TCP 10.10.14.2:60278 > 10.129.2.28:139 S ttl=45 id=7372 iplen=44  seq=4175171232 win=1024 <mss 1460>
Nmap scan report for 10.129.2.28
Host is up.

PORT    STATE    SERVICE
139/tcp filtered netbios-ssn
MAC Address: DE:AD:00:00:BE:EF (Intel Corporate)

Nmap done: 1 IP address (1 host up) scanned in 2.06 seconds
```
- `10.129.2.28` 	Scans the specified target.
- `-p 139` 	Scans only the specified port.
- `--packet-trace` 	Shows all packets sent and received.
- `-n` 	Disables DNS resolution.
- `--disable-arp-ping` 	Disables ARP ping.
- `-Pn` 	Disables ICMP Echo requests.

<br>
<br>

### Discovering Open UDP Ports

```console
sudo nmap 10.129.2.28 -F -sU
```
- `10.129.2.28` 	Scans the specified target.
- `-F` 	Scans top 100 ports.
- `-sU` 	Performs a UDP scan.

<br>

We often do not get a response back because `Nmap` sends empty datagrams to the scanned UDP ports, and we do not receive any response. So we cannot determine if the UDP packet has arrived at all or not. If the UDP port is `open`, we only get a response if the application is configured to do so.

```console
Dareck7@htb[/htb]$ sudo nmap 10.129.2.28 -sU -Pn -n --disable-arp-ping --packet-trace -p 137 --reason 

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-15 16:15 CEST
SENT (0.0367s) UDP 10.10.14.2:55478 > 10.129.2.28:137 ttl=57 id=9122 iplen=78
RCVD (0.0398s) UDP 10.129.2.28:137 > 10.10.14.2:55478 ttl=64 id=13222 iplen=257
Nmap scan report for 10.129.2.28
Host is up, received user-set (0.0031s latency).

PORT    STATE SERVICE    REASON
137/udp open  netbios-ns udp-response ttl 64
MAC Address: DE:AD:00:00:BE:EF (Intel Corporate)

Nmap done: 1 IP address (1 host up) scanned in 0.04 seconds
```

<br>
<br>
<br>
