---
description: >-
  Never run any of these tools and techniques on any machine or network without
  proper authorization!
---

# Footprinting & Scanning

## Mapping a Network

These techniques work both on local and remote. Every host connected to the Internet or a private network must have a unique IP address.

Example:

```bash
Block: 200.200.0.0/16
2^16 hosts = 200.200.0.0 - 200.200.255.255
```

* `ping` command tests whether a machine is alive.
* Ping works by sending one or more special ICMP packets (**echo request** - Type 8).
* If the destination host replies with **ICMP echo reply**.
* ICMP is part of the IP protocol.
* `fping` is an improved version of the `ping` utility.
* When running `fping` on a LAN you are directly attached to, even if you use the `-a` option, you will get some warning messages about the offline hosts (`ICMP Host Unreachable`). Those messages are easily removed by: `fping -a g 192.168.82.0 192.168.82.255 2>/dev/null`.

```bash
fping -a -g IPRANGE

# -a option forces the tool to show only alive hosts
# -g option tells the tool we want to perform a ping sweep instead of standard ping

fping -a -g 10.54.12.0/24
fping -a -g 10.54.12.0 10.54.12.255
```

## Nmap Ping Scan

```bash
nmap -sn 200.200.0.0/16
nmap -sn -iL hostilist.txt
```

HOST DISCOVERY:

* `-sL`: List Scan - simply list targets to scan.
* `-sn`: Ping Scan - disable port scan.
* `-Pn`: Treat all hosts as online -- skip host discovery.
* `-PS/PA/PU/PY[portlist]`: TCP SYN/ACK, UDP or SCTP discovery to given ports.
* `-PE/PP/PM`: ICMP echo, timestamp, and netmask request discovery probes.
* `-PO[protocol list]`: IP Protocol Ping.

## OS Fingerprinting

* Possible to identify OS because of some tiny differences in the network stack implementation of the various OS.
* Signature of the host behavior.
* The signature is compared against a database of known OS signatures.
* Offline OS fingerprinting can be done with `p0f` but we'll use `nmap`.

```bash
nmap -Pn -O <target(s)>
# -Pn switch to skip the ping scan if you already know that the targets are alive
```

OS DETECTION:

* `-O`: Enable OS detection.
* `--osscan-limit`: Limit OS detection to promising targets.
* `--osscan-guess`: Guess OS more aggressively.

## Port Scanning

{% hint style="info" %}
**Goals**

* Prepare for the vulnerability assessment phase.
* Perform stealth reconnaissance.
* Detect firewalls.
{% endhint %}

* Port Scanning goes after knowing the active targets on the network.
* Determine what TCP/UDP ports are opened.
* Also knowing what services are running, software and version, on an specific port.
* Port scanners automate probes requests and response analysis.
* Also let you detect if there's a firewall between you and your target.
* 3-way handshake: If port is closed ➝ RST + ACK.

### TCP Connect Scan

* Simplest way to perform a port scan.
* If the scanner receives a `RST` packet, then the port is closed.
* If the scanner is able to complete the connection, then the port is open.
* TCP Connect Scans are recoded in the daemon logs (from the app point of view, the probe looks like a legitimate connection).

### TCP SYN Scan

* Default nmap scan.
* Stealthy by design
* Sends a SYN packet and analyzes the response coming from the target machine.
* If a RST packet is received, then port is closed.
* if a ACK packet is received, then the port is open (and RST packet is sent to the target to stop the handshake).
* Cannot be detected by looking at daemons logs.

### Nmap Scan Types

```bash
-sT # performs a TCP connect scan
-sS # performs a SYN scan
-sV # performs a version detection scan
```

* `-sV` version detection scan mixes a TCP connect scan with some probes, which are used to detect what application is listening on a particular port, which isn't stealthy but useful.
* During version detection scan, Nmap performs a TCP connect and reads from the banner of the daemon listening on a port.
* If the daemon does not send a banner, nmap sends some probes to understand what application is, by studying its behavior

### NMAP Port Scanning

```bash
nmap -sn 192.168.1.0/24 > hosts-up.txt
nmap -sT -p80 192.168.1.0/24              # checks for all webservers in this network range
nmap -sS -sV -p 21 192.168.1.0/24         # checks for service version
```

### Specifying targets

```bash
# By DNS name:
nmap <scan_type> target1.domain.com target2.domain.com

# With an IP address list
nmap <scan_type> 192.168.1.45 200.200.14.56 10.10.1.3

# CIDR notation
nmap <scan_type> 192.168.1.0/24 200.200.1.0/16

# By using wildcards
nmap <scan_type> 192.168.1.*
nmap <scan_type> 10.10.*.1
nmap <scan_type> 200.200.*.*

# Specifying ranges
nmap <scan_types> 200.200.6-12.*

# Octets Lists
nmap <scan_types> 10.14.33.1,3,17
nmap <scan_type> 10.14,20.3.1,3,17,233

# Choosing the ports to scan `-p`:
nmap -p 21,22,139,445,443,80 <target>
nmap -p 100-1000 <target>
```

### Discovering Network with Port Scanning

* You might encounter networks that are protected by firewalls and where pings are blocked.
* It's not uncommon to come across a server that does not respond to pings but has many TCP/UDP ports open.
* `-Pn`: forces the scan on a server.
* If you would like to find an alive host, you can scan typical ports instead of performing a ping sweep.
* The four most basic TCP ports (22, 445, 80, 443) can be used as indicators of live hosts in the network.

### Spotting a Firewall

* You might often see that a version was not recognized regardless of the open port.
* Or even the service type is not recognized.
* `tcpwrapped` means that the TCP handshake was completed but the remote host closed the connection without receiving any data.
* `--reason` nmap flag will show an explanation of why a port is marked as open or closed.

## `masscan`

* Another interesting tool that can help you to discover a network via probing TCP ports.
* Designed to deal with large networks and to scan thousands of IP addresses at once.
* Like `nmap` but a lot faster, however is less accurate.
* Maybe best to use this for host discovery and then conduct a detailed scan with nmap against certain hosts.

```bash
masscan -p22,80,443,53 -Pn --rate=800 --banners 192.168.0.0/24
masscan -p22,80,443,53 -Pn --rate=800 --banners 192.168.0.0/24 --echo > masscan.conf
```

## Examples: Scanning and OS Fingerprinting

```bash
# Perform a Ping Scan with Fping: Run a ping scan on the entire network with fping.
> fping -a -g 10.142.111.0/24 2> /dev/null

# Run a Ping Scan with Nmap
> nmap -sn -n 10.142.111.*

# Run a SYN Scan: This time run nmap only on the alive hosts.
> nmap -sS 10.142.111.1,6,48,96,99,100,213

# Version Detection Scan: Run the version detection scan and spot services running on non-conventional default ports.
> nmap -sV 10.142.111.1,6,48,96,99,100,213

# OS Fingerprinting
> nmap -O 10.142.111.1,6,48,96,99,100,213
```
