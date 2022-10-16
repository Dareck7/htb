<img src=https://academy.hackthebox.com/storage/modules/90/0-PT-Process.png>

#### Pre-Engagement
```
The pre-engagement stage is where the main commitments, tasks, scope, limitations, and related agreements are documented in writing. 
During this stage, contractual documents are drawn up, and essential information is exchanged 
that is relevant for penetration testers and the client, depending on the type of assessment.
```

#### Information Gathering
```
Next, we move towards the Information Gathering stage. 
Before any target systems can be examined and attacked, we must first identify them. 
It may well be that the customer will not give us any information about their network and components other than a domain name or just a listing of in-scope IP addresses/network ranges. 
Therefore, we need to get an overview of the target web application(s) or network before proceeding further.
When we do not have enough information on hand, here we can dig deeper to find more information that will give us a more accurate view.
```

#### Vulnerability Assessment 	
``` diff
The next stop on our journey is Vulnerability Assessment, where we use the information found to identify potential weaknesses. 
We can use vulnerability scanners that will scan the target systems for known vulnerabilities and manual analysis 
+ (an analysis is more about thinking outside the box) 
where we try to look behind the scenes to discover where the potential vulnerabilities might lie.
```

#### Exploitation 
```
The first we can jump into is the Exploitation stage. This happens when we do not yet have access to a system or application. 
Of course, this assumes that we have already identified at least one gap and prepared everything necessary to attempt to exploit it.
``` 

#### Post-Exploitation
```
The second way leads to the Post-Exploitation stage, where we escalate privileges on the target system. 
This assumes that we are already on the target system and can interact with it.
```

#### Lateral Movement 	
```
Our third option is the Lateral Movement stage, where we move from the already exploited system through the network and attack other systems. 
Again, this assumes that we are already on a target system and can interact with it. 
However, privilege escalation is not strictly necessary because interacting with the system already allows us to move further in the network under certain circumstances. 
Other times we will need to escalate privileges before moving laterally. Every assessment is different.
```
