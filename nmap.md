### Nmap
<br>

```diff
nmap -p80,443 {target_IP}
+    -p <port ranges>: Only scan specified ports

nmap -sV {target_IP}
+    -sV: Enables version detection, which will detect what versions are running on what port.

If no alternative flag is specified in the command syntax, nmap will scan the most common 1000 TCP ports for active services.

nmap -p- -sV {target_IP}
+    -p-: to scan ports from 1 through 65535

nmap -sC -sV {target_IP}
+   -sC: Performs a script scan using the default set of scripts. It is equivalent to --script=default. Some of the scripts in this category are considered intrusive and should not be run against a target network without permission.
```
