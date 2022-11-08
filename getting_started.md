Setting Up a Pentest Distro

A hypervisor is software that allows us to create and run virtual machines (VMs). It will enable us to use our host computer to run multiple VMs by virtually sharing memory and processing resources. VMs on a hypervisor run isolated from the primary operating system, which offers a layer of isolation and protection between our production network and vulnerable networks, such as Hack The Box, or when connecting to client environments from a VM (though VM breakout vulnerabilities do arise from time to time).

##### ISO

The ISO (Optical disc image) file is essentially just a CD-ROM that can be mounted within our hypervisor of choice to build the VM by installing the operating system ourselves. An ISO gives us more room for customization, e.g., keyboard layout, locale, desktop environment switch, custom partitioning, etc., and therefore a more granular approach when setting up our attack VM.

##### OVA

The OVA (Open Virtual Appliance) file is a pre-built virtual appliance that contains an OVF XML file that specifies the VM hardware settings and a VMDK, which is the virtual disk that the operating system is installed on. An OVA is pre-built and therefore can be rapidly deployed to get up and running quicker.


Folder Structure

![[Pasted image 20221108152655.png]]


Here we have a folder for the client Acme Company with two assessments, <b>Internal Penetration Test (IPT)</b> and <b>External Penetration Test (EPT)</b>. Under each folder, we have subfolders for saving scan data, any relevant tools, logging output, scoping information (i.e., lists of IPs/networks to feed to our scanning tools), and an evidence folder that may contain any credentials retrieved during the assessment, any relevant data retrieved as well as screenshots.



Connecting to HTB VPN

At a high-level, VPN works by routing our connecting device's internet connection through the target VPN's private server instead of our internet service provider (ISP). When connected to a VPN, data originates from the VPN server rather than our computer and will appear to originate from a public IP address other than our own.


```shell-session
Dareck7@htb[/htb]$ sudo openvpn user.ovpn
```

The last line `Initialization Sequence Completed` tells us that we successfully connected to the VPN.


Where `sudo` tells our host to run the command as the elevated root user, `openvpn` is the VPN client, and the `user.ovpn` file is the VPN key. If we type `ifconfig` in another terminal window, we will see a `tun` adapter if we successfully connected to the VPN :

```shell-session
Dareck7@htb[/htb]$ ifconfig

<SNIP>

tun0: flags=4305<UP,POINTOPOINT,RUNNING,NOARP,MULTICAST>  mtu 1500
        inet 10.10.x.2  netmask 255.255.254.0  destination 10.10.x.2
        inet6 dead:beef:1::2000  prefixlen 64  scopeid 0x0<global>
        inet6 fe80::d82f:301a:a94a:8723  prefixlen 64  scopeid 0x20<link>
        unspec 00-00-00-00-00-00-00-00-00-00-00-00-00-00-00-00  txqueuelen
```


Typing `netstat -rn` will show us the networks accessible via the VPN :

```shell-session
Dareck7@htb[/htb]$ netstat -rn

Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
0.0.0.0         192.168.1.2     0.0.0.0         UG        0 0          0 eth0
10.10.14.0      0.0.0.0         255.255.254.0   U         0 0          0 tun0
10.129.0.0      10.10.14.1      255.255.0.0     UG        0 0          0 tun0
192.168.1.0     0.0.0.0         255.255.255.0   U         0 0          0 eth0
```

Here can see that the 10.129.0.0/16 network used for HTB Academy machines is accessible via the `tun0` adapter via the 10.10.14.0/23 network.

<br>


### Why Use A VPN?


We can use a VPN service and connect to a VPN server in another part of our country or another region of the world to obscure our browsing traffic or disguise our public IP address. This can provide us with some level of security and privacy. 

Still, since we are connecting to a company's server, there is always the chance that data is being logged or the VPN service is not following security best practices or the security features that they advertise. Using a VPN service comes with the risk that the provider is not doing what they are saying and are logging all data. Usage of a VPN service **does not** guarantee anonymity or privacy but is useful for bypassing certain network/firewall restrictions or when connected to a possible hostile network (i.e., a public airport wireless network). A VPN service should never be used with the thought that it will protect us from the consequences of performing nefarious activities.



There are two main types of remote access VPNs: <b>client-based VPN</b> and <b>SSL VPN</b>. SSL VPN uses the web browser as the VPN client. The connection is established between the browser and an SSL VPN gateway can be configured to only allow access to web-based applications such as email and intranet sites, or even the internal network but without the need for the end user to install or use any specialized software. 

Client-based VPN requires the use of client software to establish the VPN connection. Once connected, the user's host will work mostly as if it were connected directly to the company network and will be able to access any resources (applications, hosts, subnets, etc.) allowed by the server configuration. Some corporate VPNs will provide employees with full access to the internal corporate network, while others will place users on a specific segment reserved for remote workers.



