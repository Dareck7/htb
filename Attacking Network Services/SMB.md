Some SMB versions may be vulnerable to RCE exploits such as [EternalBlue](https://www.avast.com/c-eternalblue).

**Nmap** has many scripts for enumerating SMB, such as <i>smb-os-discovery.nse</i>, which will interact with the SMB service to extract the reported operating system version.

```shell-session
nmap --script smb-os-discovery.nse -p445 10.10.10.40
```


We can run a scan against our target to gather information from the SMB service.

```shell-session
nmap -A -p445 {target_IP}
```



#### Shares

```shell-session
smbclient -N -L \\\\{target_IP}
```

	 -N : suppresses the password prompt
	 -L : retrieve a list of available shares on the remote host


```shell-session
smbclient \\\\{target_IP}\\share
```

<u>Using credentials :</u>
```shell-session
smbclient -U user \\\\{target_IP}\\share
```

<u>SMB Commands :</u>
```shell-session
smb: \> ls
smb: \> cd folder
smb: \> get file.txt
smb: \> exit
```