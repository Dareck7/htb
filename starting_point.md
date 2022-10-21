### Starting Point

<br>

#### Connect to the lab
```diff
sudo openvpn {filename}.ovpn
+ tun0 (tunnel interface)
```
<br>
    
#### Test our connection to the target (ICMP echo request)
```
ping -c2 {target_IP}
```
<p>Note that this might not always work in a large-scale corporate environment, as firewalls usually have rules
to prevent pinging between hosts, even in the same subnet (LAN), to avoid insider threats and discover other hosts and services.<p><br>


#### If you would like to learn about a specific command in more depth
```
man {commandName}
```
<br>
    
#### Print the flag
```
cat flag.txt
```

VPS (Virtual Private Server)
