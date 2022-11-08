### Nmap
<br>

```diff
If no alternative flag is specified in the command syntax, nmap will scan the most common 1000 TCP ports for active services.

nmap -sC -sV {target_IP}
   -sC: Performs a script scan using the default set of scripts. It is equivalent to --script=default. Some of the scripts in this category are considered intrusive and should not be run against a target network without permission.
   -sV: Enables version detection, which will detect what versions are running on what port.

nmap -p- --min-rate 5000 -sV 10.129.136.91
  -p- : This flag scans for all TCP ports ranging from 0-65535
  --min-rate : This is used to specify the minimum number of packets Nmap should send per second; it speeds up the scan as the number goes higher
```

Nmap uses a port-services database of well-known services in order to determine the service
running on a particular port. It later also sends some service-specific requests to that port to
determine the service version & any additional information about it.
Thus, Nmap is mostly but not always correct about the service info for a particular port.
