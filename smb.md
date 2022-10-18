### SMB (Server Message Block)

<img src="Screenshot from 2022-10-17 16-38-36.png">

```diff
sudo apt install smbclient -y
+ TCP/445
```

The Transport layer protocol that Microsoft SMB Protocol is most often used with is NetBIOS over TCP/IP (NBT). 
An SMB-enabled storage on the network is called a share.

Smbclient will attempt to connect to the remote host and check if there is any authentication required. 
If we do not specify a specific username to smbclient when attempting to connect to the remote host, it will just use your local machine's username.
That is the one you are currently logged into your Virtual Machine with.
This is because SMB authentication always requires a username, so by not giving it one explicitly to try to login with, it will just have to pass your current local username to avoid throwing an error with the protocol.

Nevertheless, let us use our local username since we do not know about any remote usernames present on
the target host that we could potentially log in with. Next up, after that, we will be prompted for a password.
This password is related to the username you input before.
In this case, we do not have such credentials, so what We will be trying to perform is any of the following:

  - Guest authentication
  - Anonymous authentication

Any of these will result in us logging in without knowing a proper username/password combination and
seeing the files stored on the share. Let us proceed to try that. We leave the password field blank, simply
hitting Enter to tell the script to move along.


```diff
smbclient -L {target_IP}  

+   [-L|--list=HOST] : Selecting the targeted host for the connection request
```

```
ADMIN$ - Administrative shares are hidden network shares created by the Windows NT family of operating systems that allow system administrators to have remote access to every disk volume on a network-connected system. These shares may not be permanently deleted but may be disabled.
C$ - Administrative share for the C:\ disk volume. This is where the operating system is hosted.
IPC$ - The inter-process communication share. Used for inter-process communication via named pipes and is not part of the file system.
```
We will try to connect to each of the shares except for the IPC$ one, which is not valuable for us since it is
not browsable as any regular directory would be and does not contain any files that we could use at this
stage of our learning experience.

The NT_STATUS_ACCESS_DENIED is output, letting us know that we do not have the proper credentials to
connect to this share. 
