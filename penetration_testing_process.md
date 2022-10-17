## Penetration Testing Process

<p>A penetration testing process is defined by successive steps and events performed by the penetration tester to find a path to the predefined objective.<p> 	
  
<img src=https://academy.hackthebox.com/storage/modules/90/0-PT-Process.png>

#### 1. Pre-Engagement
```
The first step is to create all the necessary documents in the pre-engagement phase, discuss the assessment objectives, and clarify any questions.
```
#### 2. Information Gathering
```
Once the pre-engagement activities are complete, we investigate the company's existing website we have been assigned to assess. 
We identify the technologies in use and learn how the web application functions.
```
#### 3. Vulnerability Assessment 	
``` diff
With this information, we can look for known vulnerabilities and investigate questionable features that may allow for unintended actions.
=> Nessus, Qualys, OpenVAS
+ thinking outside the box 
```
#### 4. Exploitation 
```
Once we have found potential vulnerabilities, we prepare our exploit code, tools, and environment and test the webserver for these potential vulnerabilities.
``` 
#### 5. Post-Exploitation
```
Once we have successfully exploited the target, we jump into information gathering and examine the webserver from the inside. 
If we find sensitive information during this stage, we try to escalate our privileges (depending on the system and configurations).
```
#### 6. Lateral Movement 	
```
If other servers and hosts in the internal network are in scope, we then try to move through the network and access other hosts and servers using the information we have gathered.
```
#### 7. Proof-of-Concept
```
We create a proof-of-concept that proves that these vulnerabilities exist and potentially even automate the individual steps that trigger these vulnerabilities.
```
#### 8. Post-Engagement
```
Finally, the documentation is completed and presented to our client as a formal report deliverable. 
Afterward, we may hold a report walkthrough meeting to clarify anything about our testing or results and provide any needed support to personnel tasked with remediating our findings.
```
<br>

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
