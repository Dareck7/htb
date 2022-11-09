
### Nmap

```
nmap -sV --script=banner -p21 10.10.10.0/24
```

### nc
```shell-session
Dareck7@htb[/htb]$ nc -nv 10.129.42.253 21

(UNKNOWN) [10.129.42.253] 21 (ftp) open
220 (vsFTPd 3.0.3)
```