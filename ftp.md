### FTP (File Transfer Protocol)

```diff
sudo apt install ftp -y
ftp -h
ftp {target_IP}

+ TCP/21

ftp> help / ?
ftp> dir / ls
ftp> get flag.txt
ftp> exit / bye
```

<p>For secure transmission that protects the username and password and encrypts the content, 
FTP is often secured with SSL/TLS (FTPS) or replaced with SSH-tunneling => SSH File Transfer Protocol (SFTP).</p>

<p>What is username that is used over FTP when you want to log in without having an account? anonymous
A typical misconfiguration for running FTP services allows an anonymous account to access the service like any other authenticated user. 
The anonymous username can be input when the prompt appears, followed by any password whatsoever since the service will disregard the password for this specific account.<p>

<p>The operation of FTP services also issue the status for the commands you
are sending to the remote host. The meaning of status updates are as follows:<p>

```
200 : PORT command successful. Consider using PASV.
150 : Here comes the directory listing.
226 : Directory send OK.
230 Login successful.
```
  
An example of a well-known GUI-oriented FTP Service is FileZilla.<br>
What is the name of a handy web site analysis plug-in we can install in our browser? Wappalyzer
