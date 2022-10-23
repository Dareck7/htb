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

    
| **Command** | **Description** |
| --------------|-------------------|
|`uname -a`|  Shows the current kernel and OS information |
|`sudo <command>`| Executes the command as root (Administrator) |

```


    a TCP port 2121. - TCP already means that this service is connection-oriented.

    Is this a standard port? - No, because these are between 0-1023, aka well-known or system ports

    Are there any numbers in this port number that look familiar? - Yes, TCP port 21 (FTP). From our experience, we will get to know many standard ports and their services, which administrators often try to disguise, but often use "easy to remember" alternatives.

Based on our guess, we can try to connect to the service using Netcat or an FTP client and try to establish a connection to confirm or disprove our guess.
```
