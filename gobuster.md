### Gobuster

Brute-force is a method of submitting data provided through a specially made list of variables known as the wordlist in an attempt to guess the correct input for it to be validated and access to be gained. 
Applying the brute-forcing method to web directories and resources with such a tool will inject the variables from the wordlist one by one, inconsequent requests to the HTTP server,
and then read the HTTP response code for each request to see if it is accepted as an existing resource or not.

```diff
SecLists -> /usr/share/wordlists
git clone https://github.com/danielmiessler/SecLists.git

gobuster --help

```
```
$ gobuster dir --url http://{target_IP}/ --wordlist {wordlist_location}/directory-list-2.3-small.txt

dir : Uses directory/file enumeration mode.
--url : The target URL.
--wordlist : Path to the wordlist.
-x : File extension(s) to search for.
```
