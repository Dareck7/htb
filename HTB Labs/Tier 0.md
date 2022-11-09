

### Telnet

+ TCP/23

```bash
telnet {target_IP}
```



### FTP (File Transfer Protocol)

- TCP/21

The File Transfer Protocol (FTP) is a standard communication protocol used to transfer
computer files from a server to a client on a computer network. FTP users may
authenticate themselves with a clear-text sign-in protocol, generally using a username
and password. However, they can connect anonymously if the server is configured to
allow it.

```bash
sudo apt install ftp -y
```

```bash
ftp {target_IP}
```

<u>FTP Commands :</u>
```
ftp> ?
ftp> ls
ftp> get flag.txt
ftp> bye
```
<p>For secure transmission that protects the username and password and encrypts the content, 
FTP is often secured with SSL/TLS (FTPS) or replaced with SSH-tunneling => SSH File Transfer Protocol (SFTP).</p>
<i>What is username that is used over FTP when you want to log in without having an account? anonymous</i>

A typical misconfiguration for running FTP services allows an anonymous account to access the service like any other authenticated user. The anonymous username can be input when the prompt appears, followed by any password whatsoever since the service will disregard the password for this specific account.

The operation of FTP services also issue the status for the commands you
are sending to the remote host. The meaning of status updates are as follows :

```
200 : PORT command successful. Consider using PASV.
150 : Here comes the directory listing.
226 : Directory send OK.
230 : Login successful.
```
  
An example of a well-known GUI-oriented FTP Service is **FileZilla**.<br>
<i>What is the name of a handy web site analysis plug-in we can install in our browser? Wappalyzer</i>




### Gobuster

Brute-force is a method of submitting data provided through a specially made list of variables known as the wordlist in an attempt to guess the correct input for it to be validated and access to be gained. Applying the brute-forcing method to web directories and resources with such a tool will inject the variables from the wordlist one by one, inconsequent requests to the HTTP server, and then read the HTTP response code for each request to see if it is accepted as an existing resource or not.

```shell-session
SecLists -> /usr/share/wordlists
git clone https://github.com/danielmiessler/SecLists.git
```

```bash
gobuster dir --url http://{target_IP}/ --wordlist {wordlist_location} -x php,html
```

	dir : Uses directory/file enumeration mode.
	--url : The target URL.
	--wordlist : Path to the wordlist.
	-x : File extension(s) to search for.

<i>/usr/share/wordlists/dirbuster/directory-list-2.3-small.txt</i>




### Redis

+ 6379/tcp

Redis (REmote DIctionary Server) is an open-source advanced NoSQL key-value data store used as a database, cache, and message broker. The data is stored in a dictionary format having key-value pairs. It is typically used for short term storage of data that needs fast retrieval. Redis does backup data to hard drives to provide consistency.

```bash
sudo apt install redis-tools
```

```bash
redis-cli -h {target_IP}
```

	-h <hostname> : specify the hostname of the target to connect to


```
# information and statistics about the Redis server:
10.129.120.118:6379> info
# Server
redis_version:5.0.7
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:66bd629f924ac924```


[** SNIP **]

# Keyspace
db0:keys=4,expires=0,avg_ttl=0


# Let us select this Redis logical database:
10.129.120.118:6379> select 0
OK

# We can list all the keys present in the database:
10.129.120.118:6379> keys *
1) "temp"
2) "stor"
3) "numb"
4) "flag"

# we can view the values stored for a corresponding key
10.129.120.118:6379> get <key>
```

<i>The keyspace section provides statistics on the main dictionary of each database. 
The statistics include the number of keys, and the number of keys with an expiration.
In our case, under the Keyspace section, we can see that only one database exists with index 0. </i>

There are different types of databases and one among them is Redis, which is an 'in-memory' database. In-memory databases are the ones that rely essentially on the primary memory for data storage (meaning that the database is managed in the RAM of the system); in contrast to databases that store data on the disk or SSDs. As the primary memory is significantly faster than the secondary memory, the data retrieval time in the case of 'in-memory' databases is very small, thus offering very efficient & minimal response times.

In-memory databases like Redis are typically used to cache data that is frequently requested for quick retrieval. For example, if there is a website that returns some prices on the front page of the site. The website may be written to first check if the needed prices are in Redis, and if not, then check the traditional database (like MySQL or MongoDB). When the value is loaded from the database, it is then stored in Redis
for some shorter period of time (seconds or minutes or hours), to handle any similar requests that arrive during that timeframe. For a site with lots of traffic, this configuration allows for much faster retrieval for the majority of requests, while still having stable long term storage in the main database.

The database is stored in the server's RAM to enable fast data access. Redis also writes the contents of the database to disk at varying intervals to persist it as a backup, in case of failure.





### SMB (Server Message Block)

+ TCP/445

```bash
sudo apt install smbclient -y
```

The Transport layer protocol that Microsoft SMB Protocol is most often used with is NetBIOS over TCP/IP (NBT). An SMB-enabled storage on the network is called a share.

Smbclient will attempt to connect to the remote host and check if there is any authentication required. If we do not specify a specific username to smbclient when attempting to connect to the remote host, it will just use your local machine's username. That is the one you are currently logged into your Virtual Machine with. This is because SMB authentication always requires a username, so by not giving it one explicitly to try to login with, it will just have to pass your current local username to avoid throwing an error with the protocol.

Nevertheless, let us use our local username since we do not know about any remote usernames present on the target host that we could potentially log in with. Next up, after that, we will be prompted for a password. This password is related to the username you input before. In this case, we do not have such credentials, so what We will be trying to perform is any of the following:

  - <i>Guest authentication</i>
  - <i>Anonymous authentication</i>

Any of these will result in us logging in without knowing a proper username/password combination and seeing the files stored on the share. Let us proceed to try that. We leave the password field blank, simply hitting Enter to tell the script to move along.


```bash
smbclient -L {target_IP}  
smbclient \\\\{target_IP}\\WorkShares
```

	   -L : Selecting the targeted host for the connection request
	
<u>SMB Commands :</u>
```
ls : listing contents of the directories within the share
cd : changing current directories within the share
get : downloading the contents of the directories within the share
exit : exiting the smb shell
```

	ADMIN$ - Administrative shares are hidden network shares created by the Windows NT family of operating systems that allow system administrators to have remote access to every disk volume on a network-connected system. These shares may not be permanently deleted but may be disabled.

	C$ - Administrative share for the C:\ disk volume. This is where the operating system is hosted.
	
	IPC$ - The inter-process communication share. Used for inter-process communication via named pipes and is not part of the file system.


We will try to connect to each of the shares except for the IPC$ one, which is not valuable for us since it is not browsable as any regular directory would be and does not contain any files that we could use at this stage of our learning experience.

The NT_STATUS_ACCESS_DENIED is output, letting us know that we do not have the proper credentials to connect to this share. 