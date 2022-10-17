[HTB][Starting Point]


#sudo openvpn {filename}.ovpn
    - tun0: tunnel interface
    

Test our connection to the target (ICMP echo request):
```
ping -c2 {target_IP}
```
Note that this might not always work in a large-scale corporate environment, as firewalls usually have rules
to prevent pinging between hosts, even in the same subnet (LAN), to avoid insider threats and discover
other hosts and services.

If you would like to learn about a specific command in more depth:
```
man {commandName}
```
