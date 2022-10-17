<img src=https://academy.hackthebox.com/storage/modules/90/0-PT-Process.png>

### Testing Methods
``` diff
+ External Penetration Test

Many pentests are performed from an external perspective or as an anonymous user on the Internet. 
Most customers want to ensure that they are as protected as possible against attacks on their external network perimeter. 
We can perform testing from our own host (hopefully using a VPN connection to avoid our ISP blocking us) or from a VPS. 
Some clients will not care about stealth, while others will request that we proceed as quietly as possible and approach the target systems to avoid being banned by the firewalls and IDS/IPS systems and avoid triggering an alarm. 
They may ask for a stealthy or "hybrid" approach where we gradually become "noisier" to test their detection capabilities. 
Ultimately our goal here is to access external-facing hosts, obtain sensitive data, or gain access to the internal network.


+ Internal Penetration Test

In contrast to an external pentest, an internal pentest is when we perform testing from within the corporate network. 
This stage may be executed after successfully penetrating the corporate network via the external pentest or starting from an assumed breach scenario.
Internal pentests may also access isolated systems with no internet access whatsoever, which usually requires our physical presence at the client's facility.
```

### Types of Penetration Testing
```
Type              Information Provided
_____________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________
Blackbox          Minimal. Only the essential information, such as IP addresses and domains, is provided.
Greybox           Extended. In this case, we are provided with additional information, such as specific URLs, hostnames, subnets, and similar. 
Whitebox          Maximum. Here everything is disclosed to us. This gives us an internal view of the entire structure, which allows us to prepare an attack using internal information. We may be given detailed configurations, admin credentials, web application source code, etc.
Red-Teaming       May include physical testing and social engineering, among other things. Can be combined with any of the above types.
Purple-Teaming    It can be combined with any of the above types. However, it focuses on working closely with the defenders.
```

### Types of Testing Environments
```
Network     Web App     Mobile              API                   Thick Clients
IoT         Cloud       Source Code         Physical Security     Employees
Hosts 	    Server      Security Policies   Firewalls             IDS/IPS
```

#### Pre-Engagement
```
```

#### Information Gathering
```
After a successful lateral movement, we can jump into Pillaging once again. This is local information gathering on the target system that we accessed.
```

#### Vulnerability Assessment 	
``` diff
+ thinking outside the box 
Nessus
Qualys
OpenVAS
```

#### Exploitation 
```
``` 

#### Post-Exploitation
```
```

#### Lateral Movement 	
```
```

#### Proof-of-Concept
```
```

#### Post-Engagement
```
```
