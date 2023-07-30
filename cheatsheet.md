# HTB CheatSheet

<img src="img/htb2.png">

# Information Gathering - Web Edition

- [Passive Information Gathering](#passive-information-gathering)
    - [WHOIS](#whois)
    - [DNS Enumeration](#dns-enumeration)
    - [Passive Subdomain Enumeration](#passive-subdomain-enumeration)
    - [Passive Infrastructure Identification](#passive-infrastructure-identification)
- [Active Information Gathering](#active-information-gathering)
    - [Active Infrastructure Identification](#active-infrastructure-identification)
    - [Active Subdomain Enumeration](#active-subdomain-enumeration)
    - [Virtual Hosts](#virtual-hosts)
    - [Crawling](#crawling)
- [Nmap](#nmap)
    - [Host Discovery](#host-discovery)
    - [General Enumeration](#general-enumeration)
    - [Scanning UDP](#scanning-udp)
    - [Vulnerability Assessment](#vulnerability-assessment)
    - [nmapAutomator](#automated-nmap-scanning-(my-preference-is-nmapautomator,-never-missed-a-port))

<br>

## Passive Information Gathering

### WHOIS
>The WHOIS domain lookups allow us to retrieve information about the domain name of an already registered domain. In simple terms, the Whois database is a searchable list of all domains currently registered worldwide.

```bash
$ export TARGET="domain.tld"	
$ whois $TARGET
```
There are some online versions like [whois.domaintools.com]([whois.domaintools.com) we can also use.

<br>

### DNS Enumeration

**Identify the `A` record for the target domain.**
```bash
$ dig a $TARGET @<nameserver/IP>
```
**Identify the `PTR` record for the target IP address.**
```bash
$ dig -x <IP> @<nameserver/IP>
```
**Identify `ANY` records for the target domain.**
```bash
$ dig any $TARGET @<nameserver/IP>
```
>The more recent `RFC8482` specified that ANY DNS requests be abolished. Therefore, we may not receive a response to our ANY request from the DNS server or get a reference to the said `RFC8482`.

**Identify the `TXT` records for the target domain.**
```bash
$ dig txt $TARGET @<nameserver/IP>
```
**Identify the `MX` records for the target domain.**
```bash
$ dig mx $TARGET @<nameserver/IP>
```

<br>

### Passive Subdomain Enumeration

  - [VirusTotal](https://www.virustotal.com/gui/home/search)
  - [Censys](https://search.censys.io/)
  - [crt.sh](https://crt.sh/)
  
**Certificate Transparancy**
```bash
$ curl -s "https://crt.sh/?q=${TARGET}&output=json" | jq -r '.[] | "\(.name_value)\n\(.common_name)"' | sort -u > "target_crt.sh.txt"
```

**theHarvester**

The tool collects emails, names, subdomains, IP addresses, and URLs from various public data sources for passive information gathering.
```bash
$ theharvester -d kali.org -l 500 -b google
```
**All subdomains for a given domain.**
```bash
curl -s https://sonar.omnisint.io/subdomains/{domain} | jq -r '.[]' | sort -u	
```
**All TLDs found for a given domain.**
```bash
curl -s https://sonar.omnisint.io/tlds/{domain} | jq -r '.[]' | sort -u	
```
**All results across all TLDs for a given domain.**
```bash
curl -s https://sonar.omnisint.io/all/{domain} | jq -r '.[]' | sort -u	
```
**Reverse DNS lookup on IP address.**
```bash
curl -s https://sonar.omnisint.io/reverse/{ip} | jq -r '.[]' | sort -u	
```
**Reverse DNS lookup of a CIDR range.**
```bash
curl -s https://sonar.omnisint.io/reverse/{ip}/{mask} | jq -r '.[]' | sort -u	
```

<br>

### Passive Infrastructure Identification

  - [Netcraft](https://sitereport.netcraft.com)
  - [WaybackMachine](http://web.archive.org/)
  - [WayBackURLs](https://github.com/tomnomnom/waybackurls)

Crawling URLs from a domain with the date it was obtained.
```bash
$ waybackurls -dates https://$TARGET > waybackurls.txt	
```

<br>
<br>

## Active Information Gathering

### Active Infrastructure Identification

**Web Servers**

HTTP Headers
```bash
$ curl -I http://${TARGET}
```
- X-Powered-By header
- Cookies (`ASPSESSIONID`, `PHPSESSID`, `JSESSION`)

WhatWeb
```bash
$ whatweb -a3 https://${TARGET} -v
```
- [Wappalyzer](https://www.wappalyzer.com/) has similar functionality to Whatweb


WafW00f
```bash
# check possible WAF in place
$ wafw00f -av http://${TARGET}
```
- https://github.com/EnableSecurity/wafw00f

Aquatone
```bash
$ cat hosts.txt | aquatone -out ./aquatone -screenshot-timeout 1000
```
When it finishes, we will have a file called `aquatone_report.html`
- https://github.com/michenriksen/aquatone

<br>

#### Active Subdomain Enumeration

ZoneTransfers
>https://hackertarget.com/zone-transfer/

1. Identifying Nameservers
```bash
$ dig ns zonetransfer.me
```
2. Testing for AXFR Zone Transfer
```bash
$ dig axfr zonetransfer.me @nsztm1.digi.ninja
```
If we manage to perform a successful zone transfer for a domain, there is no need to continue enumerating this particular domain as this will extract all the available information.

Gobuster
```bash
$ gobuster dns -q -r "${NS}" -d "${TARGET}" -w "${WORDLIST}" -o "gobuster_${TARGET}.txt"
```
sublist3r
vt-subdomains.py

#### Virtual hosts

-  IP-based virtual hosting
-  Name-based virtual hosting

```bash
$ curl -s http://192.168.10.10 -H "Host: ${vhost}.randomtarget.com"
```
**Automating Virtual Hosts Discovery**

[ffuf](https://github.com/ffuf/ffuf)
```bash
$ ffuf -c -w /path/to/wordlist -u http://192.168.10.10 -H "HOST: FUZZ.randomtarget.com" -fs <size>
```

#### Crawling

**[ZAP](https://www.zaproxy.org/)**
  - We can use the spidering functionality (enumerates the resources it finds in links and forms)

**[FFuF](https://github.com/ffuf/ffuf)**
```bash
$ ffuf -recursion -recursion-depth 1 -u http://192.168.10.10/FUZZ -w /opt/useful/SecLists/Discovery/Web-Content/raft-small-directories-lowercase.txt
```
- `-recursion`: Activates the recursive scan.
- `-recursion-depth`: Specifies the maximum depth to scan.
- `-u`: Our target URL, and FUZZ will be the injection point.
- `-w`: Path to our wordlist.

**Sensitive Information Disclosure**

There are some lists of common extensions we can find in the `raft-[ small | medium | large ]-extensions.txt` files from [SecLists](https://github.com/danielmiessler/SecLists/tree/master/Discovery/Web-Content).

-  The first step will be to create a file with the following folder names and save it as `folders.txt`.
```
wp-admin
wp-content
wp-includes
```
- Next, we will extract some keywords from the website using [CeWL](https://github.com/digininja/CeWL).
```bash
$ cewl -m5 --lowercase -w wordlist.txt http://192.168.10.10
```
- The next step will be to combine everything in `ffuf` to see if we can find some juicy information.
```bash
$ ffuf -w ./folders.txt:FOLDERS,./wordlist.txt:WORDLIST,./extensions.txt:EXTENSIONS -u http://192.168.10.10/FOLDERS/WORDLISTEXTENSIONS
```
<br>

### Nmap

#### Host Discovery
```bash
$ nmap ${IPRANGE} -sn -n | grep for | cut -d" " -f5 > hosts.lst
```

#### General Enumeration

Quick Scan
```bash
$ nmap -sC -sV -oA nmap/machinename $ip
```
Doing a blanket scan first
```bash
$ nmap -Pn -n -vvv -p- -T4 -oN nmap/allports $ip
```
Then an aggressive scan after based on what you find
```bash
$ nmap -Pn -n -vvv -p<ports> -A -oN nmap/targeted $ip
```

#### UDP Scanning
```bash
$ sudo nmap -Pn -n -vvv -sU -oN nmap/udp $ip
```
#### Vulnerability Assessment

> `/usr/share/nmap/scripts`
- [OWASP](https://owasp.org/www-project-web-security-testing-guide/)
```bash
$ nmap -Pn -n -vvv $ip -p<port> -sV --script vuln 
```
#### Automated nmap scanning (my preference is nmapAutomator, never missed a port)
```bash
# It is recommended to scan ONE IP at a time
# Do NOT overload the network
# All scans, consecutively: Quick, Targeted, UDP, All ports, Vuln scan, CVE scan, Gobuster, Nikto
$ nmapAutomator $ip All
```
