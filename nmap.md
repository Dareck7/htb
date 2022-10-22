### Nmap
<br>

```diff
nmap -sV {target_IP}
+     -sV: Probe open ports to determine service/version info

nmap -p- -sV {target_IP}
      -p-: to scan ports from 1 through 65535
```
