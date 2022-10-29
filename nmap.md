### Nmap
<br>

```diff
nmap -sV {target_IP}
+    -sV: Probe open ports to determine service/version info

If no alternative flag is specified in the command syntax, nmap will scan the most common 1000 TCP ports for active services.

nmap -p- -sV {target_IP}
+    -p-: to scan ports from 1 through 65535
```
