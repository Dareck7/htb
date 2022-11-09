
<u>Test our connection to the target (ICMP echo request) :</u>

```
ping -c2 {target_IP}
```

Note that this might not always work in a large-scale corporate environment, as firewalls usually have rules to prevent pinging between hosts, even in the same subnet (LAN), to avoid insider threats and discover other hosts and services.


<u>If you would like to learn about a specific command in more depth :</u>
```
man {commandName}
```
    
<u>Print the flag :</u>
```
cat flag.txt
```


| **Command** | **Description** |
| --------------|-------------------|
|`uname -a`|  Shows the current kernel and OS information |
|`sudo <command>`| Executes the command as root (Administrator) |