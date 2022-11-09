

Nmap uses a port-services database of well-known services in order to determine the service running on a particular port. It later also sends some service-specific requests to that port to determine the service version & any additional information about it.

Port numbers range from 1 to 65,535, with the range of well-known ports 1 to 1,023 being reserved for privileged services. Port 0 is a reserved port in TCP/IP networking and is not used in TCP or UDP messages. If anything attempts to bind to port 0 (such as a service), it will bind to the next available port above port 1,024 because port 0 is treated as a "wild card" port.

If we don't specify any additional options, Nmap will only scan the **1,000** most common ports by default.

```shell-session
Dareck7@htb[/htb]$ nmap 10.129.42.253

Starting Nmap 7.80 ( https://nmap.org ) at 2021-02-25 16:07 EST
Nmap scan report for 10.129.42.253
Host is up (0.11s latency).
Not shown: 995 closed ports
PORT    STATE SERVICE
21/tcp  open  ftp
22/tcp  open  ssh
80/tcp  open  http
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds

Nmap done: 1 IP address (1 host up) scanned in 2.19 seconds
```

Under the `PORT` heading, it also tells us that these are TCP ports. By default, `Nmap` will conduct a TCP scan. The `STATE` heading confirms that these ports are open. Sometimes we will see other ports listed that have a different state, such as `filtered`. This can happen if a firewall is only allowing access to the ports from specific addresses.

The `SERVICE` heading tells us the service's name is typically mapped to the specific port number. However, the default scan will not tell us what is listening on that port. `VERSION` heading -> reports the service version and the operating system if this is possible to identify.


```shell-session
nmap -sV -sC -p- {target_IP}

-sV: instructs `Nmap` to perform a version scan
-sC: `Nmap` scripts should be used to try and obtain more detailed information
-p-: tells Nmap that we want to scan all 65,535 TCP ports.
```


#### Nmap Scripts

```shell-session
Dareck7@htb[/htb]$ locate scripts/citrix

/usr/share/nmap/scripts/citrix-brute-xml.nse
/usr/share/nmap/scripts/citrix-enum-apps-xml.nse
/usr/share/nmap/scripts/citrix-enum-apps.nse
/usr/share/nmap/scripts/citrix-enum-servers-xml.nse
/usr/share/nmap/scripts/citrix-enum-servers.nse
```

<u>The syntax for running an Nmap script is :</u>

```
nmap --script <script name> -p<port> <host>
```

