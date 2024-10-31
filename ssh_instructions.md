# SCC SSH Instructions

If you're using SCC machines (in the labs or virtual), open `~/.ssh` and edit `config` in that directory (see below for what should be included) along with adding your ssh key pairs. The permissions for items in `~/.ssh` do have to be quite specific, [here's a summary.](https://superuser.com/a/1559867) 

Remember dotfiles/folders on unix systems are hidden by default so they may not show up in the file browser, but can be seen in on the command line with `ls -al`.

One of the login scripts which run on login on scc managed linux machines is to create a ramdisk with the contents of your `.ssh` folder in your `h-drive` mounted to `~/.ssh` so you can access them via the default ssh agent on any machine. 

The contents of your `~/.ssh/config` should include:

```shell
Host <hostname>  
    Port 6767  
    User <username>  
    IdentityFile ~/.ssh/<your_key.pub>  
    Hostname %h.scc.lancs.ac.uk
```

You should now be able to ssh into the machine using: 
```shell
ssh <hostname>
```

## If the above does not work, try the folowing steps to debug: 

- Confirm the contents of your `~/.ssh folder` looks similar to the following (note the permissions):
```shell
ls -al ~/.ssh
total 120
drwx------  3 <username> a  4096 Oct 31 09:57 .
drwxr-xr-x 30 <username> a  4096 Oct 31 10:45 ..
-rw-------  1 <username> a   689 Oct 16 13:03 authorized_keys
-rw-------  1 <username> a  1720 Oct 30 17:05 config
-rw-------  1 <username> a 35040 Oct 30 17:05 known_hosts
-rw-r--r--  1 <username> a    81 Oct 21 11:45 <public_key>.pub
-r--------  1 <username> a    81 Oct 21 11:45 <private_key>
```
- Confirm you can reach the machine with:

```shell
ping <hostname>.scc.lancs.ac.uk
```

- Confirm the port is open with telnet:

``` shell 
telnet <hostname>.scc.lancs.ac.uk 6767

# should result with something similar to:
Trying <ip address>...
Connected to <hostname>.
Escape character is '^]'.
SSH-2.0-OpenSSH_9.6p1

Invalid SSH identification string.
Connection closed by foreign host. 
```

- Try using ssh commands specifying the whole config:
```shell
ssh <username>@<hostname>.scc.lancs.ac.uk -p 6767 -i <path/to/key/file>
```

- Try using ssh with verbose mode:
```shell
ssh <hostname> -vvv
```

If all else fails, you're welcome to come down to the systems team in A33 Infolab21 and we can give you a hand in person! 