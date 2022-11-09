

## Using SSH

SSH can be configured with password authentication or passwordless using [public-key authentication](https://serverpilot.io/docs/how-to-use-ssh-public-key-authentication/) using an SSH public/private key pair. An SSH connection can often be used as a "jump host" to enumerate and attack other hosts in the network, transfer tools, set up persistence, etc.

```bash
ssh Bob@10.10.10.10
```

It is also possible to read local private keys on a compromised system or add our public key to gain SSH access to a specific user. It also provides a way for mapping local ports on the remote machine to our localhost, which can become handy at times.

<br>

## Using Netcat

`Netcat`, `ncat`, or `nc`, is an excellent network utility for interacting with TCP/UDP ports. `Netcat` can be used to connect to any listening port and interact with the service running on that port.

```shell-session
Dareck7@htb[/htb]$ netcat 10.10.10.10 22

SSH-2.0-OpenSSH_8.4p1 Debian-3
```

As we can see, port 22 sent us its banner, stating that `SSH` is running on it. This technique is called `Banner Grabbing`, and can help identify what service is running on a particular port. `Netcat` can also be used to transfer files between machines.

 **`Socat`** can also be used to [upgrade a shell to a fully interactive TTY](https://blog.ropnop.com/upgrading-simple-shells-to-fully-interactive-ttys/#method-2-using-socat). A [standalone binary](https://github.com/andrew-d/static-binaries) of `Socat` can be transferred to a system after obtaining remote code execution to get a more stable reverse shell connection.

<br>

## Using Tmux

```bash
sudo apt install tmux -y
```

Once we have `tmux`, we can start it by entering `tmux` as our command.
The default key to input `tmux` commands prefix is `[CTRL + B]`. In order to open a new window in `tmux`, we can hit the prefix 'i.e. `[CTRL + B]`' and then hit `C`.

We see the numbered windows at the bottom. We can switch to each window by hitting the prefix and then inputting the window number, like `0` or `1`. We can also split a window vertically into panes by hitting the prefix and then `[SHIFT + %]`. We can also split into horizontal panes by hitting the prefix and then `[SHIFT + "]`.

We can switch between panes by hitting the prefix and then the `left` or `right` arrows for horizontal switching or the `up` or `down` arrows for vertical switching.

This [cheatsheet](https://tmuxcheatsheet.com) is a very handy reference.

<br>

## Using Vim

We usually find `Vim` or `Vi` installed on compromised Linux systems.

```bash
vim /etc/hosts
```

If we want to create a new file, input the new file name, and `Vim` will open a new window with that file. Once we open a file, we are in read-only `normal mode`, which allows us to navigate and read the file. To edit the file, we hit `i` to enter `insert mode`, shown by the "`-- INSERT --`" at the bottom of `Vim`. 

We can hit the escape key `esc` to get out of `insert mode`, back into `normal mode`. When we are in `normal mode`, we can use the following keys to perform some useful shortcuts :

| **Command**       |        **Description** | 
| ------------- | ------------------ |
| `x`  |                 Cut character |
| `dw` |                      Cut word |
| `dd` |                 Cut full line |
| `yw` |                     Copy word |
| `yy` |                Copy full line |
| `p`  |                         Paste |

<i>Tip: We can multiply any command to run multiple times by adding a number before it. For example, '4yw' would copy 4 words instead of one, and so on.</i>


If we want to save a file or quit `Vim`, we have to press`:` to go into `command mode`. 

| **Command**       |        **Description** | 
| ------------- | ------------------ |
| `:1` |           Go to line number 1.|
| `:w` |           Write the file, save|
| `:q` |                          Quit |
| `:q!`|           Quit without saving |
| `:wq`|                 Write and quit|

This [cheatsheet](https://vimsheet.com) is an excellent resource for further unlocking the power of `Vim`.
















