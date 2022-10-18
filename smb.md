### SMB (Server Message Block)

<img src="Screenshot from 2022-10-17 16-38-36.png">

```diff
sudo apt install smbclient -y
+ TCP/445
```

The Transport layer protocol that Microsoft SMB Protocol is most often used with is NetBIOS over TCP/IP (NBT). 
An SMB-enabled storage on the network is called a share.

Smbclient will attempt to connect to the remote host and check if there is any authentication required. 
If there is, it will ask you for a password for your local username. We should take note of this. 
If we do not specify a specific username to smbclient when attempting to connect to the remote host, it will just use your local machine's username. 
That is the one you are currently logged into your Virtual Machine with. 
This is because SMB authentication always requires a username, so by not giving it one explicitly to try to login with, it will just have to pass your current local username to avoid throwing an error with the protocol.

we will be trying to perform is any of the following:

  - Guest authentication
  - Anonymous authentication

```
smbclient -L {target_IP}  
```
