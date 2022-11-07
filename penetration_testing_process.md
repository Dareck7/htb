## Penetration Testing Process

<p>A penetration testing process is defined by successive steps and events performed by the penetration tester to find a path to the predefined objective.<p> 	
  
<img src=https://academy.hackthebox.com/storage/modules/90/0-PT-Process.png>

### 1. Pre-Engagement
```
The first step is to create all the necessary documents in the pre-engagement phase, discuss the assessment objectives, and clarify any questions.
```
<br>
  
### 2. Information Gathering
```diff
Once the pre-engagement activities are complete, we investigate the company's existing website we have been assigned to assess. 
We identify the technologies in use and learn how the web application functions.
  
@@ We can obtain the necessary information relevant to us in many different ways : @@
  
    - Open-Source Intelligence (OSINT)
    - Infrastructure Enumeration (name servers, mail servers, web servers, cloud instances, and more)
    - Service Enumeration
    - Host Enumeration
    - Pillaging (understanding the role of the system)
```
<br>
  
### 3. Vulnerability Assessment 	
``` diff
With this information, we can look for known vulnerabilities and investigate questionable features that may allow for unintended actions.

@@ Suppose we found an open TCP port 2121 on a host during the information-gathering phase : @@

    - a TCP port 2121. - TCP already means that this service is connection-oriented.
    - Is this a standard port? - No, because these are between 0-1023, aka well-known or system ports
    - Are there any numbers in this port number that look familiar? - Yes, TCP port 21 (FTP). From our experience, we will get to know many standard ports and their services, which administrators often try to disguise, but often use "easy to remember" alternatives.
    - Based on our guess, we can try to connect to the service using Netcat or an FTP client and try to establish a connection to confirm or disprove our guess.
  
+ Nessus, Qualys, OpenVAS
```
- <p>Find vulnerability disclosures and CVEs :</p>
  
    - [CVEdetails](https://www.cvedetails.com/)
    - [Packet Storm Security](https://packetstormsecurity.com/)
    - [Exploit DB](https://www.exploit-db.com/)
    - [NIST](https://nvd.nist.gov/vuln/search?execution=e2s1)
    - [SecurityFocus](https://bugtraq.securityfocus.com/archive)
    - [Vulners](https://vulners.com/) 

<br>

### 4. Exploitation 
```diff
Once we have found potential vulnerabilities, we prepare our exploit code, tools, and environment and test the webserver for these potential vulnerabilities.

- Complexity represents the effort of exploiting a specific vulnerability.
``` 
We need to assess the probability of successfully executing a particular attack against the target. [CVSS Scoring](https://nvd.nist.gov/vuln-metrics/cvss) can help us here, using the [NVD calculator](https://nvd.nist.gov/vuln-metrics/cvss/v3-calculator) better to calculate the specific attacks and their probability of success.

<br>

### 5. Post-Exploitation
```diff
Once we have successfully exploited the target, we jump into information gathering and examine the webserver from the inside. 
If we find sensitive information during this stage, we try to escalate our privileges (depending on the system and configurations).
From the inside (local) perspective, we have many more possibilities and alternatives to access certain information that is relevant to us. 
We want to get the privileges of the root (on Linux-based systems) or the domain administrator/local administrator/SYSTEM (on Windows-based systems).

- Persistence is maintaining access to the exploited host.

Security systems such as Data Loss Prevention (DLP) and Endpoint Detection and Response (EDR) help detect and prevent data exfiltration. 
In addition to Network Monitoring, many companies use encryption on hard drives to prevent external parties from viewing such information.

@@ What is the name of the security regulation for credit card payments a company must adhere to? PCI-DSS @@
```
<br>

### 6. Lateral Movement 	
```diff
If other servers and hosts in the internal network are in scope, we then try to move through the network and access other hosts and servers using the information we have gathered.
The goal here is that we test what an attacker could do within the entire network.

- One of the most common examples is ransomware. 

If a system in the corporate network is infected with ransomware, it can spread across the entire network. 
Some pivoting techniques (access inaccessible systems via an intermediary system) allow us to use the exploited host as a proxy and perform all the scans from our attack machine or VM. In this way, we make non-routable networks can still be reached. 

Another standard method is to use our existing credentials on other systems. We can use the tool Responder to intercept NTLMv2 hashes. 
If we can intercept a hash from an administrator, then we can use the pass-the-hash technique to log in as that administrator (in most cases) on multiple hosts and servers.
After all, the Lateral Movement stage aims to move through the internal network.

@@ There are many ways to protect against lateral movement, including network (micro) segmentation, threat monitoring, IPS/IDS, EDR, etc. @@
```
<br>

### 7. Proof-of-Concept
```diff
Finally, we are ready to show off our hard work and help our client, and those responsible for remediation efficiently reproduce our results.
We create a proof-of-concept that proves that these vulnerabilities exist and potentially even automate the individual steps that trigger these vulnerabilities.

@@ The more practical version of a PoC is a script or code that automatically exploits the vulnerabilities found. @@

Therefore, working against our script instead of with it and modifying and securing the systems so that our script no longer works does not mean that the information obtained from the script cannot be obtained in another way.
Show how fixing one flaw will break the chain, but the other flaws will still exist.
For example, if a user uses the password Password123, the underlying vulnerability is not the password but the password policy.

+ High quality stands for high standards, which we should emphasize through our remediation recommendations.
```
<br>

### 8. Post-Engagement
```diff
Finally, the documentation is completed and presented to our client as a formal report deliverable. 
Afterward, we may hold a report walkthrough meeting to clarify anything about our testing or results and provide any needed support to personnel tasked with remediating our findings.
Once testing is complete, we should perform any necessary cleanup, such as deleting tools/scripts uploaded to target systems, reverting any (minor) configuration changes we may have made, etc.

- We should not keep any Personal Identifiable Information (PII), potentially incriminating info, or other sensitive data we came across throughout testing.

We should not be implementing changes ourselves or even giving precise remediation advice (i.e., for SQL Injection, we may say "sanitize user input" but not give the client a rewritten piece of code).
We should ensure that any systems used to connect to the client's systems or process data have been wiped or destroyed and that any artifacts leftover from the engagement are stored securely (encrypted) per our firm's policy and per contractual obligations to our client.

@@ What designation do we typically give a report when it is first delivered to a client for a chance to review and comment? DRAFT @@
```
<br>
<br>
<br>

### Testing Methods
``` diff
- External Penetration Test

Many pentests are performed from an external perspective or as an anonymous user on the Internet. 
Most customers want to ensure that they are as protected as possible against attacks on their external network perimeter. 
We can perform testing from our own host (hopefully using a VPN connection to avoid our ISP blocking us) or from a VPS. 
Some clients will not care about stealth, while others will request that we proceed as quietly as possible and approach the target systems to avoid being banned by the firewalls and IDS/IPS systems and avoid triggering an alarm. 
They may ask for a stealthy or "hybrid" approach where we gradually become "noisier" to test their detection capabilities. 
Ultimately our goal here is to access external-facing hosts, obtain sensitive data, or gain access to the internal network.


- Internal Penetration Test

In contrast to an external pentest, an internal pentest is when we perform testing from within the corporate network. 
This stage may be executed after successfully penetrating the corporate network via the external pentest or starting from an assumed breach scenario.
Internal pentests may also access isolated systems with no internet access whatsoever, which usually requires our physical presence at the client's facility.
```
<br>

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
<br>
<br>
The field of information technology changes rapidly. New attacks are discovered frequently, and we need to stay on top of the latest and greatest TTPs  (Tactics, Techniques, and Procedures) to be as effective as possible and provide our clients with the necessary information to help secure their environments from an ever-evolving threat landscape.
